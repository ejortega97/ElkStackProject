## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!Users/Erick/ElkStackProject/Diagrams/ElkStack Map

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - elkplaybook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly resilient, in addition to restricting threats to the network.
- Load balancers can protect against DDoS attack by detecting the traffic before it gets to the website. It also authenticates a user address before granting access. 
- Jump boxes improve network security by making sure it acts as a safe bridge for admins to connect to before launching into any administrative tasks.
- The advantage of this that it acts a highly secured entrypoint never used for non administrative tasks for users to access before enterying project enviroments from. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name        | Function          | IP Address | Operating System |
|-------------|-------------------|------------|------------------|
| Jump Box VM | Gateway           | 10.0.0.4   | Linux            |
| Web 2       | DVWA Container VM | 10.0.0.5   | Linux            |
| Web 3       | DVWA Container VM | 10.0.0.7   | Linux            |
| Elk VM      | Elk Server        | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 24.157.115.212 (Admin Workstation IP Address)

Machines within the network can only be accessed by the Jump Box VM.
- 10.0.0.4/32 (Jumpbox VM Address)

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Gateway             | 24.157.115.212       |
| Elk Server | DVWA Container VM   | 10.0.0.4/32          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- It cuts down on the time and the margin for human error when attempting manual configuration.

The playbook implements the following tasks:
- Configure Elk VM with Docker
- Install docker.io
- Install python3-pip
- Install Docker module
- Increase virtual memory
- Use more memory
- Download and launch a docker elk container
- List published ports which ELK will run on
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!Users/Erick/ElkStackProject/Ansible/Images/'docker ps'

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.4

We have installed the following Beats on these machines:
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat comes with internal modules that simplify the collection, parsing, and visualization of common log formats down to a single command.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to the ansible diretory.
- Update the yml file to include line...
- Run the playbook, and navigate to the Elk server VM to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? .yml, copy it into the ansible directory
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? Update the hosts file with the internal IP of the host VM. To specify the machine, make sure to configure the ELK VM with Docker by setting hosts and remote_user to match what you have in the hosts config file and that it correlates to the machine you're installing it on.
- _Which URL do you navigate to in order to check that the ELK server is running? http://40.112.179.178/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present
		
name: Install Docker module
      pip:
        name: docker
        state: present
		
name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
       
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
		  
name: Enable service docker on boot
      systemd:
        name: docker