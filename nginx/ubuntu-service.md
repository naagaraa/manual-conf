# ubuntu service

1. restrat nginx and php fpm

```bash
systemctl restart nginx
systemctl restart php7.0-fpm
systemctl restart php7.2-fpm
```


example ngix site 1 used php7.2

```bash
server {
   listen 80;

   root /var/www/html/site2.example.com/;
   index index.php;

   server_name site2.example.com;

   location / {
      try_files $uri $uri/ =404;
   }

   location ~ \.php$ {
      try_files $uri =404;
      fastcgi_split_path_info ^(.+\.php)(/.+)$;
      fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
      fastcgi_index index.php;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      include fastcgi_params;
   }
}
```

example ngix site 1 used php7.0

```bash
server {
   listen 80;

   root /var/www/html/site2.example.com/;
   index index.php;

   server_name site1.example.com;

   location / {
      try_files $uri $uri/ =404;
   }

   location ~ \.php$ {
      try_files $uri =404;
      fastcgi_split_path_info ^(.+\.php)(/.+)$;
      fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      fastcgi_index index.php;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      include fastcgi_params;
   }
}
```