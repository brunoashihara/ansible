# ANSIBLE

Ansible labs

## Checklist

1. Arquitetura do lab
2. Instalação ansible
3. Criar config
4. Criar inventario e ansible_host
5. Criar playbooks

## Arquitetura do lab

1. Para esse laboratório foi criado 5 VMs com um sudo user chamado ***lab***:

   + ans-control: Control node (Ubuntu 22.04);
   + ans-wk01: Managed node (Ubuntu 22.04);
   + ans_wk02: Managed node (Ubuntu 22.04);
   + ans_wk03: Managed node (CentOS Stream 9);
   + ans_wk04: Managed node (CentOS Stream 9);

2. Grupos no ansible:

   + ubuntu: ans-wk01 e ans-wk02;
   + centos: ans-wk03 e ans-wk04;
   + apache: ans-wk02 e ans-wk04;
   + nginx: ans-wk01 e ans-wk03;
   + mysql: ans-wk01 e ans-wk04;
   + pgs: ans-wk02 e ans-wk03;
   + datacenter: todos os grupos;

3. Playbooks:

   + Criar users e acesso via ssh-key
   + Instalar apache;
   + Instalar nginx;
   + Instalar mysql; **EM CONSTRUÇÃO**
   + Instalar postgres; **EM CONSTRUÇÃO**

## Instalar ansible

1. Ubuntu

```bash
   $ sudo apt update
   $ sudo apt install software-properties-common
   $ sudo add-apt-repository --yes --update ppa:ansible/ansible
   $ sudo apt install ansible
```

2. CentOS

```bash
   dnf config-manager --set-enabled crb
   dnf install epel-release epel-next-release -y
   dnf install ansible -y
```

## criar config

1. Crie uma pasta e rode o ***ansible-config*** para criar um arquivo de config para este lab, conforme o exemplo abaixo:

```bash
   mkdir -p ~/ansible
   cd ~/ansible
   ansible-config init --disabled -t all > ~/ansible/ansible.cfg
```

2. Você também pode optar por utilizar um git clone para criar a pasta ansible e já baixar todos os arquivos:

```bash
   cd ~/
   git clone https://github.com/brunoashihara/ansible.git
   cd ~/ansible
   ansible-config init --disabled -t all > ~/ansible/ansible.cfg
```

## Criar inventario e ansible_host

1. Podemos usar arquivos com extensão yaml (declarativa) ou ini. Para esse lab vamos fazer do formato yaml

   + Caso tenha um dns, adicione os IPs dos hosts ao dns, ou adicione localmente no ***/etc/hosts***.
   + Cada grupo de hosts destinam a workloads (nodes) específicos.

2. Após criado o arquivo de inventário, podemos criar um outro arquivo chamado ***ansible_host*** para apresentar as variaveis e configurações de cada host

   + Neste lab utilize os parametros:

        + **ansible_host:** IP do host em questão;
        + **ansible_user:** Usuário para autenticar na hora da execução das tasks;
        + **ansible_ssh_private_key_file:** chave ssh criada;

3. Rode o seguinte comando para listar e verificar possíveis erros ou falta de parametros/configurações em seu inventário:

```bash
   ansible-inventory -i inventory.yaml -i ansible_host --list
```

## Criar playbooks

1. Para este lab criaremos duas chaves para acesso aos users ***teste*** e ***ansible***

```bash
   ssh-keygen -t rsa -b 4096 -C "ansible" -f "/home/"$USER"/.ssh/id_rsa_ansible" -q
   ssh-keygen -t rsa -b 4096 -C "teste" -f "/home/"$USER"/.ssh/id_rsa_teste" -q
   chmod 600 ~/.ssh/id_rsa_ansible
   chmod 600 ~/.ssh/id_rsa_teste
```

2. Antes de criar ou executar novos playbooks, para este lab vamos criar users especificos para o ansible nos *managed nodes* utilizando o playbook *create-user.yaml*

   + Para o comando abaixo substituir o campo ***lab*** com algum usuário que tenha permissões (sudo) nos ***Managed nodes***;
   + O comando -kK serve para autenticação, o primeiro k (minusculo) serve para autenticar via user ssh e o segundo K (maiusculo) serve para autenticação sudo;
   + Em caso de erro
```bash
   export ANSIBLE_HOST_KEY_CHECKING=False
   ansible-playbook create-user.yaml -i inventory.yaml  -u lab -kK
```

3. Teste se agora é possível mandar comandos sem precisar passar senhas, apenas com as chaves ssh com o comando:

```bash
   export ANSIBLE_HOST_KEY_CHECKING=True
   ansible all -i inventory.yaml -i ansible_host -m ping
```

4. Pronto ambiente está preparado para criação dos próprios playbooks e testes!

## Referências

+ [Ansible docs](https://docs.ansible.com/ansible/latest/getting_started/index.html)
+ [Criação de user](https://minimum-viable-automation.com/ansible/use-ansible-to-create-user-accounts-and-setup-ssh-keys/)
