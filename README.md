# Base server config (VPS)
Instruções básicas para configurar um servidor usando Ubuntu 14

## UNBUNTU 14

``sudo apt-get update``

## NGINX

``sudo apt-get install nginx``

``ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'``

``sudo service nginx start``

## CONFIGURANDO UM PROJETO NO NGINX

``cd /etc/nginx/sites-available``

``nano <nome do projeto, o mesmo que ta no MUP OU DOCKER>``

**Colar o texto abaixo alterando os dominios para o nome que ira ficar**

```
upstream domain.com {
    server domain.com:3001;
}
server {
    listen *:80;
    server_name domain.com;
    access_log /var/log/nginx/domain.dev.access.log;
    error_log /var/log/nginx/domain.dev.error.log;
    location / {
        proxy_pass http://domain.com;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```


``cd ~``

``cd /etc/nginx/sites-enabled``

``ln -s /etc/nginx/sites-avaliable/<nome do projeto, o mesmo que ta no MUP> /etc/nginx/sites-enabled/``

``sudo service nginx restart``

## NODE 
**Versão acima ou igual de 0.10.40**

``sudo apt-get install npm``

``sudo npm cache clean``

``sudo npm install -g n``

``sudo n stable`` 

_Com 'list' voce pode escolher a versão_

## MONGO

``echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list`` 

``sudo apt-get update``

``sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys```

``sudo apt-get install mongodb-org``
