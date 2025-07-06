# Python Flask Project on Red Hat 8

This repository contains a simple Flask web application that can be deployed on a Red Hat 8 server using Gunicorn and Nginx.

## 📋 Prerequisites

- Red Hat 8-based system
- Root or sudo access
- Internet connectivity

---

## 🛠️ 1. Environment Setup

### Disable SELinux:
```bash
setenforce 0
Stop and Disable Firewall (temporarily for setup):
bash
Copy
Edit
systemctl stop firewalld
systemctl disable firewalld
🐍 2. Install Python and Pip
Install Python 3 and Pip:
bash
Copy
Edit
yum install python3 python3-pip -y
Verify Python Installation:
bash
Copy
Edit
python3 --version
📁 3. Create Project Directory
bash
Copy
Edit
mkdir mypythonproject
cd mypythonproject
🔒 4. Set Up Virtual Environment
Create and Activate Virtual Environment:
bash
Copy
Edit
python3 -m venv venv
source venv/bin/activate
🌐 5. Create Flask Application
Create a file named app.py with the following content:

python
Copy
Edit
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, World! This is my Python project running on Red Hat 8."

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
📦 6. Install Flask and Gunicorn
Install Flask:
bash
Copy
Edit
pip install Flask
Upgrade Pip:
bash
Copy
Edit
python3 -m pip install --upgrade pip
Install Gunicorn:
bash
Copy
Edit
pip install gunicorn
▶️ 7. Run Flask Application
Development Server (for testing):
bash
Copy
Edit
python app.py
Production Server with Gunicorn:
bash
Copy
Edit
gunicorn --bind 0.0.0.0:5000 app:app
⚙️ 8. Set Up systemd Service
Create a file at /etc/systemd/system/myflaskapp.service:

ini
Copy
Edit
[Unit]
Description=Gunicorn instance to serve my Flask app
After=network.target

[Service]
User=root
Group=root
WorkingDirectory=/root/mypythonproject
ExecStart=/root/mypythonproject/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:5000 app:app

[Install]
WantedBy=multi-user.target
Enable and Start the Service:
bash
Copy
Edit
systemctl daemon-reload
systemctl start myflaskapp
systemctl enable myflaskapp
Check Service Status:
bash
Copy
Edit
systemctl status myflaskapp
🌐 9. Install and Configure Nginx
Install Nginx:
bash
Copy
Edit
yum install nginx -y
Start and Enable Nginx:
bash
Copy
Edit
systemctl start nginx
systemctl enable nginx
Configure Nginx:
Edit /etc/nginx/nginx.conf and add the following under the http block:

nginx
Copy
Edit
server {
    listen 80;
    server_name your_domain_or_ip;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
Test and Restart Nginx:
bash
Copy
Edit
nginx -t
systemctl restart nginx
🔐 10. Re-enable Firewall and Open Ports
Start and Enable Firewall:
bash
Copy
Edit
systemctl start firewalld
systemctl enable firewalld
Open Port 80:
bash
Copy
Edit
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --reload
✅ 11. Verify Setup
Open a browser and visit http://your_domain_or_ip to see the message:

Hello, World! This is my Python project running on Red Hat 8.

🕒 12. Command History
To review your shell command history:

bash
Copy
Edit
history
