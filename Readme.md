# Nextcloud testbed

### Usefull links

https://github.com/Wonderfall/dockerfiles/tree/master/nextcloud
https://www.c-rieger.de/nextcloud-installation-guide-de/

Simple Init: https://github.com/Yelp/dumb-init  
Nginx in docker: https://github.com/ngineered/nginx-php-fpm  

### Ansible playbook

Requirements:
- aptitude
- ansible

apt install aptitude ansible

ansible-playbook -i hosts install.yml
