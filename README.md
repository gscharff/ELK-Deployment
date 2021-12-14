## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](Images/Network-Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - elk-install.yml
  - filebeat-playbook.yml
  - metricbeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- A load balancer protects the DVWA web applications by distributing traffic, and maintaining availability in high traffic situations, or when a DVWA machine is down.
- A jump box is used to provide secure access via ssh key into the DVWA machines for system maintenence.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log data and system metrics.
- Filebeat monitors log or other file systems, and sends the data to elasticsearch on the ELK Server.
- Metricbeat monitors system metrics, including CPU and memory usage, and sends them to the ELK Server.

The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.0.0.1   | Linux            |
| DVWA 1   | Web App    | 10.0.0.5   | Linux            |
| DVWA 2   | Web App    | 10.0.0.6   | Linux            |
| ELK      | ELK Stack  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 185.209.176.131

Machines within the network can only be accessed by the Jump Box.
- The Jump Box was the only VM given access to establish an SSH connection to the ELK and DVWA servers.

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses | Allowed Ports              |
|---------------|---------------------|----------------------|----------------------------|
| Jump Box      | Yes                 | 185.209.176.131      | 22 (SSH)                   |
| DVWA 1/2      | No                  | 10.0.0.1             | 22 (SSH)                   |
| ELK           | No                  | 10.0.0.0/24          | 22 (SSH), 5601, 9200, 5044 |
| Load Balancer | Yes                 | *                    | 80 (HTTP)                  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows multiple machines to be configured efficiently and automatically from a centralized location, providing security by limiting access points as well as efficiency in large networks.

The playbook implements the following tasks:
- Install docker.io
- Install pip for python3
- Install docker module for python3 using pip
- Install ELK container and open the relevant ports (5601, 5044, 9200)

The following screenshot displays the result of running `sudo docker ps` after successfully configuring the ELK instance.

![](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DVWA 1/2 (IP 10.0.0.5 and 10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat primarily collects log data, such as that generated by apache. Metricbeat collects system metric data, such as CPU and memory usage.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elk-install.yml file to /etc/ansible/.
- Update the ansible.cfg and hosts files to include ELK Server Information.
- Run the playbook, and navigate to http://[ELK_Public_IP]/app/kibana to check that the installation worked as expected.

_Common Questions_
- _Which file is the playbook?_ elk-install.yml, filebeat-playbook.yml, metricbeat-playbook.yml
- _Where do you copy it?_ Copy to the /et/ansible/ directory in the Ansible container in the Jump Box VM.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ /etc/ansible/hosts
- _Which URL do you navigate to in order to check that the ELK server is running?_ http://[ELK_Public_IP]/app/kibana
