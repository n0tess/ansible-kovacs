## Idempotence

### Étape 1

Installation successive des paquets `tree`, `git` et `nmap` sur toutes les cibles en se plaçant dans le dossier `ansible/projects/ema/` : 

```console
$ ansible all -m package -a "name=tree,git,nmap"
```

Installe correctement les paquets sur tous les `Target Hosts`.

Deuxième installation pour voir ce qu'il se passe : 

```console
$ ansible all -m package -a "name=tree,git,nmap"

suse | SUCCESS => {
    "changed": false,
    "name": [
        "tree",
        "git",
        "nmap"
    ],
    "rc": 0,
    "state": "present",
    "update_cache": false
}
debian | SUCCESS => {
    "cache_update_time": 1761245363,
    "cache_updated": false,
    "changed": false
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}
```

Ansible affiche des `SUCCESS` au lieu de redémarrer l'installation des paquets. 

### Étape 2 

Désinstallation successive de ces trois paqutes : 

```console
$ ansible all -m package -a "name=tree,git,nmap state=absent"
```

Les paquets sont bien désinstallés sur tous les `Target Hosts`.

Deuxième désinstallation pour voir ce qu'il se passe : 

```console
$ ansible all -m package -a "name=tree,git,nmap state=absent"

suse | SUCCESS => {
    "changed": false,
    "name": [
        "tree",
        "git",
        "nmap"
    ],
    "rc": 0,
    "state": "absent",
    "update_cache": false
}
debian | SUCCESS => {
    "changed": false
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}
```

Ansible affiche aussi un `SUCCESS`.

### Étape 3

Copie du fichier `/etc/fstab` du *Control Host* vers tous les *Target Hosts* sous forme d'un fichier `/tmp/test3.txt` : 

```console
$ ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"

debian | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1773654321.349653-6001-144519554598294/source",
    "state": "file",
    "uid": 0
}
suse | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1773654321.444943-6002-45628091499748/source",
    "state": "file",
    "uid": 0
}
rocky | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "secontext": "unconfined_u:object_r:user_home_t:s0",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1773654321.374424-6000-3283640513007/source",
    "state": "file",
    "uid": 0
}
```

Deuxième copie pour voir ce qu'il se passe :

```console
$ ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"

rocky | SUCCESS => {
    "changed": false,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "secontext": "unconfined_u:object_r:user_home_t:s0",
    "size": 679,
    "state": "file",
    "uid": 0
}
suse | SUCCESS => {
    "changed": false,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "size": 679,
    "state": "file",
    "uid": 0
}
debian | SUCCESS => {
    "changed": false,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "size": 679,
    "state": "file",
    "uid": 0
}
```

Ansible ne copie pas une seconde fois le fichier mais affiche un `SUCCESS`.

### Étape 4

Suppression du fichier `/tmp/test3.txt` sur les *Target Hosts* : 

```console
$ ansible all -m file -a "dest=/tmp/test3.txt state=absent"

debian | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
suse | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
```

Deuxième suppression pour voir ce qu'il se passe :

```console
$ ansible all -m file -a "dest=/tmp/test3.txt state=absent"

suse | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
debian | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
```

Comme d'habitude, affichage de `SUCCESS`.

### Étape 5 :

Affichage de l'espace utilisé par la partition principale sur tous les *Target Hosts* : 

```console
$ ansible all -m command -a "df -h /"

debian | CHANGED | rc=0 | (stdout) Filesystem       Size  Used Avail Use% Mounted on\n/dev/mapper/debian--12--vg-root   62G  1.7G   57G   3% /
rocky | CHANGED | rc=0 | (stdout) Filesystem        Size  Used Avail Use% Mounted on\n/dev/sda2        61G  2.1G   59G   4% /
suse | CHANGED | rc=0 | (stdout) Filesystem     Size  Used Avail Use% Mounted on\n/dev/sda3        64G  2.4G   58G   4% /
```

Je remarque que pour cette commande il n'y a pas d'idempotence.

