Ansible Website Deployment (HTML5 UP – Story)

This project uses Ansible to automatically deploy a single HTML5 UP template (Story) to an AWS EC2 server with Nginx.

Live Demo: https://storisite.serveftp.com

📌 Project Overview

The goal of this project is to show how Ansible can be used to:

Connect to an EC2 server

Install and configure Nginx

Deploy a static website automatically

Make the process repeatable and simple

Only one template was used for this project:
👉 HTML5 UP – Story

✨ Features

One-command website deployment using Ansible

Automated Nginx installation and configuration

Static website hosting on AWS EC2

Idempotent playbook (safe to run multiple times)

Beginner-friendly Ansible structure

🧰 Tech Stack

Ansible

AWS EC2 (Ubuntu)

Nginx

HTML5 UP – Story template

SSH

📁 Project Structure
ansible-test-app/
├── README.md
├── inventory.ini
├── playbook.yml
├── group_vars/
│   └── webservers.yml
└── roles/
    └── web_template/
        ├── tasks/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        └── templates/
            └── nginx_site.conf.j2

🔧 Prerequisites
Local Machine

Ansible installed

SSH access

Python 3

Server

AWS EC2 (Ubuntu 22.04 or 24.04)

Open ports:

22 (SSH)

80 (HTTP)

SSH key access

⚙️ Configuration
Inventory (inventory.ini)
[webservers]
web ansible_host=YOUR_SERVER_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/YOUR_KEY.pem

Variables (group_vars/webservers.yml)
website_domain: storisite.serveftp.com
website_name: "Story Website"
app_root: /var/www
app_directory: "{{ app_root }}/{{ website_domain }}"

🚀 Deployment
Test Connection
ansible webservers -m ping

Deploy Website
ansible-playbook playbook.yml


That’s it.
After deployment, the site will be live using Nginx.

🎨 Template Used

HTML5 UP – Story

Full-screen layout

Clean and simple design

Ideal for storytelling or portfolio websites

Template source: https://html5up.net/story

🛠️ Common Checks
Check Nginx Status
sudo systemctl status nginx

View Website Files
ls /var/www/storisite.serveftp.com

Check Logs
sudo tail /var/log/nginx/error.log

🎯 Why This Project?

This project demonstrates:

Infrastructure automation with Ansible

Real-world server configuration

Cloud deployment using AWS

DevOps best practices for repeatable deployments

It is suitable for:

DevOps / Cloud Engineering portfolios

Technical assessments

REG / internship surveys

👤 Author

Victoria Francis
📧 Email: victoriafrancis885@gmail.com

🌐 Website: https://storisite.serveftp.com

📄 License

MIT License

🙏 Acknowledgments

HTML5 UP for the Story template

Ansible for automation

Nginx for web hosting

AWS EC2 for cloud infrastructure
