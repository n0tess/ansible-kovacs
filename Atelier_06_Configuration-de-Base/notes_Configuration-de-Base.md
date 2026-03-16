## Configuration de base

### Étape 1

Démarrage des VM :

```
$ vagrant up
```

Connexion au *Control Host* :

```
$ vagrant ssh control
```

### Étape 2

Édition du fichier `/etc/hosts` de manière à ce que les *Target Hosts* soient joignables par leur nom d'hôte :

```
# /etc/hosts
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan      target01
192.168.56.30  target02.sandbox.lan     target02
192.168.56.40  target03.sandbox.lan       target03
```

Vérification de la connectivité : 

```
$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```

Résultat : 

```
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms

PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms

PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms
```


### Étape 3 

Configuration de l'authentification par clé SSH avec les trois *Target Hosts* : 

Collection des clés SSH publiques des Target Hosts : 

```
$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

Mise en place de l'authentification par clé SSH depuis le Control Host : 

```
$ ssh-keygen
```

Distribution de la clé publique sur les Target Hosts en fournissant à chaque fois le mot de passe `vagrant` :

```
$ ssh-copy-id vagrant@target01
...
vagrant@target01's password: *******
Number of key(s) added: 1
$ ssh-copy-id vagrant@target02
...
vagrant@target02's password: *******
Number of key(s) added: 1
$ ssh-copy-id vagrant@target03
...
vagrant@target03's password: *******
Number of key(s) added: 1
```

### Étape 4

Installation d'Ansible : 

```
$ sudo apt update && sudo apt install -y ansible && sudo apt install -y direnv
```

Vérification de la version installée : 

```
$ ansible --version
    ansible 2.10.8
```

### Étape 5

Envoi d'un premier ping Ansible sans configuration : 

```
$ ansible all -i target01,target02,target03 -m ping

target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### Étape 6

Création d'un répertoire de projet `~/monprojet` :

```
$ mkdir -v ~/monprojet
```

### Étape 7

Création d'un fichier vide `ansible.cfg` dans ce répertoire : 

```
$ touch ansible.cfg
```

### Étape 8

Vérification que ce fichier est bien pris en compte par Ansible : 

```
$ ansible --version |  head -n 2
    ansible 2.10.8
        config file = /home/vagrant/monprojet/ansible.cfg

```

Activation de Direnv :

```
$ echo 'eval "$(direnv hook bash)"' >> ~/.bashrc

$ source ~/.bashrc
```


Création d'un fichier `.envrc` à la racine du projet pour utiliser identifier mon fichier de configuration : 

```
~/monprojet/.envrc

export ANSIBLE_CONFIG=$(expand_path ansible.cfg)
```

Autorisation de l'utilisation de la variable d'environnement : 

```
$ direnv allow

direnv: loading ~/monprojet/.envrc
direnv: export ~ANSIBLE_CONFIG
```

Vérification de la bonne prise en compte de mon fichier `ansible.cfg`

```
$ pwd
/home/vagrant/monprojet

$ ansible --version | head -n 2
    ansible 2.10.8
        config file = /home/vagrant/monprojet/ansible.cfg
```

### Étape 9

Spécification d'un inventaire nommé `hosts` : 

```
$ touch hosts
```

Ajout de la directive dans le fichier `ansible.cfg` : 

```
[defaults]
inventory = ./hosts
```


### Étape 10 

Activation de la journalisation dans `~/journal/ansible.log` :

```
$ mkdir -v ~/journal
```

Définition de la journalisation dans `ansible.cfg` : 

```
[defaults]
inventory = ./inventory
log_path = ~/journal/ansible.log
```

### Étape 11

Test de la journalisation : 

```
$ ansible all -i target01,target02,target03 -m ping
...
$ cat ~/journal/ansible.log

2026-03-16 08:57:55,061 p=3316 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2026-03-16 08:57:55,086 p=3316 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2026-03-16 08:57:55,087 p=3316 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### Étape 12 

Création d'un groupe `[testlab]` avec les trois *Target Hosts* dans le fichier `hosts`: 

```
[testlab]
target01
target02
target03
```

### Étape 13

Définition d'un utilisateur `vagrant` pour la connexion aux cibles dans le fichier `hosts` : 

```
[testlab:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=vagrant
```

### Étape 14 

Envoi d'un `ping` Ansible vers le groupe de machines `[all]` :

```
$ ansible all -m ping

target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### Étape 15 

Définition de l'élévation des droits pour l'utilisateur `vagrant` sur les *Target Hosts* dans le fichier `hosts` : 

```
[testlab:vars]
ansible_become=yes
```

### Étape 16 

Affichage de la première ligne du fichier `/etc/shadow` sur tous les *Target Hosts* : 

```
$ ansible all -a "head -n 1 /etc/shadow"

target01 | CHANGED | rc=0 >> root:*:19977:0:99999:7:::
target03 | CHANGED | rc=0 >> root:*:19977:0:99999:7:::
target02 | CHANGED | rc=0 >> root:*:19977:0:99999:7:::
```

### Étape 17 

Quittez le *Control Host* et supprimez toutes les VM de l'atelier : 

```
$ exit

$ vagrant destroy -f
```

