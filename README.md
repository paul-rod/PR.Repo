## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/paul-rod/PR.Repo/blob/main/Network%20Topology.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml and .config file may be used to install only certain pieces of it, such as Filebeat.

  - [Pentest](../blob/main/pentest.yml)
  - [Hosts](../blob/main/Hosts)
  - [Ansible Configuration](../blob/mainansible)
  - [Ansible ELK Installation and VM Configuration](../blob/main/install-elk.yml)
  - [Filebeat Config](../blob/main/filebeat_config.yml)
  - [Filebeat Playbook](../blob.main/filebeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional and available, in addition to restricting traffic to the network.

- What aspect of security do load balancers protect? 

  - **_Load balancers add resiliency by rerouting live traffic from one server to another if a server falls prey to a DDoS attack or otherwise becomes unavailable._**

- What is the advantage of a jump box?

  - **_A Jump Box Provisioner is also important as it prevents Azure VMs from being exposed via a public IP Address. This allows us to do monitoring and logging on a single box. We can also restrict the IP addresses able to communicate with the Jump Box._**

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **_Network_** and **_system logs_**.

- What does Filebeat watch for?

  - **_Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing._**

- What does Metricbeat record?
  - **_Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash._**

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Ubuntu   | 10.0.0.7   | Linux            |
| Web-2    | Ubuntu   | 10.0.0.6   | Linux            |
| Web-3    | Ubuntu   | 10.0.0.8   | Linux            |
| ELK-VM   | Ubuntu   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **_Jumpbox Provisioner_** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- **99.234.22.248**

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner through SSH Jump-Box.

    Jump-Box-Provisioner IP : 10.0.0.1 via SSH port 22

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 52.152.234.219       |
| Web-1    | No                  | 10.0.0.7 on SSH 22   |
| Web-2    | No                  | 10.0.0.6 on SSH 22   |
| Web-3    | No                  | 10.0.0.8 on SSH 22   |
| ELK-VM   | No                  | MY IP using TCP 5601 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- **Ansible lets you quickly and easily deploy multitier applications through a YAML playbook.**
- **You don't need to write custom code to automate your systems.**
- **Ansible will also figure out how to get your systems to the state you want them to be in.**

The playbook implements the following tasks:

- **Install Docker: Installs the core docker code to the remote server**
- **Install Python3_pip: Pip is an installation module that allows for additional docker modules to be installed easier**
- **Docker Module: Tells the previous PIP module to install the necessary docker component modules**
- **Increase Memory/Use More Memory: A common issue with the ELK Docker image is to little memory. This help fix the issue to allow the server to launch**
- **Download and Launch ELK Container: This downloads the ELK docker container and initializes it with the specified ports being published**

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/paul-rod/PR.Repo/blob/main/ELK%20Server%20-%20sudo%20docker%20ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- **Web-1: 10.0.0.7**
- **Web-2: 10.1.0.6**
- **Web-3: 10.1.0.8**

We have installed the following Beats on these machines:

- **Filebeat** - ![Filebeat Module Status Screenshot](https://github.com/paul-rod/PR.Repo/blob/main/Screen%20Shot%202021-09-10%20at%206.04.30%20PM.png)

These Beats allow us to collect the following information from each machine:
- Filebeat will be used to collect log files from very specific files such as Apache, Microsft Azure tools and web servers, MySQL databases.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the **_yml_** file to **_ansible_** folder.
- Update the **_config_** file to included **_remote users and ports_**
- Run the playbook, and navigate to _Kibana (http://20.102.192.222:5601)_ to check that the installation worked as expected.

_Answer the following questions to fill in the blanks:_
- _Which file is the playbook?_
    - **pentest.yml**
    - **install-elk.yml** 
    - **filebeat-playbook.yml**
    
- _Where do you copy it?_
    - **/etc/ansible/**

- _Which file do you update to make Ansible run the playbook on a specific machine?_ 
    - **/etc/ansible/hostsfile (IP of the Virtual Machines)**

- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
    - **One of the group will be webservers which has the IPs of the 3 VMs that I will install Filebeat to. The other group is named ELKserver which will have the IP of the VM I will install ELK to.**

- _Which URL do you navigate to in order to check that the ELK server is running?_
    - http://20.102.192.222:5601/app/kibana
