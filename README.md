# Ansible Apache Automation

This repository contains an Ansible playbook to automate the installation and setup of Apache2 web server on multiple EC2 instances. The playbook is designed to install the latest version of Apache2 and start the service across the specified servers in a simple and efficient manner.

## Table of Contents
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Playbook Breakdown](#playbook-breakdown)
- [Contributing](#contributing)
- [License](#license)

## Project Overview
This project demonstrates the power of Ansible for automation by setting up Apache2 on two slave servers (`slaveServer1` and `slaveServer2`) using a single master server (`masterServer`). The playbook handles both the installation and configuration of Apache2, ensuring it is installed and running on all target machines.

## Prerequisites
- **Ansible**: Installed on the master server (`masterServer`).
- **EC2 Instances**: At least three EC2 instances (one master and two slaves).
- **SSH Key**: A valid SSH key (`ansible2.pem`) for all EC2 instances.

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/ansible-apache-automation.git
   cd ansible-apache-automation
   ```

2. Transfer your SSH key to the master server:
   ```bash
   scp -i "ansible2.pem" ansible2.pem ubuntu@<masterServer_IP>:/home/ubuntu/.ssh
   ```

3. Edit your Ansible hosts file (`/etc/ansible/hosts`) and add the target servers' IP addresses and metadata:
   ```ini
   [servers]
   server1 ansible_host=<server1_IP>
   server2 ansible_host=<server2_IP>

   [servers:vars]
   ansible_python_interpreter=/usr/bin/python3
   ansible_ssh_private_key_file=/home/ubuntu/.ssh/ansible2.pem
   ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   ```

4. Create a directory for playbooks on the master server:
   ```bash
   mkdir -p /home/ubuntu/playbooks
   cd /home/ubuntu/playbooks
   ```

5. Copy the `apache2.yaml` file into the `playbooks` directory.

## Usage
Run the following command to execute the playbook and install Apache2 on the servers:
```bash
ansible-playbook apache2.yaml
```

The playbook will:
- Install Apache2 on both `slaveServer1` and `slaveServer2`.
- Start the Apache2 service and enable it to start on boot.

## Playbook Breakdown
Here is the playbook's structure:

```yaml
---
- name: Install Apache2 on servers
  hosts: servers
  become: yes
  tasks:
    - name: Install Apache2
      apt:
        name: apache2
        state: latest

    - name: Start Apache2
      service:
        name: apache2
        state: started
        enabled: yes
```

- **Install Apache2**: This task installs the Apache2 package using the `apt` module.
- **Start Apache2**: This task ensures the Apache2 service is running and is enabled to start on boot.

## Contributing
Feel free to open issues or submit pull requests if you have suggestions for improvements or additional features.

## License
This project is licensed under the MIT License.
```

This README file provides a clear overview of the project, how to set it up, and explains the Ansible playbook structure. You can modify the `git clone` URL once you've created the repository.
