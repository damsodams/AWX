# AWX
Installation d'Ansible-AWX l'alternative gratuite de ansible-tower sur CentOs7
avec docker-compose.

## Pre-requis 
# Matériel:
  - 4GB RAM.
  - 20 GB d'espace dédié sur le disque dur.
  - Processeur 64 Bit.
  - 2 CPU minimum.
# Logiciel:
Pour commencer il est neccessaire de metre a jour le systeme. 
```
yum check-update
yum update
```
Installation du repository EPEL
```
yum -y install epel-release
```
Installation des paquets et dépendances nécessaires 
```
yum -y install git gettext ansible docker nodejs npm gcc-c++ bzip2
yum -y install python-docker-py
```

## Installation

Démarrer et activer le service Docker
```
systemctl start docker
systemctl enable docker
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
récuperer le repos git de awx :
```
git clone https://github.com/ansible/awx.git
```
exécuter l'installer qui ce trouve dans "~/awx/installer":
```
ansible-playbook -i inventory install.yml
```

Une fois l'installer exécuté vous devriez avoir des container docker monté:
```
docker ps -a
```
Connectez vous sur l'interface Web via l'url suivante: http://ipoudnsdevotremachine
Attendez que l'installation de AWX se finalise.
 Une fois l'installation terminée authentifiez vous avec les identifiants par défaut (login : admin, mot de passe: password).
