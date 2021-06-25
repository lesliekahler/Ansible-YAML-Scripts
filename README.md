## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

<img width="1002" alt="Jumpbox_Diagram" src="https://user-images.githubusercontent.com/78235740/123361791-e4de8e00-d52c-11eb-837e-75bed311ee18.png">


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible file may be used to install only certain pieces of it, such as Filebeat.

- [Install-Elk](https://github.com/lesliekahler/Ansible-YAML-Scripts/blob/main/Ansible/Install-Elk%20YAML.rtf)
-  [Install DVWA](https://github.com/lesliekahler/Ansible-YAML-Scripts/blob/main/Ansible/Install-DVWA-YAML.rtf)
-  [Filebeat-playbook](https://github.com/lesliekahler/Ansible-YAML-Scripts/blob/main/Ansible/Filebeat-Playbook-YAML.rtf)
-  [Metricbeat-playbook](https://github.com/lesliekahler/Ansible-YAML-Scripts/blob/main/Ansible/Metricbeat-Playbook-YAML.rtf)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting traffic to the network.
- Load balancing is the process of distributing network traffic across multiple servers. This ensures no single server bears too much demand. By spreading the work evenly, load balancing improves application responsiveness. It also increases availability of applications and websites for users. 
- A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them to Elasticsearch for indexing. 
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch.

The configuration details of each machine may be found below.


| Name         | Function     | IP Address | Operating System |
|----------------|-----------------|----------------|--------------------------|
| Jump Box  | Gateway     | 10.0.0.7     | Linux                      |
| Web-1       |Web Server  | 10.0.0.5     | Linux                      |
| Web-2       |Web Server  | 10.0.0.6     | Linux                      |
| Web-3       |Web Server  | 10.0.0.10   | Linux                      |
| Elk-VM      | Elasticsearch Stack |  10.1.0.5  |   Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 107.2.130.224

Machines within the network can only be accessed by the JumpBox.
- JumpBox: 10.0.0.7

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses |
|-----------------|---------------------------|-------------------------------|
|Allow-RDP   |  No                          | 107.2.130.224             |
|Allow-HTTP |  No                          | 107.2.130.224             |
|Allow-SSH   |  No                          | 10.0.0.7                       |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because you can combine multiple commands into one Yaml file. Writing our the Yaml file takes time, but will save you time in the long-run. 

The playbook implements the following tasks:
- Install Docker
- Install Python
- Increase memory
- Download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
- File location: <img width="925" alt="Docker_ps_output" src="https://user-images.githubusercontent.com/78235740/123361824-f0ca5000-d52c-11eb-80fc-e8f2e1efd4e5.png">

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7
- 10.0.0.10

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them to Elasticsearch for indexing. 
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install_elk.yml playbook file to /etc/ansible/roles/ directory inside the ansible container.
- 	 $ sudo docker cp install_elk.yml <container.name>:/etc/ansible/roles/install_elk.yml
- Optional: Copy the whole directory for the Metricbeat and Filebeat playbooks and configuration files.
- 	$ sudo docker cp Ansible/* <container.name>:/etc/ansible
- Attach to the ansible docker with $ sudo docker attach <container.name>
- Update the /etc/ansible/hosts file to include the ELK stack VM IP address.
- 	Append ansible_python_interpreter=/usr/bin/python3 to ensure that the correct version of python is used.
- Example configuration of /etc/ansible/hosts
[elk]
<internal.ip>      ansible_python_interpreter=/usr/bin/python3
<external.ip>      ansible_python_interpreter=/usr/bin/python3
- Run the playbook, and navigate to http://[your.elk.ip]:5601/app/kibana. to check that the installation worked as expected.
- 	$ ansible-playbook /etc/ansible/roles/elk-stack-playbook.yml


<img width="1589" alt="Screen Shot 2021-06-24 at 8 24 51 PM" src="https://user-images.githubusercontent.com/78235740/123361749-d3958180-d52c-11eb-86d0-869bee513368.png">

Using the Metricbeat and Filebeat Playbooks

Filebeat
- Edit /etc/ansible/files/filebeat-config.yml in the ansible container on the control node to include the ELK Stack IP address. You should also change the default login credentials.
output.elasticsearch:
hosts: ["<elk.ip.addr>:9200"]
username: "elastic"
password: "changeme"
setup.kibana:
host: "<elk.ip.addr>:5601"
- Then run the playbook
- 	$ ansible-playbook /etc/ansible/roles/filebeat-playbook.yml

Metricbeat
- Edit /etc/ansible/files/metricbeat-config.yml in the ansible on the control node to include the ELK Stack IP address. You should also change the default login credentials.
output.elasticsearch:
hosts: ["<elk.ip.addr>:9200"]
username: "elastic"
password: "changeme"
setup.kibana:
host: "<elk.ip.addr>:5601"
- Then run the playbook
- 	$ ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml
