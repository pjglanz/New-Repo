# New-Repo
ELK project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/ELK Project Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Elk file may be used to install only certain pieces of it, such as Filebeat.

  Ex. filebeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly resilient, as well as redundant, in addition to restricting resources and access to the network.
Since a load balancer distributes network services and resources between servers, it is harder for a denial of service-type attack or a failure of resource traffic to go unnoticed. Using a jump box to sign on to the cloud is also a secure way to test how the network responds, since one has to tunnel into it using SSH from another workstation, and usually they are closed off to inbound network traffic.  


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
Filebeat is a log monitoring system that can be installed through a playbook file or script into a container to record log events and then passes them on to another part of the ELK stack (usually Elasticsearch or LogStash) for organizing. 
Metricbeat is similar to Filebeat in the ELK stack as it records data metrics from the user’s operating system and any processes or services that are currently running, and then sends them to Elasticsearch or LogStash for output. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.9   | Linux            |
| Web1     | DVWA     | 10.0.0.7   | Linux            |
| Web2     | DVWA     | 10.0.0.8   | Linux            |
| ELK      | Gateway  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 108.251.183.10

Machines within the network can only be accessed by SSH.
- 10.0.0.7 (web1)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because with Ansible the automation can be performed with different formatting files, such as YAML scripts, and can be run exactly the same in each instance the commands are entered. It is also highly secure as it only allows SSH traffic. 

The playbook implements the following tasks:
- Installing Docker - sudo apt install docker.io
- using Ansible to configure the Docker elk VM by reconfiguring the hosts file to include the webservers and elk groups as well as accompanying IP addresses.
- Installing Docker python module – changing configuration file by adding servers web1 and web2 (10.0.0.7/10.0.0.8)
- Using the sysctl module to change the memory requirements for installing the Elk server
- downloading and launching the docker Elk container, by downloading an image and restricting access only to port 5601. 



The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/kibanacapture.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.7, 10.0.0.8

We have installed the following Beats on these machines:
- Elasticsearch
- Haproxy
- Kibana
- Osquery
- Logstash

- These Beats allow us to collect the following information from each machine.
- Logstash looks through all the logs and sets a default path to log files (this can be changed) while also organizing a long-lined log event into a single event for categorization, making it easier to view once in the Kibana suite.
- Elasticsearch does almost the same as Logstash while making the data output easier to view and separate in the GUI of Kibana
- Haproxy collects and organizes logs from a proxy server
- OSquery collects logs from the current operating system
- 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Filebeat configuration file to the web VM where Filebeat is installed.
- Update the config file to include which host machine will run Filebeat and which machine to install the docker ELK server. 
- Run the playbook, and navigate to Kibana using the public IP of the ELK server to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? The .deb file from github
 Where do you copy it?_to the WebVMs where Filebeat is already installed. Ex. Ansible container /etc/ansible/files
- _Which file do you update to make Ansible run the playbook on a specific machine? Filebeat-config.yml 
How do I specify which machine to install the ELK server on versus which to install Filebeat? By changing the syntax inside the configuration file and which host to run it on. 
- _Which URL do you navigate to in order to check that the ELK server is running? 13.84.130.249:5601/app/kibana/

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.
- Start the ansible container by SSH’ing into the jump box (ssh RedAdmin@20.51.249.77), sudo docker ps, sudo docker container list -a, then pick out container and sudo docker container start <filename> and sudo docker container attach <filename>.
- Download the .deb file from artifacts.elastic.co – using command curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml – this copies the files into the /etc file inside the Ansible container.
- Edit the configuration file by nano filebeat-config.yml to include enabling the system modules, setup, and starting the service when running the playbook. (filebeat modules enable system, filebeat setup, service filebeat start). 
- Insert filebeat-config.yml script into filebeat-playbook.yml file.
- Run ansible-playbook filebeat-playbook.yml
- Verify playbook script ran by venturing to public IP address of ELK server attached to port 5601 for Kibana (in this case, 13.84.130.249:5601/app/kibana

