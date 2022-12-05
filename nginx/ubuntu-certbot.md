# Let's Engcrpt at ubuntu

1. install 

```bash
sudo apt install certbot python3-certbot-nginx
```

2. allow hhtps at firewall
```bash
sudo ufw status
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

3. obtaining ssl

```
sudo certbot --nginx -d example.com -d www.example.com
```

4. active auto renewal

```
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```