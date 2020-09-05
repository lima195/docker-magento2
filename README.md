# Adminer
pedro
http://localhost:8222/
- User: root
- Pass: root
- Db: magento
- host: db

# DB info

- User: root
- Pass: root
- Db: magento
- host: 172.22.0.108
- port: 3306

# Importação de banco

- Supondo que o arquivo chame database.sql e esteja fora da pasta do docker


    bin/mysql-import ../database.sql

# Criando host

- Vai ser criado o host para magentodev.com, caso queira mudar, edite o arquivos:
- - bin/magento-create-dns-host
- - nginx_conf_default.conf


    bin/magento-create-dns-host

# Após instalar algum módulo

    bin/magento-post-install-module

# Comandos Magento:

- Limpar cache


    bin/magento cache:clean

- Compilar di


    bin/magento setup:di:compile

- Deploy content


    bin/magento setup:static-content:deploy -f
    
- Setup upgrade


    bin/magento setup:upgrade
    
- Set mode


    bin/magento deploy:mode:set developer
    
# Instalação:
- 1 - docker-compose up -d --build
- 2 - bin/magento-create-dns-host
- 3 - (se tiver um banco)
-   -   bin/mysql-import ../database.sql
-   -   criar o env.php
-   -   alterar as urls na tabela `core_config_data` pelo adminer (http://localhost:8222/?server=db&username=root&db=magento&select=core_config_data)
- 4 - Se não tiver banco pra subir, só acessar magentodev.com e fazer o setup do magento


# Util

- Caso dê algum erro de network ao dar o docker-compose up -d


    docker network prune
    
- Se for usar redis e quiser limpar o cache:


    bin/redis-flush
