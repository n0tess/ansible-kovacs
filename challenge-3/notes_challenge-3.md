## Challenge 3

### Étape 1

Démarrage de la VM Rocky : 

```bash
vagrant up rocky
```

### Étape 2 

Connexion à la VM : 

```bash
vagrant ssh rocky
```

### Étape 3

Mise à jour des informations des paquets : 

```bash
sudo dnf update
```

### Étape 4 

Ajout du dépôt tiers EPEL : 

```bash
sudo dnf install -y epel-release
```
puis activation du dépôt CRB :
```bash
sudo crb enable
```

### Étape 5

Installation de PIP et Virtualenv :

```bash
sudo dnf install -y python3-pip
```

### Étape 6 

Initialisation de l'environnement Virtualenv :

```bash
python3 -m venv ~/.venv/ansible
```

### Étape 7 

Démarrage du Virtualenv : 

```bash
source ~/.venv/ansible/bin/activate
```

Mise à jour de PIP :

```bash
pip install --upgrade pip
```

Installation d'Ansible :

```bash
pip install ansible
```

Vérification de la version : 

```bash
ansible --version
    ansible [core 2.15.13]
```

Pour quitter le Venv : 

```bash
deactivate
```

### Étape 8

Suppression de la VM :

```bash
vagrant destroy -f rocky
```

