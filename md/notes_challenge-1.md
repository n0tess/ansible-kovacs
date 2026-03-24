## Challenge 1 

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

Mise à jour des informations des paquets :

```console
$ sudo apt update
```

### Étape 4 

Recherche du paquet Ansible :

```console
$ apt-cache search --names-only ansible
```

### Étape 5 

Installation du paquet Ansible : 

```console
$ sudo apt install ansible -y
```

### Étape 6 

Vérification de la version d'Ansible :

```console
$ ansible --version 
    ansible 2.10.8
```

### Étape 7

Déconnexion puis suppression de la VM

```console
$ exit
$ vagrant destroy -f ubuntu
```