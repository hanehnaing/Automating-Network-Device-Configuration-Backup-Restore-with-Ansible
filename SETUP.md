# 🚀 Quick Setup Guide

## Step-by-Step Installation

### 1. Prerequisites Check
- [ ] Python 3.8+ installed
- [ ] Git installed
- [ ] SSH client available
- [ ] MikroTik devices accessible via network

### 2. Clone and Setup
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/mikrotik-ansible-automation.git
cd mikrotik-ansible-automation

# Create virtual environment
python3 -m venv ansible_env
source ansible_env/bin/activate  # Linux/macOS
# OR on Windows:
# ansible_env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

### 3. 🔧 Critical Configuration Steps

#### A. Configure Your Network Inventory
1. Copy the example inventory:
   ```bash
   cp hosts.ini.example hosts.ini
   ```

2. Edit `hosts.ini` with your actual network details:
   ```bash
   nano hosts.ini  # or use your preferred editor
   ```

3. Replace ALL placeholder values:
   - IP addresses (192.168.1.x → your actual IPs)
   - Hostnames (router1, router2 → your device names)
   - Python path (/path/to/your/project → actual path)

#### B. Set Up SSH Authentication
1. Generate SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible
   ```

2. Copy public key to each MikroTik device:
   ```bash
   # Transfer the public key file to your MikroTik device, then:
   # On MikroTik RouterOS CLI:
   /user ssh-keys import public-key-file=ansible.pub user=admin
   ```

#### C. Update Ansible Configuration
Edit `ansible.cfg` and update the Python path:
```ini
ssh_executable=/FULL/PATH/TO/YOUR/PROJECT/ansible_env/bin/python -m ansible_pylibssh.ssh
```

### 4. 🧪 Test Your Setup
```bash
# Test connectivity to all devices
ansible mikrotik -m ping

# Test single device
ansible mainrouter -m ping
```

### 5. ⚡ Run Your First Backup
```bash
ansible-playbook Backup.yaml
```

## 🆘 Troubleshooting

### SSH Connection Issues
```bash
# Test manual SSH connection
ssh -i ~/.ssh/ansible admin@YOUR_DEVICE_IP

# Check SSH service on MikroTik
/ip service print
/ip service enable ssh
```

### Python Path Issues
```bash
# Find your actual Python path
which python3
# Update ansible.cfg with the correct path
```

### Permission Issues
```bash
# Fix SSH key permissions
chmod 600 ~/.ssh/ansible
chmod 644 ~/.ssh/ansible.pub
```

## 📋 Pre-Flight Checklist

Before running any playbooks:
- [ ] All IP addresses updated in hosts.ini
- [ ] SSH keys configured and working
- [ ] Python paths updated in configuration files
- [ ] Virtual environment activated
- [ ] Dependencies installed
- [ ] Connectivity tested with `ansible mikrotik -m ping`

## 🎯 Ready to Go!
Once all checks pass, you're ready to automate your MikroTik network! 🚀
