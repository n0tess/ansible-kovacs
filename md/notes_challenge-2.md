## Challenge 2 

### Étape 1

Démarrage de la VM ubuntu : 

```console
$ vagrant up ubuntu
```

### Étape 2 

Connexion à la VM : 

```console
$ vagrant ssh ubuntu
```

### Étape 3 

Configuration d'un dépôt PPA pour Ansible : 

```console
$ sudo apt-add-repository ppa:ansible/ansible
```

### Étape 4

Mise à jour des informations des paquets :

```console
$ sudo apt-get update
```

### Étape 5

Installation d'Ansible en utilisant le dépôt PPA :

```console
$ sudo apt-get install ansible
```

### Étape 6 

Vérification de la version d'Ansible :

```console
$ ansible --version 
    ansible [core 2.17.14]
```
La version installée est plus récente que celle installée via le gestionnaire de paquet officiel. Cela est dû au fait que les développeurs peuvent proposer rapidement les versions récentes des logiciels. 


### Étape 7

Déconnexion puis suppression de la VM

```console
$ exit
$ vagrant destroy -f ubuntu
```