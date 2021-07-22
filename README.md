# wb9-10-HAPROXY

Creamos un servidor para la bdd:

```
apt-get install -y mariadb-server
mysql_secure_installation (usuario root/root)
mysql --execute "
	create database wordpress;
	grant all privileges on wordpress.* to 'wordpress_user'@'localhost' identified by 'vahKei4ovah0ieLasohaceiTahxeelae';
	flush privileges;"
```
En la configuración de HAproxy:

```
frontend http
  bind *:80
  mode http

  default_backend wordpress_servers

backend wordpress_servers
  balance roundrobin
  cookie SERVERUSED insert indirect nocache
  option httpchk HEAD /
  default-server check maxconn 20
  server server1 157.90.123.224:80 cookie server1
  server server2 116.203.69.0:80 cookie server2
```
Con el cookie podemos ver cual es el server

He finalizado la instalación de wp con el user enrique

Para probar haproxy con autenticación en mysql, crear el usuario haproxy por ejemplo en la bdd:
```
	use mysql;
	INSERT INTO user (Host,User,ssl_cipher,x509_issuer,x509_subject,authentication_string) values ('168.119.234.98','haproxy','','','','');
	flush privileges;
```
Y añadir la opción de check en la config de haproxy:

`option mysql-check user haproxy post-41`


Si alguna db se bloquea usar esto en al instancia correspondiente:
	
`mysqladmin flush-hosts`
	
