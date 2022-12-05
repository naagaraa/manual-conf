# ubuntu server

## nginx

1. installing the nginx web server

   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. check firewall and enable

   ```bash
   sudo ufw status
   sudo ufw allow 'Nginx HTTP'
   ```

3. if do not have domain

   ```bash
   ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
   ```

## Mysql

1. Mysql

   ```bash
   sudo apt install mysql-server
   sudo mysql_secure_installation
   ```

   next step

   ```bash
   VALIDATE PASSWORD PLUGIN can be used to test passwords
   and improve security. It checks the strength of password
   and allows the users to set only those passwords which are
   secure enough. Would you like to setup VALIDATE PASSWORD plugin?

   Press y|Y for Yes, any other key for No:
   ```

   next step

   ```
   There are three levels of password validation policy:

   LOW    Length >= 8
   MEDIUM Length >= 8, numeric, mixed case, and special characters
   STRONG Length >= 8, numeric, mixed case, special characters and dictionary file

   Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
   ```

   next step


   ```
   Please set the password for root here.

   New password:

   Re-enter new password:
   ```

2. setting up mysql

```bash
sudo mysql
```

```mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

```mysql
+------------------+-------------------------------------------+-----------------------+-----------+
| user             | authentication_string                     | plugin                | host      |
+------------------+-------------------------------------------+-----------------------+-----------+
| root             |                                           | auth_socket           | localhost |
| mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
+------------------+-------------------------------------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

```mysql
exit
```

3. test login

```bash
mysql -u root -p
```

## Installing PHP Nginx

1. add repository

```
apt -y install software-properties-common
add-apt-repository ppa:ondrej/php
```

2. install php

```
sudo apt -y install php7.4
```

3. install modul

```
sudo apt install php7.4-fpm php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip unzip php7.4-json -y
```

4. mode auto start
sudo systemctl start php7.4-fpm
sudo systemctl enable php7.4-fpm


## Ngix to use PHP Preprocessor

```
sudo mkdir /var/www/html/your_domain
```

```
sudo chown -R $USER:$USER /var/www/html/your_domain
```

```
sudo nano /etc/nginx/sites-available/your_domain
```


example conf
```
server {
    listen 80;
    server_name your_domain www.your_domain;
    root /var/www/your_domain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

copy active to sites enable

```
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

unlink

```
sudo unlink /etc/nginx/sites-enabled/default
```

reload nginx -t

```
sudo nginx -t
```