## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

github.com/JaniceEstes/Cybersecurity_Project_1/ELK_diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the all_plays_combined.yml file may be used to install only certain pieces of it, such as Filebeat.
The files can be accessed through the JumpBoxProvistioner within the Ansible container.

github.com/JaniceEstes/Cybersecurity_Project_1/all_plays_combined.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting access to the network.

- Load balancers protect the network from a Denial of Service attack by evenly distributing traffic amont the servers. The advantage of a jump box is the ability to have only one machine that can access the servers within secure networks. It is also a place from which all configurations can be securly made for all machines within all relative networks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat watches for and collects log data and new content from within log files.
- Metricbeat records metrics collected from the system and services running on the server.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1    | Server   | 10.0.0.5   | Linux            |
| Web 2    | Server   | 10.0.0.6   | Linux            |
| Web 3    | Server   | 10.0.0.8   | Linux            |
| ELK	     | Monitor  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

- Only the ELk machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 75.190.75.120

Machines within the network can only be accessed by the Jump Box.
The Jump Box also has access to the ELK server. The Jump Box IP address is 10.0.0.4.   

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  |75.190.75.120         |
| ELK      | No                  |75.190.75.120 10.0.0.4|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. 
No configuration was performed manually, which is advantageous because IaC allows all configuration to be performed automatically on all machines instantly, and any necessary corrections can be made at one time.  
This saves a lot of time and is extremely cost efficient. 

The playbook implements the following tasks:

- Install Docker
- Install Python3
- Increase virtual memory
- Download image: sebp/elk:761
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

github.com/JaniceEstes/Cybersecurity_Project_1/sebp_elk_761.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors log files and collects data and log events. For each log, Filebeat uses a harvester to read a log, looking for new content to send to Elasticsearch, which can be accessed by the ELK server through Kibana.
- Metricbeat collects metrics from the system and services running on the server.  It sends the data to Elasticsearch, which can be accessed by the ELK server through Kibana.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

Download filebeat onto ansible by running: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

Download the filebeat-config.yml file by running: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml
