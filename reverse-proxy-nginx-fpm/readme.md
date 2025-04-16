# Reverse-proxy avec nging et application php-fpm

Deployer une application (php) FastCGI (php-fpm) sur un serveur, avec nginx et fail2ban pour l'authentification.

Acces a une machine (VM), sous Debian.

- [Reverse-proxy avec nging et application php-fpm](#reverse-proxy-avec-nging-et-application-php-fpm)
  - [Creer un utilisateur de la machine dédiée](#creer-un-utilisateur-de-la-machine-dédiée)
  - [Installer nginx](#installer-nginx)
  - [Créer le systeme de fichiers pour l'application](#créer-le-systeme-de-fichiers-pour-lapplication)
  - [Configuration de Nginx](#configuration-de-nginx)


## Creer un utilisateur de la machine dédiée

~~~bash
sudo adduser deploy
sudo usermod -a -G sudo deploy
~~~

## Installer nginx

~~~bash
apt install nginx
#test
nginx -v
sudo nginx
~~~

Port 80 occupé ?

Inspecter avec netstat

~~~bash
apt install net-tools
nestat -tupln | grep :80
~~~

Arreter le service si présent (par ex apache2) sur le port 80.

Lancer nginx comme un service avec systemd

~~~bash
systemctl start nginx
systemctl status nginx
~~~

Pour tester, installer cURL

~~~bash
apt install curl
curl localhost
~~~

> Vous devez voir le site web par défaut de nginx.

> [Article sur le load balancing illustré](https://samwho.dev/load-balancing/)

## Créer le systeme de fichiers pour l'application

~~~bash
mkdir -p /home/deploy/apps/example.com/current/public
mkdir -p /home/deploy/apps/log
~~~

## Configuration de Nginx

Configuration du Virtual Host de notre application. On suppose que l'application sera associée au nom de domaine example.com. Éditer le fichier `/etc/nginx/sites-available/example.conf` :

Un VH = 1 fichier de config avec l'entrée `server{}` :

> Vous pouvez tout placer dans un seul fichier de config (plusieurs server{}, mais vous perdez en modularité !)

~~~ini
server {
    listen 80;
    server_name example.com;
    index index.php;

    # Logs
    error_log /home/deploy/apps/logs/example.error.log;
    access_log /home/deploy/apps/logs/example.access.log;
    root /home/deploy/apps/example.com/current/public;

    # Gestion des requêtes non-php
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    # Gère les requêtes PHP
    location ~ \.php {
        try_files $uri =404;
	fastcgi_split_path_info ^(.+\.php)(/.+)$;
        include fastcgi_params;
	fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	fastcgi_param SCRIPT_NAME $fastcgi_script_name;
        fastcgi_index index.php;
	#Important : passage de la requete au pool de process PHP (ecoute sur :9000)
        fastcgi_pass 127.0.0.1:9000;
    }
}
~~~

Créer un lien symbolique de la config du virtual host dans sites-enabled (publier le site web, activer le virtual host) :

~~~bash
ln -s sites-available/example.conf sites-enabled/example.conf
~~~
