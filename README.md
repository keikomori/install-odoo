<h6>Odoo</h6>
<h1 align="center">Instalação e configuração do Odoo 12</h1>
<h6 align="center">by Keiko</h6>

Esse é um script de execução bem simples para facilitar a instalação do Odoo em um SO Ubuntu.

<h3>Pré-requisitos</h3>

  - Ubuntu 20.04 | Ubuntu 18.04 Server
  
 <h2>Guia de instalação</h3>
 
 Faça o clone deste projeto utilizando o comando: 
 
 `git clone https://github.com/keikomori/odoo-installer/ && cd odoo-installer`
 
 Execute no terminal:
 
 `chmod +x run_pack.sh`
 
 > Esse comando dará permissão para o arquivo ser executado.
 > Você poderá verificar se a permissão do arquivo utilizando o comando `ls -l run_pack.sh`
 
Execute o script de instalação dos pacotes e dependencias

`./run_pack.sh`

<h2>Criação do banco de dados</h3>

Iniciamos habilitando e iniciando o servidor de banco de dados:

`sudo systemctl enable --now postgresql.service`

Criamos um banco de dados

`sudo su - postgres -c "createdb odoo"`

<h2>Instalação e configuração do Odoo</h3>

Em seguida, clonar o pacote Odoo, utilizamos a versão 12 neste projeto.

`git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0 --single-branch /opt/odoo/odoo12`

---------

Termine as configurações com o Pycharm!

<br>

<h3>Estrutura do Diretório/h3>
  
 ```

├── Odoo
│   └── 12.0
          ├── chillpass
          ├── custom-addons
          └── sipag
 ```

