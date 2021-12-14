## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Diagrams/ELK_diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the all_plays_combined.yml file may be used to install only certain pieces of it, such as Filebeat.
The files can be accessed through the link below.

https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Ansible/all_plays_combined.txt

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

| Name     | Function | IP Address | Operating System  |
|----------|----------|------------|-------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux Ubuntu 18.04|
| Web 1    | Server   | 10.0.0.5   | Linux Ubuntu 18.04|
| Web 2    | Server   | 10.0.0.6   | Linux Ubuntu 18.04|
| Web 3    | Server   | 10.0.0.8   | Linux Ubuntu 18.04|
| ELK	     | Monitor  | 10.1.0.4   | Linux Ubuntu 18.04|

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

The ELK configuration portion of this playbook implements the following tasks:

- Install Docker
- Install Python3
- Increase virtual memory
- Download image: sebp/elk:761
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Images/sebp_elk_761.png

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
- Metricbeat collects metrics from the system and services running on the server.  It sends the data to Elasticsearch, which can also be accessed by the ELK server through Kibana.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

-Download filebeat and metricbeat .deb files onto your Ansible container. 

-Run curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml

-Scoll to line 1106 in this configuration file and change the IP to match the IP of your ELK server.

-Do the same at line 1806.

-Run curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/metricbeat-config.yml

-Make the same IP changes in this file as in the filebeat-config.yml file wherever necessary.

-Run curl https://raw.githubusercontent.com/JaniceEstes/Cybersecurity_Project_1/main/all_plays_combined.txt?token=AVPRCBZMW5OM6PLXKSRPCUDBXCJXE > /etc/ansible/roles/elk_plays.yml

-Update the /etc/ansible/hosts file to include:

   -"[IP address of webserver] ansible_python_interpreter=/usr/bin/python3"
   -[Do not include quotations (" ") in the above configuration]
   -Follow this step for each webserver to be configured
   -Create a group for [elk] within the hosts file and add the IP of the ELK server just as for the webservers, as well as "ansible_python_interpreter=/usr/bin/python3".
    
- Make sure the username for your ELK machine is found within /etc/ansible/ansible.cfg under "remote users", and please update the playbook to include your username.

- Run the playbook, and navigate to http://[IP of your ELK server]/app/kibana#/home/tutorial/systemLogs, scroll to the bottom and click "Check Data" to check that the filebeat installation worked as expected. Then navigate to http://[IP of your ELK server]/app/kibana#/home/tutorial/systemMetrics, scroll to the bottom and click "Check Data" to check that the metricbeat installation worked as expected.

** Bonus ** 
Here are the specific commands the user will need to run to download and run the playbook and update the files successfully. 

-Navigate to your Ansible container from your Jump Box:
 
 -Run:
   sudo docker ps
 
  -You will see the name of your ansible container (e.g., musing-swanson)
  
  -Run:
   sudo docker start [name of your ansible container]
   sudo docker attach [name of your ansible container]
   
-From root, run:
   cd /etc/ansible
   mkdir roles
   mkdir files

-Download filebeat onto ansible by running: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

-Download metricbeat onto ansible by running: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

-Download the filebeat-config.yml file by running: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml

-Download the metricbeat-config.yml file by running: curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml

-Run the following to edit configuration files (see below for instructions on what to change):
   cd files
   nano filebeat-config.yml
   nano metricbeat-config.yml
   
-Go through each -config.yml file and update the IP to match that of the ELK server.

-Download the playbook by running:
    curl https://raw.githubusercontent.com/JaniceEstes/Cybersecurity_Project_1/main/all_plays_combined.txt?token=AVPRCBZMW5OM6PLXKSRPCUDBXCJXE >    /etc/ansible/roles/elk_plays.yml

-You must update the hosts file. Run:
    cd /etc/ansible
    nano hosts
    
-Update the /etc/ansible/hosts file:
    
   -Under [webservers], include "[IP address of webserver] ansible_python_interpreter=/usr/bin/python3"
    
   -[Do not include quotations (" ") in the above configuration]
    
   -Follow this step for each webserver to be configured
    
   -Create a group for [elk] below the [webservers] group within the hosts file and add the IP of the ELK server just as for the webservers.
    
   -Refer to this screenshot for guidance: https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Hosts%20File.png

-Make sure the username for your ELK machine is found within /etc/ansible/ansible.cfg under "remote users".

  -Here is what the ansible.cfg file looks like: https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Remote%20Users.png

-You must update the playbook to include the username(s) that match your webservers and your ELK server.
  
  -Here is where you need to confirm your host is correct, and where you need to change the "remote user" name to your own for the ELK server: https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Update%20the%20playbook.png

-Run the playbook with the following commands:
    cd /etc/ansible/roles
    ansible-playbook elk_plays.yml
    
  -This is what it looks like when the playbook runs successfully: https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/Success%20Running%20Playbook.png
    
-Navigate to http://[IP of your ELK server]/app/kibana#/home/tutorial/systemLogs, scroll to the bottom of the page, and click "Check Data" to make sure the filebeat installation was successful. Then navigate to http://[IP of your ELK server]/app/kibana#/home/tutorial/systemMetrics, scroll to the bottom and click "Check Data" to check that the metricbeat installation worked as expected.

-Here is what it looks like: 
https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/filebeat%20data%20successful.png
https://github.com/JaniceEstes/Cybersecurity_Project_1/blob/main/metricbeat%20data%20successful.png

