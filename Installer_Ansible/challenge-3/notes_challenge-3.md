## Challenge 3

### Étape 1

Démarrage de la VM Rocky : 

```
$ vagrant up rocky
```

### Étape 2 

Connexion à la VM : 

```
$ vagrant ssh rocky
```

### Étape 3

Mise à jour des informations des paquets : 

```
$ sudo dnf update
```

### Étape 4 

Ajout du dépôt tiers EPEL : 

```
$ sudo dnf install -y epel-release
```
puis activation du dépôt CRB :
```
$ sudo crb enable
```

### Étape 5

Installation de PIP et Virtualenv :

```
$ sudo dnf install -y python3-pip
```

### Étape 6 

Initialisation de l'environnement Virtualenv :

```
$ python3 -m venv ~/.venv/ansible
```

### Étape 7 

Démarrage du Virtualenv : 

```
$ source ~/.venv/ansible/bin/activate
```

Mise à jour de PIP :

```
$ pip install --upgrade pip
```

Installation d'Ansible :

```
$ pip install ansible
```

Vérification de la version : 

```
$ ansible --version
    ansible [core 2.15.13]
```

Pour quitter le Venv : 

```
$ deactivate
```

### Étape 8

Suppression de la VM :

```
$ vagrant destroy -f rocky
```

