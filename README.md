<h6>Odoo</h6>
<h1 align="center">Instalação e configuração do Odoo</h1>
<h6 align="center">by Keiko</h6>

Esse é um script de execução bem simples para facilitar a instalação do Odoo em uma nova máquina Ubuntu.

<h3>Pré-requisitos</h3>

  - Ubuntu 20.04 | Ubuntu 18.04 Server
  
 <h3>Instalação dos pacotes</h3>
 
 Faça o clone deste projeto utilizando o comando: 
 
 `git clone https://github.com/keikomori/odoo-installer/ && cd odoo-installer`
 
 Execute no terminal:
 
 `chmod +x run_pack.sh`
 
 > Esse comando dará permissão para o arquivo ser executado.
 > Você poderá verificar se a permissão do arquivo utilizando o comando `ls -l run_pack.sh`
 
Execute o script de instalação dos pacotes e dependencias

`/run_pack.sh`

<h3>Criação do usuário Odoo</h3>

Com todos os pacotes e dependencias instaladas do `run_pack.sh` vamos criar uma conta de usuário Odoo. Para que isso ocorra executaremos o comando abaixo:

`sudo useradd -m -d /opt/odoo -U -r -s /bin/bash odoo`

> O diretório doméstico do usuário será `opr/odoo/`

<h3>Criação do banco de dados</h3>

Quando executamos `run_pack.sh` foi realizada também a instalação do `postgresql`. Neste passo iremos realizar a configuração.

Iniciamos habilitando e iniciando o servidor de banco de dados:

`sudo systemctl enable --now postgresql.service`

Em seguida, crie um usuário do banco de dados `postgresql` para **Odoo** e uma nova conta de usuário chamada **Odoo**

```
# esse comando faz o login e cria um usuário de banco de dados
sudo su - postgres -c "createuser -s odoo"
```

```
#cria uma nova conta de usuário de banco de dados chamado odoo
sudo su - postgres -c "createdb odoo"
```

```
#cria uma senha para o usuário
sudo su - postgres psql
psql=# alter user odoo with encrypted password 'odoo';
```

Por fim, conceda todos os privilégios ao usuário **odoo** no banco de dados *odoo*

```
sudo -u postgres psql
grant all privileges on database odoo to odoo;

\q
```

<h3>Instalação e configuração do Odoo</h3>

Com o ambiente preparado para receber o Odoo iremos realizar a instalação e configuração dele. Para isso vamos mudar para a conta de usuário odoo que criamos lá no inicio:

`sudo su - odoo`

Em seguida, clonar o pacote Odoo, utilizamos a versão 12 neste projeto.

`sudo git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0 --single-branch /opt/odoo/odoo12`







