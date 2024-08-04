# ansible-devops-assessment
ansible-devops-assessment

# Example Application Deployment

### Overview
This repository contains the necessary configurations and instructions to deploy the Example application using Gunicorn, within a Python virtual environment, and managed by systemd on a Linux-based system.

### Prerequisites
Operating System: Linux (Ubuntu/Debian recommended)
Python: Python 3.8 or later
Systemd: For managing the service
Git: For version control
Ansible: For automating deployment (if applicable)
Getting Started
Clone the Repository

### Control Machine Setup

# Clone the Repository
raju@ansible-control:~/ansible-devops-assessment$ git clone https://github.com/SRK-RAJU/ansible-devops-assessment.git

# Install Ansible
sudo apt update
sudo apt install ansible

# File Structure of the Assignment: Control Machine

ansible_project/
├── group_vars/
│   └── all.yml

├── roles/
│   ├── ca_certs/
│   │   ├── files/
│   │   │   ├── CA1.crt
│   │   │   ├── CA2.crt
│   │   │   ├── CA3.crt
│   │   │   ├── CA1.key
│   │   │   ├── CA2.key
│   │   │   ├── CA3.key
│   │   ├── tasks/
│   │   │   └── main.yml

│   ├──deploy_app/
│   │   ├── files/
│   │   │   ├── Example-1.1.1-py3-none-any.whl
│   │   │   ├── app.py
│   │   │   ├── run.sh
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   ├── config.py.j2
│   │   │   └── example.service.j2

├── inventory.ini
├── playbook.yml
└── .git

# Configure Ansible Inventory
Edit the inventory file to include your target machine's IP address:
[target]
your-target-ip ansible_user=your-username
# Create Ansible Playbook
# create ansible vault.yml and securely parametarized the variables passing.
# Make Script Executable:
chmod +x /opt/example/run.sh

# run the playbook at control machine 

raju@ansible-control:~/ansible-devops-assessment$ ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass 

Vault password:


# Target Machine Setup
Install Required Packages and Ensure that the target machine has the necessary packages installed:

sudo apt update
sudo apt install python3 python3-venv python3-pip

# Verify Virtual Environment and Packages Activate Virtual Environment:

source /opt/example/venv/bin/activate

# Check Installed Packages:

pip list

# Check Gunicorn Installation Verify that Gunicorn is installed correctly:

ls -l /opt/example/venv/bin/gunicorn

# Verify Service Status To check the status of the service:

sudo systemctl status example.service

# Troubleshooting
# Permission Issues: If you encounter permission errors, ensure the following:

sudo chown -R your-username:your-group /opt/example

Dependency Issues: If pip install fails due to permission errors, use sudo for installing packages or adjust the virtual environment permissions.

Gunicorn Errors: If you see errors related to Gunicorn, verify the run.sh script and the systemd service file configuration. Ensure that the bind address in the run.sh script is correctly specified.

Systemd Service Issues: If the service fails to start, check the logs:

# journalctl -u example.service

# Successful Results

After following these steps, the example.service should be active and running. The logs should indicate that Gunicorn is listening on the specified port without errors:

sudo systemctl status example.service

The output should show that the service is active and running, with no errors related to the Gunicorn process.
