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
ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/example.conf
~~~

## Installer PHP-FPM

Installer php-fpm et quelques extensions utiles:

~~~bash
sudo apt install php8.2-fpm php8.2-cli php8.2-curl php8.2-gd php8.2-mcrypt php8.2-opcache php8.2-intl
~~~

Configuration de PHP-FPM et du pool de process:

Editer le fichier de config globale de FPM :  /etc/php/8.2/fpm/php-fpm.conf

Editer le fichier du pool :  /etc/php/8.2/fpm/pool.d/www.conf

~~~ini
user=deploy
group=deploy
listen = 127.0.0.1:9000
~~~

Démarrer le service php-fpm :

systemctl start php8.2-fpm
systemctl enable php8.2-fpm
systemctl status php8.2-fpm

Tester, avec une app PHP minimale (script)

vim apps/example.com/current/public/index.php

~~~php
<?php
echo "hello, world"
~~~


Donner acces à ngninx aux fichiers de log de l'application php

Ajouter www-data (l'utilisateur ne nginx) au groupe deploy

~~~bash
usermod -a -G deploy www-data
chmod g+x /home/deploy/app/logs
~~~

Donner acces a PHP a l'application (scripts)

~~~bash
sudo chmod -R +rx /home/deploy/
~~~

Tester la configuration nginx : sudo nginx -t

Tester l'app

curl localhost

## Mettre en place un banissement d'IP dans notre application web

Prérequis: services fail2ban et (parefeu) nftables actifs

1. Dans notre application, ecrire un log quand action jugée malveillante. Le log doit contenir l'IP de l'agent malveillant;
2. Indiquer à fail2ban où trouver le log;
3. Indiquer à fail2ban un pattern pour identifier une ligne de log malveillante.


Imagine une ressource de login qu'on veut proteger

~~~php
<?php
session_start();

$USER = 'admin';
$PASS = 'secret';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $user = $_POST['username'] ?? '';
    $pass = $_POST['password'] ?? '';

    if ($user === $USER && $pass === $PASS) {
        $_SESSION['auth'] = true;
        echo "Login success";
    } else {
        error_log("Login failed for user '$user' from {$_SERVER['REMOTE_ADDR']}");
        http_response_code(403);
        echo "Login failed";
    }
    exit;
}
?>

<form method="post">
    <input name="username" placeholder="Username">
    <input name="password" placeholder="Password" type="password">
    <button type="submit">Login</button>
</form>
~~~


