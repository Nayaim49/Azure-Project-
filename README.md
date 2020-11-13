# Automated ELK Stack Deployment

This document contains the following details:
- Description of the Topology
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build
- Access Policies

### Description of the Topology
This repository includes code defining the infrastructure below. 

![](Azure Resource_Group_Project.png)

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the "D*mn Vulnerable Web Application"

Load balancing ensures that the application will be highly **available**, in addition to restricting **inbound access** to the network. The load balancer ensures that work to process incoming traffic will be shared by both vulnerable web servers. Access controls will ensure that only authorized users — namely, ourselves — will be able to connect in the first place.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **file systems of the VMs on the network**, as well as watch **system metrics**, such as CPU usage; attempted SSH logins; `sudo` escalation failures; etc.

The configuration details of each machine may be found below.

| Name     |   Function  | IP Address | Operating System |
|----------|-------------|------------|------------------|
| Jump Box | Gateway     | 10.0.0.4   | Linux            |
| DVWA 1   | Web Server  | 10.0.0.5   | Linux            |         |
| ELK      | Monitoring  | 10.2.0.4   | Linux            |

In addition to the above, Azure has provisioned a **load balancer** in front of all machines except for the jump box. The load balancer's targets are organized into the following availability zones:
- **Availability Zone 1**: DVWA 1
- **Availability Zone 2**: ELK

## ELK Server Configuration
The ELK VM exposes an Elastic Stack instance. **Docker** is used to download and manage an ELK container.

Rather than configure ELK manually, we opted to develop a reusable Ansible Playbook to accomplish the task. This playbook is duplicated below.


To use this playbook, one must log into the Jump Box, then issue: `ansible-playbook install_elk.yml elk`. This runs the `install_elk.yml` playbook on the `elk` host.

### Access Policies
The machines on the internal network are _not_ exposed to the public Internet. 

Only the **jump box** machine can accept connections from the Internet. Access to this machine is only allowed from the IP address 20.55.33.199


Machines _within_ the network can only be accessed by **each other**. The DVWA 1 and DVWA 2 VMs send traffic to the ELK server.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 20.185.1.145        |
| ELK      | No                  | 10.0.0.1-254         |
| DVWA 1   | No                  | 10.0.0.1-254         |
      

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this allows for easy replication and portability. Also, by automating the installation process we could deploy multiple servers easily and quickly without having to physically touch each server.

The playbook implements the following tasks:
.Install docker
.Install ELK docker Container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
Screenshot display

Target Machines & Beats

This ELK server is configured to monitor the following machines: Web-1 and Web-2 (Both running DVWA)
TODO: List the IP addresses of the machines you are monitoring
We have installed the following Beats on these machines:
TODO: Specify which Beats you successfully installed
Filebeat
Metricbeat
These Beats allow us to collect the following information from each machine:
TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.
Filebeat watches for log files, locations and collect log events.
Metricbeat records metrics and statistical data from the operating system and from servers runnings on the server
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
Update the configuuration files to include the Private IP of the ELK-Server to the ElasticSearch and Kibana Sections of the Configuration File
Run the playbook, and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.
TODO: Answer the following questions to fill in the blanks:
Which file is the playbook? Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
The playbook files are:
elk-playbook.yml - used to install ELK server
filebeat-playbook.yml - Used to install and configure Filebeat on ELK Server and DVWA servers
metricbeat-playbook.yml - Used to install and Configure Metricbeat on ELK Server and DVWA servers
Where do you copy it : /etc/ansible
Which file do you update to make Ansible run the playbook on a specific machine? : /etc/ansible/hosts.cfg
How do I specify which machine to install the ELK server on versus which to install Filebeat on?: In /etc/ansible/hosts user writes instruction where to install the ELK servers or Filebeat
_Which URL do you navigate to in order to check that the ELK server is running? http://104.42.189.57:5601/app/kibana#/home/tutorial/systemLogs
As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
Commands needed to run the Ansible Configuration for the Elk-Server are:
1.ssh RedAdmin@JumpBox (Private IP)
2.sudo docker container list -a (to locate the ansible container)
3.sudo docker start (e.g.silly_bouman)
4.sudo docker attach (e.g.silly_bouman)
5.cd /etc/ansible/
ansible-playbook elk-playbook.yml (Installs and Configure Beats)
cd /etc/ansible/
ansible-playbook beats-playbook.yml (Installs and Vonfigure Beats)
Open a new browser on personal workstation, navigate to (ELK-Server-Public:5601/app/kibana) - This will bring up Kibana Web Portal  
Congratulations! You have an ELK Stack Server monitoring system logs on two web servers


The playbook is duplicated below.
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the "D*mn Vulnerable Web Application"

Load balancing ensures that the application will be highly **available**, in addition to restricting **inbound access** to the network. The load balancer ensures that work to process incoming traffic will be shared by both vulnerable web servers. Access controls will ensure that only authorized users — namely, ourselves — will be able to connect in the first place.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **file systems of the VMs on the network**, as well as watch **system metrics**, such as CPU usage; attempted SSH logins; `sudo` escalation failures; etc.

The configuration details of each machine may be found below.

| Name     |   Function  | IP Address | Operating System |
|----------|-------------|------------|------------------|
| Jump Box | Gateway     | 10.0.0.4   | Linux            |
| DVWA 1   | Web Server  | 10.0.0.5   | Linux            |         |
| ELK      | Monitoring  | 10.2.0.4   | Linux            |

In addition to the above, Azure has provisioned a **load balancer** in front of all machines except for the jump box. The load balancer's targets are organized into the following availability zones:
- **Availability Zone 1**: DVWA 1
- **Availability Zone 2**: ELK

## ELK Server Configuration
The ELK VM exposes an Elastic Stack instance. **Docker** is used to download and manage an ELK container.

Rather than configure ELK manually, we opted to develop a reusable Ansible Playbook to accomplish the task. This playbook is duplicated below.


To use this playbook, one must log into the Jump Box, then issue: `ansible-playbook install_elk.yml elk`. This runs the `install_elk.yml` playbook on the `elk` host.

### Access Policies
The machines on the internal network are _not_ exposed to the public Internet. 

Only the **jump box** machine can accept connections from the Internet. Access to this machine is only allowed from the IP address 20.55.33.199


Machines _within_ the network can only be accessed by **each other**. The DVWA 1 and DVWA 2 VMs send traffic to the ELK server.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 20.185.1.145        |
| ELK      | No                  | 10.0.0.1-254         |
| DVWA 1   | No                  | 10.0.0.1-254         |
      

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this allows for easy replication and portability. Also, by automating the installation process we could deploy multiple servers easily and quickly without having to physically touch each server.

The playbook implements the following tasks:

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
Screenshot display

Target Machines & Beats
This ELK server is configured to monitor the following machines: Web-1 and Web-2 (Both running DVWA)
TODO: List the IP addresses of the machines you are monitoring
We have installed the following Beats on these machines:
TODO: Specify which Beats you successfully installed
Filebeat
Metricbeat
These Beats allow us to collect the following information from each machine:
TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.
Filebeat watches for log files, locations and collect log events.
Metricbeat records metrics and statistical data from the operating system and from servers runnings on the server
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
Update the configuuration files to include the Private IP of the ELK-Server to the ElasticSearch and Kibana Sections of the Configuration File
Run the playbook, and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.
TODO: Answer the following questions to fill in the blanks:
Which file is the playbook? Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
The playbook files are:
elk-playbook.yml - used to install ELK server
filebeat-playbook.yml - Used to install and configure Filebeat on ELK Server and DVWA servers
metricbeat-playbook.yml - Used to install and Configure Metricbeat on ELK Server and DVWA servers
Where do you copy it : /etc/ansible
Which file do you update to make Ansible run the playbook on a specific machine? : /etc/ansible/hosts.cfg
How do I specify which machine to install the ELK server on versus which to install Filebeat on?: In /etc/ansible/hosts user writes instruction where to install the ELK servers or Filebeat
_Which URL do you navigate to in order to check that the ELK server is running? http://104.42.189.57:5601/app/kibana#/home/tutorial/systemLogs
As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
Commands needed to run the Ansible Configuration for the Elk-Server are:
1.ssh RedAdmin@JumpBox (Private IP)
2.sudo docker container list -a (to locate the ansible container)
3.sudo docker start (e.g.silly_bouman)
4.sudo docker attach (e.g.silly_bouman)
5.cd /etc/ansible/
ansible-playbook elk-playbook.yml (Installs and Configure Beats)
cd /etc/ansible/
ansible-playbook beats-playbook.yml (Installs and Vonfigure Beats)
Open a new browser on personal workstation, navigate to (ELK-Server-Public:5601/app/kibana) - This will bring up Kibana Web Portal  
Congratulations! You have an ELK Stack Server monitoring system logs on two web servers
The playbook is duplicated.
