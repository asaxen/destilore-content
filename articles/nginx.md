# Nginx basics
Simple how to for installing nginx and hosting a static website

## Set up

Install nginx package
```sh
pacman -S nginx # Archlinux
apt install nginx # Debian / Ubuntu
```
Enable and start service
```
systemctl enable nginx
systemctl start nginx
```

Edit nginx.conf
```sh
nano /etc/nginx/nginx.conf
```

Add the following to the end of http block in nginx.conf
```sh
include sites-enabled/*;
```

Create sites-available and sites-enabled folders
```sh
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

## Static hosting of a webpage (example)

1. Create and edit **/etc/nginx/sites-available/example.conf**
```sh
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/example;
        index index.html;

        server_name _d;

        location / {
           try_files $uri $uri/ =404;
        }
}
```

2. Link the default.conf file to sites-enabled folder
```sh
ln -s /etc/etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/example.conf
```

2. Create example folder
```
mkdir /var/www/example
```


3. Add your static webpage content to /var/www/example folder

4. Restart nginx service
```sh
systemctl restart nginx
```
5. Web page should now be accesible on server at port 80