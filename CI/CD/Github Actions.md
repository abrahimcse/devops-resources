# Automate our workflow from idea to production (Github Actions)

**GitHub Actions** allows us to `automate our software workflows` seamlessly, now with powerful CI/CD capabilities. `Easily build, test, and deploy` our code directly from GitHub.
It simplifies code reviews, branch management, and issue triaging, helping teams work more efficiently.

## Benefits 
- No software setup or installation required – directly integrated with GitHub.
- Highly scalable and easy to use.
- Access to pre-built actions and an open-source marketplace for faster automation.

## YAML Syntaxt
### 1️⃣ Basic GitHub Actions YAML Structure
```yml
name: Workflow Name   # (Optional) Name of the workflow

on:                  # Specifies the event that triggers the workflow
  push:              # Runs when code is pushed
    branches:  
      - main  
  pull_request:      # Runs on pull request events
    branches:  
      - main  

jobs:                # Defines one or more jobs
  example-job:  
    runs-on: ubuntu-latest   # Uses Ubuntu runner

    steps:  
      - name: Checkout Repository  
        uses: actions/checkout@v3  # Checks out the code from the repository

      - name: Run a simple script  
        run: echo "Hello, GitHub Actions!"  # Runs a shell command

```
### 2️⃣ Multiple Jobs Example (Build & Deploy)

```yml
name: CI/CD Pipeline  

on: push  # Runs the workflow on every push event

jobs:  
  build:  
    runs-on: ubuntu-latest  
    steps:  
      - name: Checkout Code  
        uses: actions/checkout@v3  # Pulls the latest code

      - name: Setup Node.js  
        uses: actions/setup-node@v3  # Installs Node.js
        with:  
          node-version: 18  

      - name: Install Dependencies  
        run: npm install  # Installs required project dependencies

      - name: Run Tests  
        run: npm test  # Runs unit tests

  deploy:  
    needs: build  # Runs only after the 'build' job completes  
    runs-on: ubuntu-latest  
    steps:  
      - name: Deploy to Server  
        run: echo "Deploying..."  # Placeholder for deployment script
```
### 3️⃣ Self-hosted Runner Example

```yml
name: Deploy to Local VM  

on:  
  push:  
    branches:  
      - main  

jobs:  
  deploy:  
    runs-on: self-hosted  # Uses a self-hosted runner (local machine or VM)
    steps:  
      - name: Checkout Code  
        uses: actions/checkout@v3  # Pulls the latest code

      - name: Restart Service  
        run: |  
          sudo systemctl restart my-app  # Restarts the application service
```
### 4️⃣ Using Secrets & Environment Variables

```yml
name: Secure Workflow  

on: push  

jobs:  
  secure-job:  
    runs-on: ubuntu-latest  
    env:  
      API_URL: https://example.com  # Defines an environment variable

    steps:  
      - name: Use Secret Key  
        run: echo "Secret is ${{ secrets.MY_SECRET_KEY }}"  # Accesses a secret key securely
```

🔹 **Secrets :** Go to GitHub repository → `Settings` → `Secrets and variables` → `Actions` → Add `MY_SECRET_KEY`.

🔹 **Environment Variable :** Set `API_URL` inside `env:` to make it available in the job.

--- 

### Github Actions - Triggers

![Github Actions - Triggers](https://github.com/abrahimcse/devops-resources/blob/main/CI/CD/Images/githubaction.png)