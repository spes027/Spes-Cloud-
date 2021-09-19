
This is a repository for the Spes clound network
 
## Automated ELK Stack Deployment

![ELK network drawio](https://user-images.githubusercontent.com/84035560/133915159-efbaa64d-63c4-4de1-a7fb-ca293eb21eca.png)


	

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config files may be used to install only certain pieces of it, such as Filebeat.

  - [Ansible Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-Playbook.yml)
  - [Ansible Hosts](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-Hosts.txt)
  - [Ansible Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-config.cfg)
  - [Ansible ELK Installation and VM Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-ELK_install.yml)
  - [Ansible Filebeat Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/filebeat-playbook.yml)
  - [Ansible Filebeat Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/filebeat-config.yml)
  - [Ansible Metricbeat Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Metricbeat-Playbook.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

- What aspect of security do load balancers protect?
 
Load Balancers protect the Avalibility of a website by ristricting flow to servers. 

- What is the advantage of a jump box?_

The Advantage of a jumpbox is access control. It only allows those with ssh access to enter the rest of the the VMs.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- What does Filebeat watch for?

File beat monitors log files on servers. It searches for changes and logs events to be viewed in ELK.

- What does Metricbeat record?_

Metricbeat helps to monitor servers by collecting metrics from the system and services running on a server. This can be CPU usage and data. 


The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| WEB1     | WebServer| 10.0.0.5   | Linux            |
| WEB2     | WebServer| 10.0.0.6   | Linux            |
| ELK      |Monitoring| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.247.XXX.XXX

Machines within the network can only be accessed by a selected Work station and then into the Jumpbox Provisioner and so on
.
- Which machine did you allow to access your ELK VM? What was its IP address?

The machine with access is the Jumpbox Ip= 104.43.205.224 from SSH port 22.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Workstation public ip |
| Web 1    | No                  | 10.0.0.4  |
| Web 2    | No                  | 10.0.0.4 |
| ELK Server| No                 |10.0.0.4|


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_
The main advantage of Ansible is that it allows you to distribute multitier apps.  A playbook is written and ansible executes the commands.

The playbook implements the following tasks:


- This specifies that these tasks should only be run on the machines in the elk group 
```- 
 name: Configure Elk VM with Docker
  hosts: elk
  remote_user: SPES027
  become: true
  tasks:
```
- This installs docker on ELK machines 
```
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present
```
- This installs python and pip 
```
      # Use apt module
    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
      # Use pip module (It will default to pip3)
    - name: Install Docker module
      pip:
        name: docker
        state: present
```
- This configures the VM target to use more memory
```
      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes
```
Launching Container with ports below 
```
      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044
```
- Enables Docker to start up on boot up.
```
      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
```


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELK Server](https://github.com/spes027/Spes-Cloud-/blob/main/Diagrams/Screenshot%202021-09-18%20220702.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5
- Web-2: 10.0.0.6

We have installed the following Beats on these machines:

- Filebeat 

- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat is used to collect log files from specific places like the web servers, Apache and MySQL databases.

- Metricbeat is used to monitor the functions of the VMS such as CPU useage and memory.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to the ansible folder.
- Update the config file to include remote users and ports.
- Run the playbook, and navigate to http://IPADDRESS:5602/app/kibana to check that the installation worked as expected.

- Which file is the playbook? 
  - The ansible playbook file is here: [Ansible Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-Playbook.yml)

- Where do you copy it? 
  - The files should be copied to the path: /etc/ansible/


- Which file do you update to make Ansible run the playbook on a specific machine? 

  - This would be the hosts file on the VM.

-  How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  - This is done by creating two seperate groups within the hosts file

- _Which URL do you navigate to in order to check that the ELK server is running?
  - http://ELK IP Address:5601/app/kibana


|Commands|Execution|
|--------|---------|
|ssh-keygen| create a ssh key for setup VM's|
|sudo cat .ssh/id_rsa.pub| to view the ssh public key|
|ssh YOUR USER NAME@Jump-Box-Provisioner IP address| to log into the Jump-Box-Provisioner|
|sudo docker container list -a|	list all docker containers|
|sudo docker ps -a|list all active/inactive containers|
|sudo docker start (container name)| Start container|
|sudo docker exec -it (container name) bash| enter container|
|cd /etc/ansible|	Change directory to the Ansible directory|
|ls -aA|	List all file in directory|
|nano /etc/ansible/hosts|	to edit the hosts file|
|nano /etc/ansible/ansible.cfg|	to edit the ansible.cfg file|
|nano /etc/ansible/ansibleplaybook.yml|	to edit the Ansible playbook|
|ansible-playbook (location)(filename)|	to run the playbook|
|exit|	to exit out of VM|
|nano /etc/ansible/ansible.cfg|	to edit the ansible.cfg file|
|nano /etc/ansible/hosts|	to edit the hosts file|
|nano /etc/ansible/pentest.yml|	to edit the My-Playbook|
|sudo apt-get update|	update packages|
|sudo apt install docker.io|	install docker application|
|sudo service docker start|	start the docker application|
|sudo systemctl status docker|	status of the docker application|
|sudo systemctl start docker|	start the docker service|
|sudo docker pull cyberxsecurity/ansible|	pull the docker container file|
|sudo docker run -ti cyberxsecurity/ansible bash|	run and create a docker container image|
|ansible -m ping all|	check the connection of ansible containers|
|curl -L -O (location of the file on the web)|	to download a file from the web|
|dpkg -i (filename)|	to install the file i.e.| 
|http://ELK IP ADDRESS:5601//app/kibana|	Open web browser and navigate to Kibana Logs|
|nano filebeat-config.yml|	create and edit filebeat config file|
|nano filebeat-playbook.yml|	write YAML file to install filebeat on webservers|
|nano metricbeat-config.yml|	create metricbeat config file and edit it|
|nano metricbeat-playbook.yml|	write YAML file to install metricbeat on webservers|


