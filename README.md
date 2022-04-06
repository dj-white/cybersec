# cybersec
A space where all my cyber learning is kept (in disarray)
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

-Diagrams/RedTeam_Network_Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - -Ansible/filebeat-playbook.txt

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound connections to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
		
		-Load balancers are used to balancer a regulate the incoming connections to a network or subnet. They ensure that traffic is routed equally between the servers in the backend, 		 this can help avoid DDoS based attacks.
		
		-The advantage of a Jump Box is that it provides a controlled way to access and manage devices on a network. By blocking the ips of the VMs on our Virtual network an only 			 accessing them through the jump box, we've hardened the network by reducing the attack service.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system and system logs.
- _TODO: What does Filebeat watch for?
		
		-Filebeat monitors log files or locations that the administrator specifies, then collects log events, and forwards the data to the ELK stack server. 
			(https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html) 

- _TODO: What does Metricbeat record?

		-Metricbeat records metrics and statistics from operating systems and services being ran on the servers an administrator wants to monitor. These metrics and statistics are then 		shipped to the ELK stack in a specified output. 
			(https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html)
		

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function         | IP Address             | Operating System |   
|----------------------|------------------|------------------------|------------------|
| Jump-Box-Provisioner | Gateway          | 10.2.0.4;20.92.246.166 | Linux            |
| Web-1                | Web-Server       | 10.2.0.7               | Linux            |
| Web-2                | Web-Server       | 10.2.0.6               | Linux            |
| ELK                  | ELK stack server | 10.3.0.4;20.213.243.56 | Linux            |
| RT-Load-Balancer     | Load Balancer    | 20.213.75.232          | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 174.93.84.102
Machines within the network can only be accessed by SSH from the Jump-Box_Provisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
	 The only machines that can access the ELK VM are the host workstation (174.93.84.102) and the 	jump-box-provisioner using the container through ssh.  

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump-Box-Provisioner | Yes                 | 174.93.84.102        |
| ELK                  | Yes                 | 174.93.84.102:5601   |
| Web-1                | No                  |                      |
| Web-2                | No                  |                      |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
	The main advantage of automating configuration with Ansible is that using Ansible playbooks allow you to configure multiple machines all at once with minimal administrative interaction.

The playbook implements the following tasks:
- Installs Docker
- Installs Python
- Installs Docker python module
- Increases the virtual memory of machine so Elasticsearch will work properly
- Downloads and launches a docker elk container and opens the ports used by the ELK stack
- Ensures that Docker starts on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

-/Images/elk_ss.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.2.0.7
  Web-2: 10.2.0.6

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- These beats are used to collect system log data from the monitor machines. It can detect changes in the system and the use of different services like ssh login attempts for example. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to the /etc/ansible/files directory.
- Update the filebeat-config.yml file to include the IP address/port of the host for elasticsearch and Kibana  as well as setup the default username and password. 
- Run the playbook, and navigate to  to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? 
- It is copied to the /etc/ansible/roles directory
- _Which file do you update to make Ansible run the playbook on a specific machine?
	You would update the /etc/ansible/hosts file and add the ip addresses of the machines you want to configure to the given server type. For example the Web-1 and Web-2 addresses were added under webserver. 
How do I specify which machine to install the ELK server on versus which to install Filebeat on?
	In the playbook file you can specify which host type that corresponds to the host file you configured previously, that you want to configure using the playbook. Example, if you wanted to install filebeat on Web-1 and Web-2 you would use the webserver host name at the start of the playbook file.
	
- _Which URL do you navigate to in order to check that the ELK server is running?
	-localhost:5601

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._ 
-
curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml

nano /etc/ansible/filebeat-config.yml
nano /etc/ansible/hosts
cd /etc/ansible/roles
nano filebeat-playbook.yml 
ansible-playbook filebeat-playbook.yml
nano /etc/ansible/metricbeat-config.yml
nano /etc/ansible/hosts
cd /etc/ansible/roles
nano metricbeat-playbook.yml 
ansible-playbook metricbeat-playbook.yml
