# awx

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
```

#### Install

```sh
pip3 install ansible-core

BASHTOP_WORKDIR='/home/z/jocoutu/' # full path and do not use ~
mkdir $BASHTOP_WORKDIR
cd $BASHTOP_WORKDIR

KEY='k2'
ssh-keygen -b 4096 -t rsa -f $KEY -q -N ""
cat $KEY.pub | ssh $USER@localhost "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

echo "[defaults]
remote_user = $USER
private_key_file = $KEY
roles_path = bashtop/roles/
stdout_callback = debug
gathering = explicit
bin_ansible_callbacks=True
callbacks_enabled = profile_tasks" > ansible.cfg

git clone https://github.com/jocoutu/bashtop bashtop
ansible -i 'localhost,' all -m include_role -a 'name=ansible' -e cmd=install -e dest=$BASHTOP_WORKDIR -K
git clone https://github.com/jocoutu/awx_project1 awx_project1
ansible-playbook -i awx_project1/inventory.yml awx_project1/awx.yml -K
source ~/.bashrc
```

#### Awxkit

```sh
# Use web interface to know mandatory field,
# and much simpler no example in the docs

a # no bash completion just run command to get help
a me
# awx --conf.host https://awx.z --conf.username admin --conf.password Ansible123! --conf.insecure me

a project clone --organization 'Default' --name 'awx_project1' --scm_type git --scm_url 'https://github.com/joCoutu/awx_project1'

a credentials clone --organization 'Default' --name 'k1' --credential_type 'Machine' # --inputs '{"username": "z", "ssh_key_data": "@k1"}' and username/ask passwd
# https://awx.z/#/credentials/6/edit - ssh_key_data - k1

a inventory clone --organization 'Default' --name 'inv1' --source_project 'awx_project1' #
# https://awx.z/#/inventories/inventory/4/sources/add - inv_source1 - Sourced from a project - 'awx_project1' - file

# a job_templates clone --name='Template1' --project 'awx_project1' --inventory 'inv1' --playbook '1_awx.yml' and sshkey
# argument --timeout: conflicting option string: --timeout

a job_templates launch 'Template1' --monitor
a jobs list --all --name 'Template1' --filter 'name,created,status'
# https://docs.ansible.com/ansible-tower/latest/html/towercli/reference.html
```

#### Collection

```sh
# https://docs.ansible.com/ansible/latest/collections/awx/awx/credential_module.html#ansible-collections-awx-awx-credential-module
```
