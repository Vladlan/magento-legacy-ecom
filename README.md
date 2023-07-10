#How to start magento

## Prerequisites (tested on MacOS and Windows 11 (WSL))
1. Docker
2. curl
3. unzip


## Steps to setup initial state of the magento:

1. Install magento: https://courses.m.academy/courses/set-up-magento-2-development-environment-docker/lectures/8974570


2. To setup proper state of the magento app:  
    
    a. Download backup from https://drive.google.com/file/d/17H_i-BLjry-ZSSthbG65K-6DoqaWS9wU/view?usp=sharing  

    b. Prepare backup:  

    ```bash
    sudo apt-get install unzip
    unzip backup.zip
    current_folder=$(basename "$PWD")
    container_name="${current_folder}-app-1"
    docker cp ./backup $container_name:var/www/html/var/backups
    ```   

    c. Run command to rollback backup  

    ```bash
    bin/magento setup:rollback -c 1687872010_filesystem_code.tgz -m 1687872010_filesystem_media.tgz -d  1687872010_db.sql
    bin/magento setup:upgrade

    ```

    c. Check `magento.test` and `magento.test/admin (with mynewuser1, mynewpassword1)` 


## Main source of info
https://github.com/markshust/docker-magento


## Useful commands
1. To create user for https://magento.test/admin (you will also have to complete step below):

```bash
bin/magento admin:user:create \
    --admin-user="mynewuser1" \
    --admin-password="mynewpassword1" \
    --admin-email="john@doe.com" \
    --admin-firstname="MyName" \
    --admin-lastname="MyLastName"
```

2. To disable TFA:
```bash
bin/magento module:disable {Magento_AdminAdobeImsTwoFactorAuth,Magento_TwoFactorAuth}
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento indexer:reindex
bin/magento cache:flush
```


3. Links about how to move magento 2 to another PC
https://stackoverflow.com/questions/16538560/how-to-bring-live-magento-site-to-localhost-without-effecting-live-site


4. To enable db backup 
```bash
bin/magento config:set system/backup/functionality_enabled 1
```

5. To backup magento 2 
```bash
bin/magento setup:backup --code --media --db
```