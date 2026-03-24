## Challenge 3

### Étape 1

Démarrage de la VM Rocky : 

```console
$ vagrant up rocky
```

### Étape 2 

Connexion à la VM : 

```console
$ vagrant ssh rocky
```

### Étape 3

Mise à jour des informations des paquets : 

```console
$ sudo dnf update
```

### Étape 4 

Ajout du dépôt tiers EPEL : 

```console
$ sudo dnf install -y epel-release
```
puis activation du dépôt CRB :
```
$ sudo crb enable
```

### Étape 5

Installation de PIP et Virtualenv :

```console
$ sudo dnf install -y python3-pip
```

### Étape 6 

Initialisation de l'environnement Virtualenv :

```console
$ python3 -m venv ~/.venv/ansible
```

### Étape 7 

Démarrage du Virtualenv : 

```console
$ source ~/.venv/ansible/bin/activate
```

Mise à jour de PIP :

```console
$ pip install --upgrade pip
```

Installation d'Ansible :

```console
$ pip install ansible
```

Vérification de la version : 

```console
$ ansible --version
    ansible [core 2.15.13]
```

Pour quitter le Venv : 

```console
$ deactivate
```

### Étape 8

Suppression de la VM :

```console
$ vagrant destroy -f rocky
```

