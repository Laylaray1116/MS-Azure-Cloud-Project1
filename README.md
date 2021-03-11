# MS-Azure-Cloud-Project1

***
**Automated ELK Stack Deployment using DVWA**

The files in this repository are used to demonstrate the current ELK topology using MS Azure PaaS and DVWA. The diagram below provides the ELK virtual design including its network scheme and applies security access controls to support this cloud environment.  

 [My Network Diagram] <https://github.com/Laylaray1116/MS-Azure-Cloud-Project1/blob/main/Diagrams/Azure%20Cloud%20Computing%20Diagram_030921.png>

Next, the Ansible playbooks directly following were tested and used to generate a live elk deployment on Azure. The approach here can be used to either recreate the entire deployment pictured above or choose selected portions of the **filebeat-playbook.yml** file to install specific SW modules, such as Filebeat and Metricbeat--in support of ELK monitoring.

 <https://github.com/Laylaray1116/MS-Azure-Cloud-Project1/blob/main/Ansible/ELK.yml>

 <https://github.com/Laylaray1116/MS-Azure-Cloud-Project1/blob/main/Ansible/filebeat-playbook.yml>

This READme.md document contains the following details: - Description of the Topology - Access Policies - ELK Configuration - Beats in Use - Target Machines Being Monitored - How to Use the Ansible Build
***
**Description of the Topology**

The main purpose of this virtual network is to execute a load-balanced and monitored instance of DVWA--DAMM Vulnerable Web Application.

Load balancing (LB) ensures that the web application traffic is  highly distrbuted over the three DVWA VMs reducing load and ultimately improving end-user productivity. Keeping in mind productivity, security must be the stronghold and having LBs incorporated in this or any environment leads to:

- "Protect applications from emerging threats
The Web Application Firewall (WAF) in the load balancer protects your website from hackers and includes daily rule updates just like a virus scanner

- Authenticate User Access
The load balancer can request a username and password before granting access to your website to protect against unauthorized access

- Protect against DDoS attack. The load balancer can detect and drop distributed denial-of-service (DDoS) traffic before it gets to your website.

- Simplify PCI compliance
If you process credit cards, you need to comply with Payment Card Industry (PCI) regulations. A load balancer simplifies compliance with PCI rules."

In addition, by placing a jumpbox running Ansible to access/manage the current DVWA VM containers this serves critical support and executes 24/7 strong security access controls protecting the aforemention VMs from possible outside threats.

Furthermore, integrating an ELK server allows internal sysadmins/users to easily monitor the DVWA VMs for changes to their respective network, apps, and system logs. Specifically in this deployment, ELK Beats, Filebeat and its partner Metribeat are monitoring and gathering server syslogs and metrics to ensure consistent environment availability and security.

The MS Azure configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.

|    Name   |        Function        | IP Address  | Operation System  | MS Azure Zone |
|:---------:|:----------------------:|:-----------:|:-----------------:|---------------|
| Jump Box  | Management Gateway     | 10.0.0.4    | Linux             | East US       |
| Web-1     | xCorp Web-App Server 1 | 10.0.0.5    | Linux             | East US       |
| Web-2     | xCorp Web-App Server 2 | 10.0.0.6    | Linux             | East US       |
| Web-3     | xCorp Web-App Server 3 | 10.0.0.7    | Linux             | East US       |
| ELK       | ELK-Monitor Log Server | 10.1.0.5    | Linux             | East US 2     |

***

**Access Policies**

The DVWA Web-App machines on the internal network are not exposed to the public Internet.

Only the **Jump Box** provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresse:

- 72.94.212.21

As well the Web-App and the ELK VMs within the network can only be accessed by the Ansible container running on the jumpbox or authorized sysadmin using ssh from the jumpbox ip 10.0.0.4.

A summary of the access policies in place can be found in the table below.

|    Name   |  Publicly Accessible Yes-Y/No-N?  | Allowed IP Addresses  | MS Azure Zone |
|:---------:|:---------------------------------:|:---------------------:|---------------|
| Jump Box  |                 Y                 |      72.94.212.21     |    East US    |
|   Web-1   |                 N                 |        10.0.0.4       |    East US    |
|   Web-2   |                 N                 |        10.0.0.4       |    East US    |
|   Web-3   |                 N                 |        10.0.0.4       |    East US    |
|    ELK    |                 N                 |        10.0.0.4       |   East US 2   |

 ***
**Elk Configuration**

Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible ensures software deployment and configurations are consistent across all pertinent servers for application viabiity.

The Ansible playbook was tooled to propel ELK deployment automation. This playbook is created in YAML and implements the following tasks:

- Install docker.io
- Install python3-pip
- Install Docker python module
- Increase memory for Docker container
- Download / Launch Docker elk container
- Enable service docker on boot

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

<https://github.com/Laylaray1116/MS-Azure-Cloud-Project1/blob/main/Images/elk_docker_ps_output.PNG>

***
**Target Machines & Beats**

This ELK server is configured to monitor the following machines:

- 10.0.0.4
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:

- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat
  - Filebeat is gathering logs to provide

- Metricbeat
  - for now we've enacting the system module allowing Metricbeat to collect server metrics regarding CPU performance, OS, Memory utilization and important network I/O realtime activity.

***
**Try this yourself. Using the Ansible filebeat-playbook.yml playbook**

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

- SSH into the control node and follow the steps below:
    - obtain/download the *filebeat-config.yml* template from GitHub
    - edit the *filebeat-config.yml* file by the following:
        1. verify / enter username is *elastic* and the password is *changme*.
        2. ensure the hostip is changed to your elk node ip keeping the port intact
        3. Under kibana setup; ensure the hostip is changed to your elk node ip keeping the port intact
        4. save the *filebeat-config.yml* file in /etc/ansible/files
    - Run the playbook. This will copy the edited *filebeat-config.yml* file to *filebeat.yml* file and enable, configure, setup and start filebeat on all VMs earmarked for monitoring.
    - Upon playbook completion, navigate to **http://[your.elk.VM.IP]:5601/app/kibana** to check that the installation worked as expected--that the ELK stack is receiveing log data from the monitored VMs.

**Commands for reference**

 - **Open a terminal and SSH into your jump box:** *ssh username@jumpboxpubip*
 - **Start the Ansible container:**
    *sudo docker start [docker container name]*
- **Use the correct Docker command to attach to your Ansible container:**
    *sudo docker attach [docker container name]*
- **Display running docker containers:** *sudo docker ps*
- **Download filebeat-config.yml template from GitHub. Run:**
    - *curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml*
