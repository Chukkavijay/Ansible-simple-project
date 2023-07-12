# Ansible-simple-project
                         # Ansible-simple-project #
Introduction: 
- Ansible is an open-source automation tool used for configuration management,
application deployment, and orchestration.
- It follows a declarative approach, where you define the desired state of the
system rather than scripting specific commands.
- Ansible uses SSH for communication with target systems, making it agentless
and easy to set up.

Inventory: The inventory file lists the target hosts on which Ansible will run
tasks. It can be a static file or generated dynamically.

Playbooks: Playbooks are YAML files that define a set of tasks and
configurations to be executed on target hosts.

Tasks: Tasks are the individual units of work in Ansible. They represent actions
to be performed on target hosts.

Modules: Ansible provides a wide range of modules for various tasks, such as
package installation, file manipulation, service management, etc.

Roles: Roles are reusable units of playbooks. They encapsulate related tasks,
handlers, variables, and files into a directory structure.


Installation:
Ansible master and 3 other servers

Launch a server:
 Launch an Ec2 instance like Ansible-master(ubuntu 22.04)
Connect to instance using ssh:
  ssh -i "<private_key.pem>" ubuntu@<instance_dns/public_ip>
  #example -> ssh -i "ansible-key.pem" ubuntu@ec2-44-203-3-231.compute-1.amazonaws.com
 -> sudo apt update
 -> sudo apt install software-properties-common -y
 -> sudo add-apt-repository --yes --update ppa:ansible/ansible
 -> sudo apt install ansible -y
#create folder 
 -> mkdir keys

Launch 3 servers:
 launch 3 server using same .pem file (used for master server key)
 using ssh we connect these 3 server from master.
 Then we execute commands on master to push 3 servers.

Ansible master:
 The private_key.pem must be in server, This should easy for connecting 3 server.
 copy from local to server
 -> scp -i "<private_key.pem>" <private_key.pem> ubuntu@<instance_dns/public_ip>:/home/ubuntu/keys/
 #example => scp -i "ansible-key.pem" ansible-key.pem ubuntu@ec2-44-203-3-231.compute-1.amazonaws.com:/home/ubuntu/keys/
 # ssh -i "ansible-key.pem" ubuntu@ec2-44-203-3-231.compute-1.amazonaws.com
 Then connect to server and check file copied or not 
 -> ssh -i "<private_key.pem>" ubuntu@<instance_dns/public_ip>

Using ansible_host update server_ips and varibales

  -> sudo nano /etc/ansible/hosts
     [servers]
     server1 ansible_host=<public_ip>
     #server1 ansible_host=3.91.57.47
     server2 ansible_host=3.83.86.226
     server3 ansible_host=3.82.227.187
     
     [servers:vars]
     ansible_python_interpreter=/usr/bin/python3
     ansible_user=ubuntu
     ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-key.pem

  #you can check like
   -> ansible-inventory --list -y

Ansible Adhoc commands:
  -> ad hoc commands are great for tasks you repeat rarely
  -> -a is used for adhoc commands
  -> ansible all -a "df -h" -u ubuntu
  -> ansible servers -a "uptime" -u ubuntu

Ansible modules are units of code that can control system resources or execute system commands:
  -> -m is used for modules
  -> ansible all -m ping -u ubuntu

Ansible Playbooks:

vi date_playbook.yml
#vim date_playbook.yml
##################
-
 name: Display Date using ansible-playbook
 hosts: server   
 tasks:
  - name: display the system date
    command: date
###################
ansible-playbook date_playbook.yml -vv


vi nginx_playbook.yml
#nano nginx_playbook.yml
###################
-
 name: This playbook will install nginx
 hosts: servers
 become: yes
 tasks:
  - name: install nginx
    apt:
      name: nginx
      state: latest
  - name: start nginx
    service:
      name: nginx
      state: started
      enabled: yes
######################
ansible-playbook nginx_playbook.yml -vv


vi docker_playbook.yml
#vim docker_playbook.yml
#######################
-
 name: playbook perform intial update & install docker
 hosts: servers
 become: yes
 tasks:
   - name: intial update
     apt:
       update_cache: true
   - name: install Docker
     apt:
       name: docker.io
       state: latest
############################
ansible-playbook docker_playbook.yml -vv


Thank you 
#Special Thank @TrainWithShubham
#youtube video https://www.youtube.com/live/yyU2UiNI08M?feature=share
