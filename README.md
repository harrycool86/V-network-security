## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](images/Cloud_N_Security.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

![filebeat-playbook](images/playbook.txt)


-  ---
    -  name: installing and launching filebeat
    -  hosts: webservers
    -  become: yes
    -  tasks:


    - name: download filebeat deb
    - command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
 

    - name: install filebeat deb
    - command: dpkg -i filebeat-7.4.0-amd64.deb


    - name: drop in filebeat.yml 
    - copy:
    - src: /etc/ansible/filebeat-config.yml
    - dest: /etc/filebeat/filebeat.yml


    - name: enable and configure system module
    - command: filebeat modules enable system
    - name: setup filebeat
    - command: filebeat setup


    - name: start filebeat service
    - command: service filebeat start





##### This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that traffic coming into the website are evenly distributed across multiple servers behind it, in addition to restricting public access directly to the network.
- Load balancers provides another level of security to webservers by providing a website an external IP address that is accessed by the internet. The load balancer receives any traffic that comes into the website and distributes it across multiple servers which also could help to mitigate DoS attacks. One advantage of the Jump box is that It controls access to other machines "webservers" by allowing connections from specific IP addresses and forwarding to those machines, using ssh key to access the jump box is another advantage.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system logs.
- Filebeat helps generate and organize log files to send to Logstash and Elasticsearch. Specifically, it logs information about the file system, including which files have changed and when
- Metricbeat is extremely easy to use which allows you to monitor your system and the processes running on it.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name        | Function   | Ip Address | Operating System |
|-------------|------------|------------|------------------|
| Jump box    | Gateway    | 10.0.0.4   | linux            |
| Web1/DVWA   | webserver  | 10.0.0.5   | linux            |
| Web2/DVWA   | Websever   | 10.0.0.6   | linux            |
| ELK machine | ELk server | 10.1.0.4   | linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 76.66.108.155, 74.66.109.543 

Machines within the network can only be accessed by Machines on the internal network.
- You can only access the elk machine from within the ansible container in the jump box "ip address 10.0.0.4" via ssh login. 

A summary of the access policies in place can be found in the table below.

| Name       | publicly accessible | Allowed Ip Addresses         |
|------------|---------------------|------------------------------|
| Jump box   | Yes/ via SSh        | 76.66.108.155, 74.66.109.543 |
| Web1/DVWA  | No                  | 10.0.0.4                     |
| Web2/DVWA  | No                  | 10.0.0.4                     |
| ELK server | No                  | 10.0.0.4                     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- In the event that there was a breach or disaster and the system got compromised, you can easily reuse the playbook to redeploy programs or run update on a system. It also helps to avoid time wasting on having to type every code or commands manually anytime you need to make changes or update the system. Ansible playbook can automate all of these tasks in one shot.

The playbook implements the following tasks:

-    ---
     - name: Configure Elk VM with Docker
     - hosts: elkservers

     - name: Install docker.io

     - name: Install python3-pip
     
      - name: increase virtual memory
        sysctl:
        name: vm.max_map_count

      - name: download and launch a docker elk container
        docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always

    
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
