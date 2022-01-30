### Hello world

### Autoryzacja

* klucz prywatny (id_student)
* ssh agent -> eval `ssh-agent`
* ssh-add ~/id_student
* fraza -> student1

### test connection
1. Maszyna
2. ``ssh ec2-user@{ip_maszyny}``

### Checkpoint -> install wordpress
install wp krok po kroku

### Requirements
- Apache
- MariaDB > 10.1
- php > 7.4

### Packeges
- wget
- mc
- git
- tree

0. perform actions as SU
1. install apache HTTP server
    sudo yum install httpd
2. start apache
    systemctl start httpd
3. config apache
```
<VirtualHost *:80>
    DocumentRoot "/var/www/blog"
    DirectoryIndex index.php index.html
 </VirtualHost>
```
4. pobierz WP
    https://pl.wordpress.org/latest-pl_PL.tar.gz
    download:
    wget https://pl.wordpress.org/latest-pl_PL.tar.gz
    extract:
    tar -xf latest-pl_PL.tar.gz
5. przeniesienie do odpowiedniego folderu
    mv wordpress/* /var/www/blog/
6.PHP
    EPEL repository
    ```yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm```
    REMI repository
    ```yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm```
    install packeges
    ```yum install php80 php80-php```
    search mysql 8.1+
    ```yum search php | grep mysql | grep 80```
    install mysql
    ```yum remove php80-php-pecl-mysql.x86```
    ```yum remove php80-php-mysqlnd.x86_64```

7. MariaDB
        create file:
        /etc/yum.repos.d/Mariadb.repo
        ````
        [mariadb]
        name = MariaDB
        baseurl = https://mariadb.mirror.serveriai.lt/yum/10.6/centos7-amd64
        gpgkey=https://mariadb.mirror.serveriai.lt/yum/RPM-GPG-KEY-MariaDB
        gpgcheck=1
        ````
        install mariadb server
        ```sudo yum install MariaDB-server MariaDB-client```
8. Create db && db user && assigne permissions
        ```
        create database databasename;

        create user 'blog' identified by 'blog1';
        grant all privileges on databasename.* to blog@localhost identified by blog;
        grant all privileges on databasename.* to 'user'@host;
        grant all privileges on databasename.* to 'user'@%;

        FLUSH PRIVILEGES;
        ```
        connect with database:
        ```mysql -u blog -p blog```
9. Put db config in wp -config
        ```cp wp-config-sample.php wp-config.php```

### Automate installation
# adsk-wp-install
