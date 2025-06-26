# Ansible-task
steps:
1.setup 3 EC2 machines :
 1.1.visit :(https://console.aws.amazon.com/ec2)
 1.2.fill launch form:
   *name : control-node , frontend-node , docker-node
   *AMI: ubuntu 24.04 
   *instance type: t3.micro
   *key pair: create (ansible-key.pem) .. note : we will use it in the three machines 
   *network setting : allow SSH for 3 machines .. note: for frontend-node add (HTTP)
   *launch instance
  note : don't forget to install ansible-key.pem
2.connect control node from your local pc:
 2.1.open cmd 
 2.2.set SSH into control node :
   *ssh -i "ansible-key.pem" ubuntu@(control-node-public-ip) "command"
   *type yes
3.setting SSH to other nodes:
 3.1.inside the control node:
  *nano ansible-key.pem "command"
  *Paste the entire contents of ansible-key.pem file into it (key)
  *crtl+o (to save)
  *enter 
  *ctrl+x (to exit)
  *chmod 400 ansible-key.pem (to make strict permissions "Only the file owner can read it")
4.SSH into docker and frontend nodes:
 4.1.ssh -i ansible-key.pem ubuntu@(frontend-node-public-ip) "command"
 4.2.ssh -i ansible-key.pem ubuntu@(docker-node-public-ip) "command"
5.installing ansible on the control nodes:
  sudo apt update "command"
  sudo apt install ansible -y "command"
6.inventory file :
 6.1.nano inventory.ini "command"
 6.2. paste :
        [frontend]
        frontend-node ansible_host=(frontend-node-public-ip) ansible_user=ubuntu               
        ansible_ssh_private_key_file=ansible-key.pem
        [docker]
        docker-node ansible_host=(docker-node-public-ip) ansible_user=ubuntu 
        ansible_ssh_private_key_file=ansible-key.pem
7.Frontend Playbook (frontend.yml)
  7.1.nano frontend.yml
  7.2.then build the yml file structure and don't forget to add the feature of the HTTP
8.Docker Playbook (docker.yml)
  8.1.nano docker.yml
  8.2..then build the yml file structure
9.Test Ping 
  *ansible -i inventory.ini all -m ping "command"
10.testing frontend 
  10.1.ansible-playbook -i inventory.ini frontend.yml "command"
  10.2.then visit : (http://(frontend-node-public-ip))
11.testing docker 
  *ansible-playbook -i inventory.ini docker.yml "command"
  

  

 









