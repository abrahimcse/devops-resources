# 🖥️ Node Exporter Installation using Ansible for Prometheus & Grafana Monitoring

This guide covers how to automate the installation and configuration of **Node Exporter** on multiple production servers using **Ansible**. This setup supports monitoring with **Prometheus** and **Grafana**.

---

## 📦 Requirements

- Ansible installed on your control node
- SSH access to all target servers using a private key
- Ubuntu-based target servers
- Internet access on the target machines

---

## 📁 Project Structure

```
automations/
├── inventory.ini
└── node_exporter.yml
```

---

## 🔧 Configuration Steps

### 1. Configure AWS CLI (Optional)

If you're using AWS infrastructure. Configure your AWS credentials:

```bash
aws configure
```

You’ll be prompted to enter:

```
AWS Access Key ID [None]: YOUR_ACCESS_KEY
AWS Secret Access Key [None]: YOUR_SECRET_KEY
Default region name [None]: us-east-1
Default output format [None]: json
```

🔒 **Credentials File Location:**

Your credentials will be saved at:

```bash
~/.aws/credentials
cat ~/.aws/credentials
```

Example content:

```ini
[default]
aws_access_key_id = AKIAxxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

### 2. Create Directory for Automation

```bash
mkdir automations
cd automations
```

---

### 3. Create Ansible Inventory File

Create `inventory.ini`:

```
vim inventory.ini
```

```ini
[prod_servers]
prod-k8s-master ansible_host=example.master.ip ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/private-key.pem
prod-k8s-worker-1 ansible_host=example.worker1.ip ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/private-key.pem
prod-k8s-worker-2 ansible_host=example.worker2.ip ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/private-key.pem
prod-kafka ansible_host=example.kafka.ip ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/private-key.pem
prod-keycloak ansible_host=example.keycloak.ip ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/private-key.pem
```

> Replace `example.*.ip` with actual server IPs and `/path/to/private-key.pem` with your private key path.
> Example : prod-k8s-master ansible_host=10.11.8.178 ansible_user=ubuntu ansible_ssh_private_key_file=/ctech/aws-key/hsms-prod-common.pem
---

### 4. Test Connectivity

Run the following to verify Ansible can connect to all servers:

```bash
ansible all -m ping -i inventory.ini
```

---

### 5. Create the Node Exporter Playbook

Create a file named `node_exporter.yml` and add the following playbook:
```
vim node_exporter.yml
```

```yaml
---
---
- name: Install and configure Node Exporter
  hosts: all
  become: yes
  vars:
    node_exporter_version: "1.6.1"
    node_exporter_user: "node_exporter"
    node_exporter_service: "/etc/systemd/system/node_exporter.service"

  tasks:
    - name: Create node_exporter user
      ansible.builtin.user:
        name: "{{ node_exporter_user }}"
        shell: /usr/sbin/nologin

    - name: Download Node Exporter
      ansible.builtin.get_url:
        url: "https://github.com/prometheus/node_exporter/releases/download/v{{ node_exporter_version }}/node_exporter-{{ node_exporter_version }}.linux-amd64.tar.gz"
        dest: /tmp/node_exporter.tar.gz

    - name: Extract Node Exporter
      ansible.builtin.unarchive:
        src: /tmp/node_exporter.tar.gz
        dest: /usr/local/bin/
        remote_src: yes

    - name: Move binary to proper location
      ansible.builtin.command:
        cmd: mv /usr/local/bin/node_exporter-{{ node_exporter_version }}.linux-amd64/node_exporter /usr/local/bin/
      notify:
        - Restart node_exporter

    - name: Set ownership and permissions for the binary
      ansible.builtin.file:
        path: /usr/local/bin/node_exporter
        owner: "{{ node_exporter_user }}"
        group: "{{ node_exporter_user }}"
        mode: '0755'

    - name: Create systemd service file
      ansible.builtin.copy:
        dest: "{{ node_exporter_service }}"
        content: |
          [Unit]
          Description=Node Exporter
          Wants=network-online.target
          After=network-online.target

          [Service]
          User={{ node_exporter_user }}
          ExecStart=/usr/local/bin/node_exporter
          Restart=always

          [Install]
          WantedBy=default.target
      notify:
        - Reload systemd

    - name: Enable and start Node Exporter service
      ansible.builtin.systemd:
        name: node_exporter
        enabled: yes
        state: started

  handlers:
    - name: Reload systemd
      ansible.builtin.systemd:
        daemon_reload: yes

    - name: Restart node_exporter
      ansible.builtin.systemd:
        name: node_exporter
        state: restarted
```

---

### 6. Run the Playbook

Execute the playbook to install Node Exporter on all servers:

```bash
ansible-playbook node_exporter.yml -i inventory.ini
```

---

## ✅ Verification

### Check the Service

On any target server:

```bash
sudo systemctl status node_exporter
```

### Verify Metrics Endpoint

Visit in your browser or use curl:

```bash
http://<your-server-ip>:9100/metrics
```

---

## 📈 Next Step: Configure Prometheus

Add the targets in Prometheus configuration file (`prometheus.yml`):

```yaml
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['server1:9100', 'server2:9100', 'server3:9100']
```

---

## 📌 Notes

- You can customize the `inventory.ini` to include other environments like `staging`, `dev`, etc.
- Update `node_exporter_version` in the playbook to install a different version.
- Ensure the target servers allow inbound traffic on port `9100`.

