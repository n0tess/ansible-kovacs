## Challenge 1 

### Étape 1

Démarrage de la VM ubuntu : 

```bash
vagrant up ubuntu
```

### Étape 2 

Connexion à la VM : 

```bash
vagrant ssh ubuntu
```

### Étape 3 

Mise à jour des informations des paquets :

```bash
sudo apt update
```

### Étape 4 

Recherche du paquet Ansible :

```bash
apt-cache search --names-only ansible
```

### Étape 5 

Installation du paquet Ansible : 

```bash
sudo apt install ansible -y
```

### Étape 6 

Vérification de la version d'Ansible :

```bash
ansible --version 
    ansible 2.10.8
```

### Étape 7

Déconnexion puis suppression de la VM

```bash
exit
vagrant destroy -f ubuntu
```