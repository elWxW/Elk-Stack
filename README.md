## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(https://drive.google.com/file/d/1HdbnpjBrfrhBjlbLA9HrFAvclf033tGZ/view?usp=sharing)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat.

  -Elk Install:
  ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: sysadmin
  become: true
  tasks:
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

    - name: python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Docker Module
      pip:
        name: docker
        state: present

    - name: Increase Memory
      command: sysctl -w vm.max_map_count=262144

    - name: User more Memory
      sysctl:
        name: vm.max_map_count
        value: 262144
        state: present
        reload: true

    - name: Download and Launch ELK Container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - "5601:5601"
          - "9200:9200"
          - "5044:5044"
          

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly resileint, in addition to restricting access to the network.
- The load balance helps withavalibilty by distbuting traffic across the network.
- The jumpbox works as a entry point into the remote network, this can bee acomplished by using ssh.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
-With the use of Filebeat the Elk Stack is able to forward and centralize log data_
- Elk server is also equiped with Metricbeat, this helps with monitoring servers by taking metrics and statistics it collects and shipstp the users desired out put.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| ELK      | Elk Stack| 10.1.0.4   | Linux            |
| WEB1     | Server   | 10.0.0.5   | Linux            |
| WEB2     | Server   | 10.0.0.6   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 137.135.116.185

Machines within the network can only be accessed by jump box provioner.
- Jumpbox:
    Public IP: 137.135.116.185
    Private IP: 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 137.135.116.185      |
| Web 1&2  | No                  |                      |
| LB       | Yes                 | *                    |
| Elk      | Kibana:5601 Yes     | *                    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-Asible helps provision and manage the infrastructure by fully automating of selected severse there by reducing chances of configuration errors

The playbook implements the following tasks:
- Install Docker
- Download & Launch ELK Container
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://docs.google.com/drawings/d/1GEEtq3qe5tNAgyX0uSaTHkbYNW4GDo_RGaY0gZrGJPw/edit?usp=sharing

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.1.0.4

We have installed the following Beats on these machines:
- Web1
- Web2

These Beats allow us to collect the following information from each machine:
- Filebeat tracks and stores file logs and events into the system. Filebeat will showing who logs in or attempts to and at what time.
- Metricbeat collects information from the operating system and from services running on the server. Metricbeat will collect the cpu usage periodiclly 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elk_install.yml file to /etc/ansible/roles/elk_install.yml.
- Update the host file to include the elk server's destination file
- Run the playbook, and navigate to http://http://104.42.218.76:5601/:5601/app/kibana to check that the installation worked as expected.