## Authentification

### Étape 1

Démarrage des VM :

```console
$ vagrant up
```

Connexion au Control Host : 

```console
$ vagrant ssh control
```

### Étape 2

Voici les étapes nécessaires pour réussir un `ping`:

Edition du fichier `/etc/hosts` :

```console
# /etc/hosts
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan      target01
192.168.56.30  target02.sandbox.lan     target02
192.168.56.40  target03.sandbox.lan       target03
```

Vérification de la connectivité : 

```console
$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```

Résultat : 

```console
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms

PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms

PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.
1 packets transmitted, 1 received, 0% packet loss, time 0ms
```

Collection des clés SSH publiques des Target Hosts : 

```console
$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

Mise en place de l'authentification par clé SSH depuis le Control Host : 

```console
$ ssh-keygen
```

Distribution de la clé publique sur les Target Hosts en fournissant à chaque fois le mot de passe `vagrant` :

```console
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

Test du fonctionnement de l'authentification sur les Target Hosts : 

```console
$ ansible all -i target01,target02,target03 -u vagrant -m ping

target02 | SUCCESS => {
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
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Deuxième test sans utiliser l'option `-u vagrant` :

```console
$ ansible all -i target01,target02,target03 -m ping

target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
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
```

Suppression des VM :

```console
$ vagrant destroy -f
```