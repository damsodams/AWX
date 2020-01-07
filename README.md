# AWX
Installation d'Ansible-AWX l'alternative gratuite de ansible-tower sur CentOs7
avec docker-compose.

## Pre-requis 
# Matériel:
  - 4GB RAM.
  - 20 GB d'espace dédié sur le disque dur.
  - Processeur 64 Bit
  - 2 CPU minimum.
# Logiciel:
  - PostgreSQL version 10 
  - Ansible version 2.2 

## Installation
Pour commencer il est neccessaire de metre a jour le systeme. 
```
yum check-update
yum update
```
Installation d'ansible: 
```
yum install ansible
```
Installation de python 3 : 
```
sudo yum install centos-release-scl
sudo yum install rh-python36
scl enable rh-python36 bash
```
Installation de pip :
```
sudo yum install epel-release
sudo yum install python-pip
```
Installation de docker-compose : 
```
sudo yum install docker-compose
```
Créer le fichier install_aws.yml qui contiendra le playbook d'installation AWX:
```
touch install_aws.yml
```
Ouvrer le avec votre éditeur préféré
```
vi install_aws.yml
```
et insérez le code suivant :
```
- name: Deploy AWX
hosts: all
become: true
become_user: root

tasks:

- name: sort out the yum repos
yum:
name: "{{ item }}"
state: "latest"
with_items:
- "epel-release"
- "yum-utils"

- name: add the docker ce yum repo
yum_repository:
name: "docker-ce"
description: "Docker CE YUM repo"
gpgcheck: "yes"
enabled: "yes"
baseurl: "https://download.docker.com/linux/centos/7/$basearch/stable"
gpgkey: "https://download.docker.com/linux/centos/gpg"

- name: install the prerequisites using yum
yum:
name: "{{ item }}"
state: "latest"
with_items:
- "epel-release"
- "libselinux-python"
- "python-wheel"
- "python-pip"
- "git"
- "docker-ce"

- name: start and enable docker
systemd:
name: "docker"
enabled: "yes"
state: "started"

- name: install the python packages using pip
pip:
name: "{{ item }}"
state: "latest"
with_items:
- "pip"
- "ansible"
- "boto"
- "boto3"
- "docker"

- name: check out the awx repo
git:
repo: "https://github.com/ansible/awx.git"
dest: "~/awx"
clone: "yes"
update: "yes"

- name: install awx
command: "ansible-playbook -i inventory install.yml"
args:
chdir: "~/awx/installer"
```
Dans /etc/ansible/hosts ajouter l'ip de votre machine puis 
Exécutez le playbook :
```
ansible-playbook -i hosts install_aws.yml
```
Une fois le playbook exécuté vous devriez avoir des container docker monté:
```
docker ps -a
```
Connectez vous sur l'interface Web via l'url suivante: http://<ipoudnsdevotremachine>
Attendez que l'installation de AWX se finalise.
 Une fois l'installation terminée authentifiez vous avec les identifiants par défaut (login : admin, mot de passe: password):
