# Reverse-proxy avec nging et application php-fpm

Deployer une application (php) FastCGI (php-fpm) sur un serveur, avec nginx et fail2ban pour l'authentification.

Acces a une machine (VM), sous Debian.

## Creer un utilisateur de la machine dédiée

sudo adduser deploy
sudo usermod -a -G sudo deploy

## Installer nginx

apt install nginx
nginx -v
sudo nginx

Port 80 occupé !

Inspecter avec netstat

apt install net-tools
nestat -tupln | grep :80

Arreter le service si présent (par ex apache2) sur le port 80

Lancer nginx comme un service avec systemd

systemctl start nginx
systemctl status nginx

Pour tester, installer cURL

apt install curl
curl localhost(:80)

Devrait voir le site web par défaut de nginx

Article sur le load balancing illustré : https://samwho.dev/load-balancing/

## Créer le systeme de fichiers pour l'application

mkdir -p /home/deploy/apps/example.com/current/public
