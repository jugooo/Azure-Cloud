## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

- Images/Network_Diagram.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  - ELK-playbook.yml (ELK installation)
  - my-playbook.yml (Docker Install on DVWA's)
  - filebeat-config.yml (Filebeat installation)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- The load balancer distributes incoming requests to the network/application across multiple servers. This helps an organization defend against denial-of-service (DDos) attacks.
- The jump box prevents the other VMs from being exposed to the public. The jump box is the entry point for administrators to connect to the other VMs if any repairs/adjustments/updates need to be made.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics.
- Filebeat will monitor log files, collects log events, and forwards them to Logstash/Elasticsearch to index. 
-  Metricbeat collects metrics from services running on the server and from the operating system.

The configuration details of each machine may be found below.

| Name        | Function    | IP Address | Operating system |
| ------------|---------------|------------ |----------------------|
| Jump Box | Gateway     | 10.0.0.4   | Linux                      |
| DVWA 1    | Web Server | 10.0.0.5   | Linux                      |
| DVWA 2    | Web Server | 10.0.0.6   | Linux                      |
| ELK           | Monitoring  | 10.1.0.4   | Linux                      |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My Workstation

Machines within the network can only be accessed by each other.
- The jump box (52.173.149.140) has access to the ELK server (52.162.232.243) via SSH.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible  | Allowed IP Address |
|--------------|------------------------|-----------------------|
| Jump Box  | Yes                           | My Workstation       |
| DVWA 1     | No                            | 10.0.0.1-254           |
| DVWA 2     | No                            | 10.0.0.1-254           |
| ELK            | No                            | 10.0.0.1-254           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because automating the Ansible configureations increases overall efficency by allowing an IT professional to install and enable modules/containers to all the necessary machines without having to manually do each one. 

The playbook implements the following tasks:
- 'Install pip3' uses the apt module to install 'python3-pip' which is essential to have to be running docker.
- 'Install Docker python module' utilizes the installed pip module to install docker.
- 'Setup sebp.elk:761 container' uses the docker_container module to install an ELK image (sebp/elk:761) that is 'started' when the VMs are active. Also the open ports on are 5601, 9200, 5044.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

- Images/ELK_Config_Docker.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DVWA 1 (10.0.0.5)
- DVWA 2 (10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat: File beat detects changes to the filesystem. Specifically, we use it to collect Apache logs.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks file to the Ansible Control Node.
- Update the /etc/ansible/hosts file to include the web servers (DVWA 1 & 2 )
- Run the playbook, and navigate to 'http://157.55.164.113/:5601/app/kibana' to check that the installation worked as expected.

- Playbooks:
        - ELK-playbook.yml (ELK installation)
        - my-playbook.yml (Docker Install on DVWA's)
        - filebeat-config.yml (Filebeat installation)
- To make Ansible run on specific machines you edit the hosts file and add the internal IP's of the DVWA's under '[webservers]'. Also you add webservers to the 'hosts' option at the header of the my-playbook.yml file.
- To specify which machine the ELK server is on you add an '[elk]' section in the ansible/hosts file with the internal IP of the ELK machine. At the header of the ELK-playbook.yml file you add elk to to the 'hosts' option.
- Navigate to 'http://157.55.164.113/:5601/app/kibana' to check that the ELK server is running
