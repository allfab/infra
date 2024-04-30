# Config

export ANSIBLE_CONFIG=/mnt/c/Users/fallamanche/Documents/Github/allfab/infra
echo $ANSIBLE_CONFIG

ansible-config init --disabled > ansible.cfg
ansible-config view

# Vault

export ANSIBLE_VAULT_PASSWORD_FILE=".vault-password"
touch .vault-password
vi .vault-password
#!/bin/bash
echo the_password

ansible-vault create group_vars/all/vault.yml
ansible-vault edit group_vars/all/vault.yml

ansible -i "127.0.0.1," all --ask-vault-pass -m debug -a "var=pve_api_user"

ansible -i "127.0.0.1," morpheus --ask-vault-pass -m debug -a "var=pve_api_user"
ansible -i "127.0.0.1," morpheus-monitoring -m debug -a "var=pve_api_user"
ansible -i hosts.ini all -m debug -a "var=pve_api_user"
ansible -i hosts.ini 192.168.10.5 -m debug -a "var=pve_api_user"
ansible -i hosts.ini morpheus -m debug -a "var=pve_api_user"

# Inventory


ansible-inventory -i hosts.ini --graph


# Playbook

# Sans les secrets
ansible-playbook -i hosts.ini playbooks/monitoring-stack.yml -u root -k

## Avec les secrets
ansible-playbook -i hosts.ini playbooks/monitoring-stack.yml --ask-vault-pass -u root -k

En mode verbeux :
ansible-playbook -i hosts.ini playbooks/monitoring-stack.yml --ask-vault-pass -u root -k -vvv
