## Challenge 1 

### Étape 1

Démarrage de la VM ubuntu : 

```
$ vagrant up ubuntu
```

### Étape 2 

Connexion à la VM : 

```
$ vagrant ssh ubuntu
```

### Étape 3 

Mise à jour des informations des paquets :

```
$ sudo apt update
```

### Étape 4 

Recherche du paquet Ansible :

```
$ apt-cache search --names-only ansible
```

### Étape 5 

Installation du paquet Ansible : 

```
$ sudo apt install ansible -y
```

### Étape 6 

Vérification de la version d'Ansible :

```
$ ansible --version 
    ansible 2.10.8
```

### Étape 7

Déconnexion puis suppression de la VM

```
$ exit
$ vagrant destroy -f ubuntu
```