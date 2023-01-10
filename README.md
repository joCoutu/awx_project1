# ansible

```sh
# bash: like notebook in AI
# ansible: idempotence, template, multi-machine, multi-os
# awx: shedule, rbac, history, webhook, cicd, infrastructure as code

# Ansible concept
https://www.jocoutu.me/bash/ansible/

# Ansible Collection - jocoutu.bashtop (nginx) - ansible_library
http://github.com/jocoutu/bashtop

# awx_project1 - bashtop usage - (nginx + dependencies + inventory) - awx_admin
http://github.com/jocoutu/awx_project1

# awx web interface - (run nginx) - awx_user

pip3 install ansible-core
```

#### setup

```sh
WORKDIR='/home/z/jocoutu' # full path and do not use ~
mkdir $WORKDIR
cd $WORKDIR
git clone https://github.com/jocoutu/awx_project1 awx_project1
git clone https://github.com/jocoutu/bashtop bashtop

# config localhost with a playbook, -e,-k,-K standard ansible ad hoc arguments, path=role arguments
ansible-playbook -i 'localhost,' bashtop/roles/ansible/tests/test.yml -e path=$WORKDIR
source ~/.bashrc

# bashtop/ | inventory | role | args
b 'localhost,' ansible -e alias=a -e path=$WORKDIR/awx_project1
source ~/.bashrc

a 0_ssh.yml  # a shortcut to:  ansible-playbook -i awx_project1/inventory.yml awx_project1/0_ssh.yml
a 1_awx.yml
#  awx_project1/1_awx.yml
# ---
# - name: Play_0 create vm
#   hosts: proxmox_1
#   roles:
#     - { role: proxmox, dest: 111 }
#     - { role: proxmox, dest: 165, ram: 16000, core: 4, cpu: host }
# - name: Play_1 dns server
#   hosts: dnsmasq_1
#   roles:
#     - { role: docker }
#     - { role: dnsmasq }
# - name: Play_2 gitea
#   hosts: k3s_1
#   roles:
#     - { role: k3s }
#     - { role: gitea }
# - name: Play_3
#   hosts: k3s_1
#   roles:
#     - { role: awx, install: true }
```

#### bashtop

```sh
# Just some examples, you don't need to run this

b 'localhost,' proxmox -e dest=111 -K # use proxmox on localhost and create vm with ip 192.168.1.111
b '192.168.1.111,' docker -K # on 192.168.1.111, install docker and -K ask sudo password
b '192.168.1.111,' dnsmasq  -K

b 'localhost,' proxmox -e dest=164 -K -e ram=16000 -e core=4 -e cpu=host -K
b '192.168.1.169,192.168.1.111' k3s -K # install k3s on 2 hosts
b '192.168.1.169,' awx  -K
b '192.168.1.169,' gitea -e dest=test3 -K # install gitea and tea remote cli if not exist and create a new repo 'test3'

b 'localhost,' proxmox -e cmd=destroy -e src=100 -e dest=170 -K # delete al vm from 192.168.1.100 to 170

# extra
c 162 # ssh z@192.168.1.162 -i k
g fistCommit # git add . && git commit -m $1 && git push
```
