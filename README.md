'# Spes-Cloud-

This is a repository for the Spes clound network
 
## Automated ELK Stack Deployment

	

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config files may be used to install only certain pieces of it, such as Filebeat.

  - [Ansible Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-Playbook.yml)
  - [Ansible Hosts](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-Hosts.txt)
  - [Ansible Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-config.cfg)
  - [Ansible ELK Installation and VM Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/Ansible-ELK_install.yml)
  - [Ansible Filebeat Playbook](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/filebeat-playbook.yml)
  - [Ansible Filebeat Configuration](https://github.com/spes027/Spes-Cloud-/blob/main/Ansible/filebeat-config.yml)


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
The Advantage of a jumpbox is access control this allows only those with ssh access to enter the rest of the the VMs.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- What does Filebeat watch for?_
File beat monitors log files or locations specified by the user then collects tyhe logs and moves the data to Elastic search or logstash.

- What does Metricbeat record?_
Metricbeat helps to monitor servers by collecting metrics from the system and services running on a server. 


The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| WEB1     | WebServer| 10.0.0.5   | Linux            |
| WEB2     | WebServer| 10.0.0.6   | Linux            |
| ELK      |ELK Server| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.247.110.30

Machines within the network can only be accessed by Work station and Jumpbox Provisioner.
- Which machine did you allow to access your ELK VM? What was its IP address?_
The machine with access is the Jumpbox Ip= 104.43.205.224 from SSH port 22.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Workstation public ip on ssh22 |
| Web 1    | No                  | 10.0.0.4 on SSH 22                     |
| Web 2    | No                  | 10.0.0.4 on SSH 22                     |
| ELK Server| No| Workstation Public IP using TCP 5601 |
|Load Balancer| No | Work Station IP on HTTP 80 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_
The main advantage of Ansible is that it allows you to distribute multitier apps.  A playbook is written and ansible executes the commands.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: SPES027
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

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

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

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

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

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

