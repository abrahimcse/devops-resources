#  Monitoring Tools Setup Guide

## Tools Used:

- **Grafana :** Visualization dashboard.
- **Prometheus :** Monitoring and alerting system.
- **Node Exporter :** Exposes system metrics.

### Prerequisites:

1. [Docker](https://github.com/abrahimcse/devops-resources/blob/main/Docker/1.Docker%20Basic.md) and [Docker Compose](https://github.com/abrahimcse/devops-resources/blob/main/Docker/Docker%20Compose.md) must be installed.
2. Basic understanding of YAML files and Linux commands.

---

## Step 1: Configure Docker Compose File

1. Create a `docker-compose.yml` file:

   ```bash
   vim docker-compose.yml
   ```

2. Add the following content to the file:

   ```yaml
   networks:
     monitoring:
       driver: bridge

   volumes:
     prometheus_data: {}

   services:
     node-exporter:
       image: prom/node-exporter:latest
       container_name: node-exporter
       restart: unless-stopped
       volumes:
         - /proc:/host/proc:ro
         - /sys:/host/sys:ro
         - /:/rootfs:ro
       command:
         - '--path.procfs=/host/proc'
         - '--path.rootfs=/rootfs'
         - '--path.sysfs=/host/sys'
         - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'

       networks:
         - monitoring

     prometheus:
       image: prom/prometheus:latest
       container_name: prometheus
       restart: unless-stopped
       volumes:
         - ./prometheus.yml:/etc/prometheus/prometheus.yml
         - prometheus_data:/prometheus
       command:
         - '--config.file=/etc/prometheus/prometheus.yml'
         - '--storage.tsdb.path=/prometheus'
         - '--web.console.libraries=/etc/prometheus/console_libraries'
         - '--web.console.templates=/etc/prometheus/consoles'
         - '--web.enable-lifecycle'

       networks:
         - monitoring

       ports:
         - "9090:9090"
   ```

---

## Step 2: Create Prometheus Configuration File

1. Create the `prometheus.yml` file:

   ```bash
   vim prometheus.yml
   ```

2. Add the following configuration:

   ```yaml
   global:
     scrape_interval: 10s

   scrape_configs:
     - job_name: 'aws-server-1'
       static_configs:
         - targets: ['node-exporter:9100']
   ```

---

## Step 3: Start the Services

1. Run Docker Compose to start all services:
   ```bash
   docker-compose up -d --build
   ```
2. Verify the services are running:
   ```bash
   docker ps
   ```

---

## Step 4: Install Grafana

1. Run the Grafana container:
   ```bash
   docker run -d --name=grafana -p 3000:3000 grafana/grafana
   ```
2. Access Grafana:
   - Open your browser and go to: `http://<server-ip>:3000`

---

## Step 5: Configure Grafana with Prometheus

1. Log in to Grafana (default credentials: admin/admin).
2. Navigate to **Home -> Connections -> Data sources**.
3. Add a new data source:
   - Select **Prometheus**.
   - Set the URL to `http://<server-ip>:9090/`.
   - Click **Save & Test**.

---

## Step 6: Extracting Dashboard ID from Grafana Links
When you are given a Grafana dashboard link (e.g., https://grafana.com/grafana/dashboards/1860-node-exporter-full/), follow these steps to find and use the **Dashboard ID:**
1. Open the provided link in your browser:
    ```ruby
    https://grafana.com/grafana/dashboards/1860-node-exporter-full/
    ```
2. Look at the URL carefully:
- The Dashboard ID is the number directly following /dashboards/.
    ```makefile
    ID = 1860
    ```
3. Copy the **Dashboard ID**
- Highlight the ID (`1860`) in the URL.
## Step 7: Import Grafana Dashboard

1. Go to Grafana **Dashboards**.
2. Click on **Import**.
3. Enter Dashboard ID: `1860` (Node Exporter Full).
4. Click **Load**.
5. Select the Prometheus data source and click **Import**.

---

## Step 8: Useful Linux Commands

- **Monitor Processes:**
  ```bash
  nrpoc
  ```
- **Check Memory Usage:**
  ```bash
  free -h
  ```
- **View CPU Info:**
  ```bash
  cat /proc/cpuinfo
  ```
- **Filter Processes from CPU Info:**
  ```bash
  cat /proc/cpuinfo | grep process
  ```
- **View Memory Info:**
  ```bash
  cat /proc/meminfo
  ```

---

## Verification

- Access Prometheus: `http://<server-ip>:9090`
- Access Grafana: `http://<server-ip>:3000`
- Check the imported Grafana dashboard to ensure metrics are displayed correctly.

---

## Notes

1. Ensure Docker is installed and running on your system.
2. Replace `<server-ip>` with your actual server IP address.
3. Adjust the Prometheus scrape interval and targets as needed for your environment.

--- 
![](https://github.com/abrahimcse/devops-resources/blob/main/Monitoring-Tools/Images/Grafana.png)