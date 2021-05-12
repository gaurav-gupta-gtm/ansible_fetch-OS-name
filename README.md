# Task Description

### Create an Ansible Playbook which dynamically load the variable file named same as OS_Name and just by using the variable names we can Configure our target node.
**Note: No need to use when keyword here.**
_____________________________________________

# Steps:

### To show this Demo, I'm creating a playbook which install webserver on target node by detect the OS-Name and load that variable file named same as OS-Name.

## System requirements

- Deployment environment have Ansible installed and configured.

## Usage

Add the system information gathered above into a file called `hosts`

```
[web]
13.233.255.243
```

## Playbook

```yaml
- hosts: web
  vars_files:
  - "./vars/{{ ansible_distribution }}.yml"
  tasks:
  - name: Install software
    package:
      name: "{{ package_name }}"
      state: present
  - name: copy web pages
    template:
      src: ./index.html
      dest: /var/www/html
  - name: start services
    service:
      name: "{{ package_name }}"
      state: started
      enabled: yes
```

Using keyword `ansible_distribution`, it detect the OS-Name and use the same var file named as OS Name. 

## Vars_files

If deteced OS is Ubuntu, it use file named as `/vars/Ubuntu.yml` and if OS is RedHat, it uses the file named as `/vars/RedHat.yml`. Set other variables according to your need.

- Ubuntu.yml
```yml
package_name: apache2
```

- RedHat.yml
```yml
package_name: httpd
```

Ansible-playbook fetch the value of variable `package_name` and use the package according to that.

After going through the setup, run the `webserver.yaml` playbook:

```sh
$ ansible-playbook webserver.yml
```
Using RedHat OS as target Node
-----------------------------
![image](https://user-images.githubusercontent.com/46498235/118032215-07558700-b385-11eb-9315-1b3132406ee0.png)
![image](https://user-images.githubusercontent.com/46498235/118032307-26ecaf80-b385-11eb-8410-9e58ecb7bfbb.png)

Using Ubuntu OS as target node
--------------------------
![image](https://user-images.githubusercontent.com/46498235/118032778-aaa69c00-b385-11eb-956a-3b15f9eb5b71.png)
![image](https://user-images.githubusercontent.com/46498235/118032853-be520280-b385-11eb-8a3d-e403be6b268e.png)

