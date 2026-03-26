## Facts et variables implicites

### Étape 1

Écriture du playbook `pkg-info.yml` : 

```yaml
---  # pkg-info.yml

- hosts: all

  tasks:

    - name: Gather the package facts
      debug:
        var: ansible_pkg_mgr
...
```

Affiche :

![Alt text](atelier_16_pkg_info.png)


### Étape 2

Écriture du playbook `python-info.yml` :

```yaml
---  # python-info.yml

- hosts: all

  tasks:

    - name: Gather python version
      debug: 
        msg: "Python version: {{ ansible_python_version }}"
...
```

Affiche : 

![Alt text](atelier_16_python_info.png)


### Étape 3

Écriture du playbook `dns-info.yml` :

```yaml
---  # dns-info.yml

- hosts: all

  tasks:

    - name: Gather the package facts
      debug:
        var: ansible_dns.nameservers
...
```

Affiche : 

![Alt text](atelier_16_dns_info.png)
