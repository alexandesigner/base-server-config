# Base Server Config (Meteor)

Instruções básicas para configurar um servidor usando Ubuntu 14.04 LTS

## Ubuntu 14.04 LTS

``$ sudo apt-get update``

## Meteor

``$ curl https://install.meteor.com/ | sh``

## NGINX

```
$ sudo apt-get install nginx
$ ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
$ sudo service nginx start
```

## Configurando um projeto básico

```
$ cd /etc/nginx/sites-available
$ nano <nome do projeto>
```

**Colar dentro do arquivo de configuração criado**

```
upstream dominio.com {
    server www.dominio.com:<port>;
}
server {
    listen 80;
    server_name dominio.com;
    
    access_log /var/log/nginx/dominio.dev.access.log;
    error_log /var/log/nginx/dominio.dev.error.log;

    location / {
        proxy_pass http://dominio.com;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}

```

**Fazendo uma cópia do sites-available para sites-enabled**

```
$ cd ~
$ cd /etc/nginx/sites-enabled
$ sudo ln -s /etc/nginx/sites-available/<nome do projeto> /etc/nginx/sites-enabled/
$ sudo service nginx restart
```

## Node JS 

**Versão acima ou igual de 0.10.40**

```
$ sudo apt-get install npm
$ sudo npm cache clean
$ sudo npm install -g n
$ sudo n stable
```

Usando ``$ sudo n stable list`` Você pode escolher a versão

## Mongo

```
$ echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
$ sudo apt-get update
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys
$ sudo apt-get install mongodb-org
```
Se de alguma forma não estiver localizando as configurações locais do mongo, rodar:

``$ export LC_ALL=C``

**Helpers**

```
$ ps wuax | grep <name>
$ htop
```

**Kill Meteor servers**
```
$ kill -9 `ps ax | grep node | grep meteor | awk '{print $1}'`
```
