<h6>Odoo</h6>
<h1 align="center">Instalação e configuração do Odoo 12</h1>
<h6 align="center">by Keiko</h6>

Esse é um script de execução bem simples para facilitar a instalação do Odoo em um SO Ubuntu.

<h3>Pré-requisitos</h3>

  - Ubuntu 20.04 | Ubuntu 18.04 Server
  
 <h3>Guia de instalação</h3>
 
 Faça o clone deste projeto utilizando o comando: 
 
 `git clone https://github.com/keikomori/odoo-installer/ && cd odoo-installer`
 
 Execute no terminal:
 
 `chmod +x run_pack.sh`
 
 > Esse comando dará permissão para o arquivo ser executado.
 > Você poderá verificar se a permissão do arquivo utilizando o comando `ls -l run_pack.sh`
 
Execute o script de instalação dos pacotes e dependencias

`./run_pack.sh`

<h3>Criação do usuário Odoo</h3>

Com todos os pacotes e dependencias instaladas do `run_pack.sh` vamos criar uma conta de usuário Odoo. Para que isso ocorra executaremos o comando abaixo:

`sudo useradd -m -d /opt/odoo -U -r -s /bin/bash odoo`

> O diretório doméstico do usuário será `opr/odoo/`

<h3>Criação do banco de dados</h3>

Quando executamos `run_pack.sh` foi realizada também a instalação do `postgresql`. Neste passo iremos realizar a configuração.

Iniciamos habilitando e iniciando o servidor de banco de dados:

`sudo systemctl enable --now postgresql.service`

Em seguida, crie um usuário do banco de dados `postgresql` para **Odoo** e uma nova conta de usuário chamada **Odoo**

`sudo su - postgres -c "createuser -s odoo"`

> esse comando acima faz o login e cria um usuário de banco de dados

`sudo su - postgres -c "createdb odoo"`

> cria uma nova conta de usuário de banco de dados chamado odoo

Para criar uma senha para o usuário
`sudo su - postgres`

`psql`

`alter user odoo with encrypted password 'odoo';`

Por fim, conceda todos os privilégios ao usuário **odoo** no banco de dados *odoo*

> ainda dentro do `#psql`

```
grant all privileges on database odoo to odoo;

\q
```

<h3>Instalação e configuração do Odoo</h3>

Com o ambiente preparado para receber o Odoo iremos realizar a instalação e configuração dele. Para isso vamos mudar para a conta de usuário odoo que criamos lá no inicio:

`sudo su - odoo`

Em seguida, clonar o pacote Odoo, utilizamos a versão 12 neste projeto.

`git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0 --single-branch /opt/odoo/odoo12`

Com a programa fonte do odoo clonado, criamos um ambiente virtual do Odoo no Python

`cd /opt/odoo/odoo12`

`python3 -m venv odoo-venv`

Ativamos o ambiente:

`source odoo-venv/bin/activate`

E instalamos todos os módulos do Python com o pip3:

`pip3 install wheel`

`pip3 install -r requirements.txt`

Desativamos o ambiente virtual com o comando:

`deactivate`

Para criar um novo diretório para os addons personalizados:

`mkdir /opt/odoo/odoo12/odoo-custom-addons`

E, finalmente, voltar para o usuário `sudo`:

`exit`

<h3>Para executar o odoo como um serviço</h3>

 Iniciamos criando um arquivo na unidade de serviço do diretório

`sudo nano /etc/systemmd/system/odoo12.service`

Colamos a seguinte configuração:

```
[Unit]
Description=Odoo12
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=odoo12
PermissionsStartOnly=true
User=odoo
Group=odoo
ExecStart=/opt/odoo/odoo12/odoo-venv/bin/python3 /opt/odoo/odoo12/odoo/odoo-bin -c /etc/odoo12.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```

Iniciamos o serviço:

```
sudo systemctl daemon-reload
sudo systemctl start odoo12
```

Para verificar o `status` do serviço utilizamos o comando:

`sudo systemctl status odoo12`

A saída deve ser similar ao indicado abaixo:

```

```

Para habilitar que o serviço Odoo seja iniciado automaticamente na inicialização:

`sudo systemctl enable odoo12`
