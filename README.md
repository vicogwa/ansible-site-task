# Ansible Website Deployment Project

Automated deployment of HTML5 UP templates to AWS EC2 using Ansible.

## 🎯 Project Overview

This project automates the deployment of professional HTML5 website templates to cloud servers using Ansible. It handles everything from installing dependencies to configuring Nginx web server and setting up SSL certificates.

**Live Demo:** https://storisite.serveftp.com

## 📋 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Available Templates](#available-templates)
- [SSL/HTTPS Setup](#sslhttps-setup)
- [Troubleshooting](#troubleshooting)
- [Maintenance](#maintenance)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- 🚀 **One-Command Deployment**: Deploy complete websites with a single Ansible command
- 🔒 **SSL Support**: Easy Let's Encrypt SSL certificate integration
- 🔄 **Idempotent**: Safe to run multiple times without breaking existing deployments
- 📦 **Automated Setup**: Automatically installs and configures Nginx
- 🛡️ **Security Hardened**: Includes security headers and best practices
- 📊 **Monitoring Ready**: Nginx logs for analytics and monitoring
- 🔧 **Customizable**: Easy to modify templates and configurations

## 🔧 Prerequisites

### Local Machine (Control Node)
- **OS**: Ubuntu/Debian, macOS, or Windows WSL
- **Ansible**: Version 2.9 or higher
- **SSH Client**: OpenSSH or equivalent
- **Python**: Version 3.8 or higher

### Remote Server (Target Node)
- **OS**: Ubuntu 22.04 LTS or 24.04 LTS
- **Access**: SSH access with sudo privileges
- **Ports**: 22 (SSH), 80 (HTTP), 443 (HTTPS) open
- **Minimum Resources**: 
  - 1 vCPU
  - 1GB RAM
  - 10GB disk space

### Cloud Provider Setup
- **AWS EC2 Instance** 
- **Security Group** configured for SSH, HTTP, HTTPS
- **SSH Key Pair** for authentication
- **Public IP Address** or domain name

## 📁 Project Structure

```
ansible-test-app/
├── README.md                          # This file
├── inventory.ini                       # Server inventory
├── playbook.yml                        # Main playbook
├── group_vars/
│   └── webservers.yml                 # Configuration variables
└── roles/
    └── web_template/
        ├── tasks/
        │   └── main.yml               # Deployment tasks
        ├── handlers/
        │   └── main.yml               # Service handlers
        └── templates/
            └── nginx_site.conf.j2     # Nginx configuration template
```

## 🚀 Installation

### Step 1: Install Ansible

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install ansible -y
```

**macOS:**
```bash
brew install ansible
```

**Verify Installation:**
```bash
ansible --version
```

### Step 2: Clone or Create Project

```bash
# Create project directory
mkdir ansible-test-app
cd ansible-test-app

# Create structure
mkdir -p roles/web_template/{tasks,handlers,templates}
mkdir -p group_vars
```

### Step 3: Set Up SSH Keys

```bash
# Generate SSH key (if you don't have one)
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# View your public key
cat ~/.ssh/id_rsa.pub
```

### Step 4: Create Configuration Files

Copy the configuration files from this repository or create them using the examples below.

## ⚙️ Configuration

### 1. Inventory Configuration (`inventory.ini`)

```ini
[webservers]
aws-webserver ansible_host=YOUR_SERVER_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/YOUR_KEY.pem

[webservers:vars]
ansible_python_interpreter=/usr/bin/python3
```

**Replace:**
- `YOUR_SERVER_IP` → Your server's public IP (e.g., `3.148.251.163`)
- `YOUR_KEY.pem` → Your SSH private key path (e.g., `chess-app-key-p.pem`)

### 2. Variables Configuration (`group_vars/webservers.yml`)

```yaml
---
website_domain: yourdomain.com           # Your domain or IP
website_name: "Your Website Name"        # Site title
admin_email: your_email@example.com      # Contact email
template_type: html5up_dimension         # Template to deploy

app_root: /var/www
app_directory: "{{ app_root }}/{{ website_domain }}"

templates:
  html5up_dimension:
    url: "https://html5up.net/dimension/download"
    filename: "dimension.zip"
  html5up_stellar:
    url: "https://html5up.net/stellar/download"
    filename: "stellar.zip"
  html5up_story:
    url: "https://html5up.net/story/download"
    filename: "story.zip"
  html5up_photon:
    url: "https://html5up.net/photon/download"
    filename: "photon.zip"
```

**Customize:**
- `website_domain` → Your domain or server IP
- `website_name` → Your site's display name
- `admin_email` → Your email address
- `template_type` → Choose from available templates

### 3. Main Playbook (`playbook.yml`)

See the complete playbook in the [playbook.yml](playbook.yml) file.

### 4. Role Tasks (`roles/web_template/tasks/main.yml`)

See the complete tasks in the [roles/web_template/tasks/main.yml](roles/web_template/tasks/main.yml) file.

### 5. Handlers (`roles/web_template/handlers/main.yml`)

```yaml
---
- name: reload nginx
  service:
    name: nginx
    state: reloaded

- name: restart nginx
  service:
    name: nginx
    state: restarted
```

### 6. Nginx Template (`roles/web_template/templates/nginx_site.conf.j2`)

See the complete Nginx configuration in the [roles/web_template/templates/nginx_site.conf.j2](roles/web_template/templates/nginx_site.conf.j2) file.

## 🎮 Usage

### Basic Deployment

```bash
# Test connectivity
ansible webservers -m ping

# Check what will change (dry-run)
ansible-playbook playbook.yml --check

# Deploy the website
ansible-playbook playbook.yml

# Deploy with verbose output
ansible-playbook playbook.yml -vv
```

### Deploy Different Templates

```bash
# Edit configuration
nano group_vars/webservers.yml
# Change: template_type: html5up_stellar

# Redeploy
ansible-playbook playbook.yml
```

### Deploy to Multiple Servers

Add more servers to `inventory.ini`:

```ini
[webservers]
server1 ansible_host=1.2.3.4 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/key.pem
server2 ansible_host=5.6.7.8 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/key.pem
```

Then deploy to all:
```bash
ansible-playbook playbook.yml
```

Or deploy to specific server:
```bash
ansible-playbook playbook.yml --limit server1
```

## 🎨 Available Templates

### HTML5 UP Dimension
**Type:** `html5up_dimension`  
**Description:** Modal-based landing page with smooth transitions  
**Best For:** Personal portfolios, agency sites

### HTML5 UP Stellar
**Type:** `html5up_stellar`  
**Description:** Smooth scrolling, single-page design  
**Best For:** Product showcases, portfolios

### HTML5 UP Story
**Type:** `html5up_story`  
**Description:** Full-screen photo story layout  
**Best For:** Photography portfolios, visual storytelling

### HTML5 UP Photon
**Type:** `html5up_photon`  
**Description:** Clean, modern product landing page  
**Best For:** SaaS products, app launches

## 🔒 SSL/HTTPS Setup

### Prerequisites
- Domain name pointing to your server
- Port 443 open in security group

### Automatic SSL with Let's Encrypt

```bash
# SSH to your server
ssh -i ~/.ssh/YOUR_KEY.pem ubuntu@YOUR_SERVER_IP

# Install Certbot
sudo apt update
sudo apt install certbot python3-certbot-nginx -y

# Get SSL certificate
sudo certbot --nginx -d yourdomain.com

# Follow prompts:
# 1. Enter email
# 2. Agree to terms
# 3. Choose option 2 (redirect HTTP to HTTPS)
```

### Verify SSL

```bash
# Check certificate
sudo certbot certificates

# Test renewal
sudo certbot renew --dry-run
```

### Auto-Renewal

Certbot automatically sets up renewal. Verify:

```bash
sudo systemctl status certbot.timer
```

## 🐛 Troubleshooting

### Connection Issues

**Problem:** `ssh: connect to host X.X.X.X port 22: Connection refused`

**Solution:**
1. Check AWS Security Group allows SSH (port 22)
2. Verify instance is running
3. Confirm correct IP address

```bash
# Test SSH manually
ssh -i ~/.ssh/YOUR_KEY.pem ubuntu@YOUR_IP -v
```

### Ansible Ping Fails

**Problem:** `ansible webservers -m ping` fails

**Solution:**
1. Verify SSH works manually first
2. Check inventory.ini has correct IP and key path
3. Ensure key permissions: `chmod 400 ~/.ssh/YOUR_KEY.pem`

```bash
# Debug with verbose output
ansible webservers -m ping -vvv
```

### Website Not Accessible

**Problem:** Can't access website after deployment

**Solution:**
1. Check AWS Security Group allows HTTP (port 80)
2. Verify Nginx is running: `sudo systemctl status nginx`
3. Check nginx logs: `sudo tail -50 /var/log/nginx/error.log`

```bash
# Test from server
ansible webservers -m shell -a "curl -I localhost"
```

### SSL Certificate Fails

**Problem:** Certbot can't get certificate

**Solution:**
1. Verify domain DNS points to server: `nslookup yourdomain.com`
2. Check port 80 is accessible from internet
3. Ensure domain is in nginx config: `cat /etc/nginx/sites-available/yourdomain.com`

```bash
# Test manually
sudo certbot --nginx -d yourdomain.com --dry-run
```

### Template Files Not Copying

**Problem:** `index.html` missing after deployment

**Solution:**
1. Check /tmp/template_download has files
2. Verify copy task in main.yml
3. Check ownership and permissions

```bash
# SSH to server and check
ls -la /var/www/yourdomain.com/
find /tmp/template_download -name "index.html"
```

## 🔧 Maintenance

### Update Website Content

```bash
# SSH to server
ssh -i ~/.ssh/YOUR_KEY.pem ubuntu@YOUR_IP

# Edit content
cd /var/www/yourdomain.com
sudo nano index.html

# No need to reload nginx for HTML changes
```

### Update Configuration

```bash
# On local machine
cd ansible-test-app
nano group_vars/webservers.yml

# Redeploy
ansible-playbook playbook.yml
```

### View Logs

```bash
# Access logs
ansible webservers -m shell -a "tail -100 /var/log/nginx/access.log"

# Error logs
ansible webservers -m shell -a "tail -100 /var/log/nginx/error.log"
```

### Backup Website

```bash
# Create backup
ansible webservers -m shell -a "tar -czf /tmp/backup-$(date +%Y%m%d).tar.gz /var/www/yourdomain.com"

# Download backup
scp -i ~/.ssh/YOUR_KEY.pem ubuntu@YOUR_IP:/tmp/backup-*.tar.gz ./
```

### Renew SSL Certificate

```bash
# Test renewal
sudo certbot renew --dry-run

# Force renewal
sudo certbot renew --force-renewal
```

## 📊 Monitoring

### Check Server Status

```bash
# Server uptime
ansible webservers -m shell -a "uptime"

# Disk usage
ansible webservers -m shell -a "df -h"

# Memory usage
ansible webservers -m shell -a "free -h"

# Nginx status
ansible webservers -m shell -a "systemctl status nginx"
```

### Website Analytics

Access logs contain visitor information:

```bash
# View recent visitors
ansible webservers -m shell -a "tail -100 /var/log/nginx/access.log"

# Count unique IPs
ansible webservers -m shell -a "awk '{print \$1}' /var/log/nginx/access.log | sort | uniq | wc -l"
```

## 🚀 Advanced Usage

### Custom Templates

Add your own template to `group_vars/webservers.yml`:

```yaml
templates:
  my_custom_template:
    url: "https://example.com/template.zip"
    filename: "custom.zip"
```

Then use:
```yaml
template_type: my_custom_template
```

### Multi-Environment Setup

Create different inventory files:

**inventory-production.ini:**
```ini
[webservers]
prod ansible_host=X.X.X.X ...
```

**inventory-staging.ini:**
```ini
[webservers]
staging ansible_host=Y.Y.Y.Y ...
```

Deploy to specific environment:
```bash
ansible-playbook playbook.yml -i inventory-production.ini
```

### Ansible Vault for Secrets

Encrypt sensitive data:

```bash
# Create encrypted file
ansible-vault create secrets.yml

# Add secrets
admin_password: mypassword123
api_key: abc123xyz

# Use in playbook
ansible-playbook playbook.yml --ask-vault-pass
```

## 📝 Best Practices

1. **Version Control**: Keep playbooks in Git
2. **Test First**: Always use `--check` before production deployment
3. **Backups**: Regular backups before major changes
4. **Monitoring**: Set up uptime monitoring (UptimeRobot, Pingdom)
5. **Updates**: Keep server packages updated
6. **Security**: Use strong passwords, keep SSH keys secure
7. **Documentation**: Document custom changes
8. **Logs**: Regularly check nginx logs for errors

## 📈 Performance Optimization

### Enable Gzip Compression

Already included in nginx template:
```nginx
gzip on;
gzip_types text/plain text/css text/xml text/javascript application/javascript;
```

### Browser Caching

Already configured:
```nginx
location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
    expires 7d;
}
```

### CDN Integration

For high-traffic sites, consider adding Cloudflare:
1. Sign up for Cloudflare (free)
2. Point domain nameservers to Cloudflare
3. Enable CDN features

## 🤝 Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 👤 Author

**Victoria Francis**
- Email: victoriafrancis885@gmail.com
- Website: https://storisite.serveftp.com

## 🙏 Acknowledgments

- [HTML5 UP](https://html5up.net) for beautiful free templates
- [Ansible](https://www.ansible.com) for automation platform
- [Let's Encrypt](https://letsencrypt.org) for free SSL certificates
- [Nginx](https://nginx.org) for web server

## 📚 Additional Resources

- [Ansible Documentation](https://docs.ansible.com)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [HTML5 UP Templates](https://html5up.net)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)

## 🆘 Support

For issues and questions:
1. Check [Troubleshooting](#troubleshooting) section
2. Review [Common Issues](https://github.com/yourusername/ansible-website-deploy/issues)
3. Open a new issue with:
   - Operating system
   - Ansible version
   - Error messages
   - Steps to reproduce

## 🔄 Changelog

### Version 1.0.0 (2026-01-06)
- Initial release
- Support for HTML5 UP templates
- Nginx configuration
- SSL/Let's Encrypt integration
- AWS EC2 deployment tested

---

**Note:** Replace placeholder values (YOUR_SERVER_IP, yourdomain.com, etc.) with your actual configuration before deployment.

**Security Warning:** Never commit private keys or sensitive credentials to version control. Use `.gitignore` to exclude:
```
*.pem
*.key
secrets.yml
```

---

Made with ❤️ using Ansible
