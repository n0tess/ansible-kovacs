## Challenge 2 

### Etape 1

Démarrage de la VM ubuntu : 

```bash
vagrant up ubuntu
```

### Etape 2 

Connexion à la VM : 

```bash
vagrant ssh ubuntu
```

### Etape 3 

Configuration d'un dépôt PPA pour Ansible : 

```bash
sudo apt-add-repository ppa:ansible/ansible
```

### Etape 4

Mise à jour des informations des paquets :

```bash
sudo apt-get update
```

### Etape 5

Installation d'Ansible en utilisant le dépôt PPA :

```bash
sudo apt-get install ansible
```

### Etape 6 

Vérification de la version d'Ansible :

```bash
ansible --version 
    ansible [core 2.17.14]
```
La version installée est plus récente que celle installée via le gestionnaire de paquet officiel. Cela est dû au fait que les développeurs peuvent proposer rapidement les versions récentes des logiciels. 


### Etape 7

Déconnexion puis suppression de la VM

```bash
exit
vagrant destroy -f ubuntu
```