Given your setup, where your project includes **Rasa**, a **custom action service**, a **frontend**, and a **file_server.py**, I'll guide you on setting up a CI/CD pipeline for these components. This approach ensures the entire project is updated, built, and deployed when changes are pushed to your GitHub repository.

---

### **Folder Structure**

Assume your project is structured like this:

```
project/
├── rasa/                # Contains Rasa NLU, Core, and domain files
│   ├── actions/         # Custom action service files
│   ├── data/            # Training data
│   ├── domain.yml
│   ├── config.yml
│   └── endpoints.yml
├── frontend/            # Frontend code (React, Vue, or other)
├── file_server.py       # Python file server script
├── deploy.sh            # Deployment script
├── requirements.txt     # Python dependencies
└── docker-compose.yml   # Docker configuration (optional, for containerization)
```

---

### **Steps to Set Up CI/CD**

#### **1. Create a Deployment Script**

Automate the deployment process using a `deploy.sh` script:

```bash
#!/bin/bash

set -e  # Exit on any error

# Step 1: Pull the latest changes
echo "Pulling latest changes from Git..."
git pull origin main

# Step 2: Activate the virtual environment
echo "Activating virtual environment..."
source /path/to/your/project/venv/bin/activate

# Step 3: Install Python dependencies
echo "Installing Python dependencies..."
pip install -r requirements.txt

# Step 4: Train Rasa model
echo "Training Rasa model..."
cd rasa
rasa train

# Step 5: Restart backend services (Rasa and custom actions)
echo "Restarting backend services..."
sudo systemctl restart rasa
sudo systemctl restart rasa-actions

# Step 6: Build and restart the frontend (if applicable)
echo "Building frontend..."
cd ../frontend
npm install
npm run build

# Step 7: Restart file server (if applicable)
echo "Restarting file server..."
sudo systemctl restart file-server

echo "Deployment completed successfully!"
```

- Ensure the script is executable:
    
    ```bash
    chmod +x deploy.sh
    ```
    

---

#### **2. Set Up GitHub Webhook**

Create a webhook on your GitHub repository to trigger the deployment script.

1. **Install a Webhook Listener on Azure VM**: Install Flask and create a simple listener:
    
    ```bash
    pip install flask
    ```
    
    Create a `webhook_listener.py`:
    
    ```python
    from flask import Flask, request
    import subprocess
    
    app = Flask(__name__)
    
    @app.route('/webhook', methods=['POST'])
    def webhook():
        if request.method == 'POST':
            subprocess.call(['/path/to/your/project/deploy.sh'])
            return 'Deployment triggered!', 200
    
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5000)
    ```
    
    Start the webhook listener:
    
    ```bash
    python webhook_listener.py
    ```
    
2. **Add a Webhook in GitHub**:
    
    - Go to **Settings > Webhooks** in your repository.
    - Add a webhook with:
        - **URL**: `http://<your-azure-vm-ip>:5000/webhook`
        - **Content type**: `application/json`
        - **Trigger**: Push events.

---

#### **3. Configure Backend Services**

Set up **systemd** services to ensure your backend components (Rasa, custom actions, file server) are restarted during deployment.

1. **Rasa Service** (`/etc/systemd/system/rasa.service`):
    
    ```ini
    [Unit]
    Description=Rasa Server
    After=network.target
    
    [Service]
    User=your-user
    WorkingDirectory=/path/to/your/project/rasa
    ExecStart=/path/to/your/project/venv/bin/rasa run --enable-api --cors "*"
    Restart=always
    
    [Install]
    WantedBy=multi-user.target
    ```
    
2. **Custom Actions Service** (`/etc/systemd/system/rasa-actions.service`):
    
    ```ini
    [Unit]
    Description=Rasa Action Server
    After=network.target
    
    [Service]
    User=your-user
    WorkingDirectory=/path/to/your/project/rasa
    ExecStart=/path/to/your/project/venv/bin/rasa run actions
    Restart=always
    
    [Install]
    WantedBy=multi-user.target
    ```
    
3. **File Server Service** (`/etc/systemd/system/file-server.service`):
    
    ```ini
    [Unit]
    Description=File Server
    After=network.target
    
    [Service]
    User=your-user
    WorkingDirectory=/path/to/your/project
    ExecStart=/path/to/your/project/venv/bin/python file_server.py
    Restart=always
    
    [Install]
    WantedBy=multi-user.target
    ```
    
4. **Enable and Start Services**:
    
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable rasa
    sudo systemctl enable rasa-actions
    sudo systemctl enable file-server
    sudo systemctl start rasa
    sudo systemctl start rasa-actions
    sudo systemctl start file-server
    ```
    

---

#### **4. Configure Frontend Deployment**

If your frontend is part of the project (e.g., React, Angular), build it as part of the pipeline:

1. Navigate to the frontend directory in `deploy.sh`:
    
    ```bash
    cd ../frontend
    npm install
    npm run build
    ```
    
2. Deploy the built files to your web server (e.g., Nginx, Apache):
    - Configure Nginx to serve the frontend from the `build/` directory:
        
        ```nginx
        server {
            listen 80;
            server_name your-domain.com;
        
            location / {
                root /path/to/your/project/frontend/build;
                index index.html;
            }
        }
        ```
        

---

#### **5. Test the Pipeline**

1. Push a change to your GitHub repository.
2. Verify the webhook triggers the deployment.
3. Check if the Rasa services, file server, and frontend are updated and restarted.

---

#### **Optional: Use Docker for All Services**

If you want to containerize everything, use `docker-compose.yml` to manage Rasa, actions, file server, and the frontend. This simplifies deployment and CI/CD by rebuilding and restarting containers on each push.

Let me know if you'd like help setting up Docker!