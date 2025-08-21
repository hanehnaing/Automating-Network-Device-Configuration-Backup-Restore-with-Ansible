# MikroTik Network Automation with Ansible

A comprehensive Ansible project for automating MikroTik RouterOS device backup and restore operations with performance monitoring and validation.

## 📋 Overview

This project provides automated solutions for:
- **Bulk Configuration Backups** - Backup multiple MikroTik devices simultaneously
- **Single Device Restore** - Restore specific device configurations with validation
- **Batch Restore Operations** - Restore multiple devices from saved backups
- **Performance Monitoring** - Track backup/restore operations with metrics
- **Error Handling** - Comprehensive validation and error recovery
- **Academic Research Implementation** - Designed for IEEE paper reproducibility

## 🛠️ Prerequisites

### Network Lab Setup
You need either:
1. **GNS3 Lab Environment** - Set up MikroTik RouterOS devices in GNS3 (see setup guide below)
2. **Physical MikroTik Devices** - Real network with accessible MikroTik routers

### Software Requirements
- **Python 3.8+**
- **Ansible Core 2.15+**
- **SSH Key Authentication** configured for MikroTik devices
- **Linux/macOS environment** (Windows with WSL also supported)

## 🏗️ Lab Setup Guide

### Option 1: GNS3 Lab Setup
1. Install GNS3 and import MikroTik RouterOS images
2. Create network topology with multiple RouterOS devices
3. Configure management IP addresses for each device
4. Enable SSH on all RouterOS devices:
   ```
   /ip service enable ssh
   /ip service set ssh port=22
   ```
5. Create admin user with SSH key authentication

### Option 2: Physical Network Setup
1. Ensure all MikroTik devices are accessible via SSH
2. Configure consistent admin credentials across devices
3. Set up SSH key-based authentication
4. Document IP addresses and hostnames

## ⚙️ Installation & Configuration

### 1. Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/mikrotik-ansible-automation.git
cd mikrotik-ansible-automation
```

### 2. Set Up Python Virtual Environment
```bash
python3 -m venv ansible_env
source ansible_env/bin/activate  # On Windows: ansible_env\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

### 4. Configure SSH Keys
```bash
# Generate SSH key pair if you don't have one
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible

# Copy public key to each MikroTik device
# On MikroTik RouterOS:
# /user ssh-keys import public-key-file=ansible.pub user=admin
```

### 5. **IMPORTANT: Update Configuration Files**

#### Update `hosts.ini` with Your Network Details
⚠️ **You MUST modify the inventory file with your actual network configuration:**

```ini
[mikrotik]
# Replace these with YOUR actual devices and IP addresses
your_router1 ansible_host=192.168.1.1  ansible_user=admin ansible_ssh_private_key_file=~/.ssh/ansible
your_router2 ansible_host=192.168.1.2  ansible_user=admin ansible_ssh_private_key_file=~/.ssh/ansible
your_router3 ansible_host=192.168.1.3  ansible_user=admin ansible_ssh_private_key_file=~/.ssh/ansible

[mikrotik:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=community.routeros.routeros
ansible_python_interpreter=/path/to/your/ansible_env/bin/python3
ansible_ssh_type=pylibssh
ansible_persistent_command_timeout=120
```

#### Update `ansible.cfg`
Modify the Python path to match your environment:
```ini
[ssh_connection]
ssh_executable=/path/to/your/ansible_env/bin/python -m ansible_pylibssh.ssh
```

## 🚀 Usage

### Test Connectivity
```bash
ansible mikrotik -m ping
```

### Backup All Devices
```bash
ansible-playbook Backup.yaml
```

### Restore Single Device
```bash
ansible-playbook RestoreOneDevice.yaml
```

### Batch Restore Operations
```bash
ansible-playbook BatchRestore.yaml
```

## 📁 Project Structure

```
├── ansible.cfg                 # Ansible configuration
├── hosts.ini                   # Device inventory (CUSTOMIZE THIS!)
├── requirements.yml            # Ansible collections
├── requirements.txt            # Python dependencies
├── Backup.yaml                 # Main backup playbook
├── RestoreOneDevice.yaml       # Single device restore
├── BatchRestore.yaml          # Batch restore operations
├── RestoreAlDevicel.yaml      # Alternative restore playbook
├── restore_timestamps.yaml    # Timestamp management
├── ansible_env/               # Python virtual environment
└── backups/                   # Backup storage directory
```

## 🔧 Customization

### Adding New Devices
1. Add device entries to `hosts.ini`
2. Ensure SSH connectivity and authentication
3. Test with `ansible new_device -m ping`

### Modifying Backup Location
Update the `backup_base` variable in playbooks:
```yaml
vars:
  backup_base: "/your/preferred/backup/path"
```

### Adjusting Timeouts
Modify timeout values in `ansible.cfg` or individual playbooks for slower networks.

## 🐛 Troubleshooting

### Common Issues
1. **SSH Connection Failed**
   - Verify IP addresses in `hosts.ini`
   - Check SSH key authentication
   - Ensure RouterOS SSH service is enabled

2. **Python Path Errors**
   - Update paths in `ansible.cfg` to match your environment
   - Verify virtual environment activation

3. **Permission Denied**
   - Check SSH key permissions (600)
   - Verify MikroTik user has sufficient privileges

### Debug Mode
Run playbooks with verbose output:
```bash
ansible-playbook -vvv Backup.yaml
```

## 📊 Features

- ✅ **Parallel Operations** - Backup/restore multiple devices simultaneously
- ✅ **Performance Metrics** - Monitor CPU, memory, and execution times
- ✅ **Validation Checks** - Verify backup integrity and device state
- ✅ **Error Recovery** - Graceful handling of network issues
- ✅ **Timestamp Management** - Organized backup versioning
- ✅ **Progress Monitoring** - Real-time operation status

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with your MikroTik setup
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Built for MikroTik RouterOS network automation
- Uses community.routeros Ansible collection
- Inspired by network automation best practices

## ⚠️ Important Notes

- **Always test in a lab environment first**
- **Backup your configurations before running restore operations**
- **This project includes example configurations - adapt to your network**
- **Ensure proper SSH security practices**

---

**Happy Automating! 🚀**

**Author:** Hein Htet Naing  
**Contact:** hhnhhnhhn369@gmail.com  

For questions or issues, please open a GitHub issue or contribute to the project.
