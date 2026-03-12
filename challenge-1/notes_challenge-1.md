## Challenge 1 

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

Mise à jour des informations des paquets :

```bash
sudo apt update
```

### Etape 4 

Recherche du paquet Ansible :

```bash
apt-cache search --names-only ansible
```

### Etape 5 

Installation du paquet Ansible : 

```bash
sudo apt install ansible -y
```

### Etape 6 

Vérification de la version d'Ansible :

```bash
ansible --version 
    ansible 2.10.8
```

### Etape 7

Déconnexion puis suppression de la VM

```bash
exit
vagrant destroy -f ubuntu
```