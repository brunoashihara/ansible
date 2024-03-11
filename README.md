# ANSIBLE

Ansible labs

## Checklist

1. Arquitetura do lab
2. Instalação ansible
3. Criar inventario e ansible_host
4. Criar config
5. Criar playbooks

## Arquitetura do lab

1. Para esse laboratório foi criado da seguinte forma:

  + ans-control: Control node (UBUNTU)
  + ans-wk01: Managed node (UBUNTU)
  + ans-wk02: Managed node (UBUNTU)
  + ans-wk03: Managed node (CENTOS)
  + ans-wk04: Managed node (CENTOS)

2. Grupos no ansible

  + ubuntu
  + centos
  + apache
  + nginx
  + mysql
  + pgs
  + datacenter

## Referências

+ [Ansible docs](https://docs.ansible.com/ansible/latest/getting_started/index.html)
+ [Criação de user](https://minimum-viable-automation.com/ansible/use-ansible-to-create-user-accounts-and-setup-ssh-keys/)
