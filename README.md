# proyecto 2 Despliegue de aplicación Moodle escalable

<h3>Integrantes</h3>

- Juan Diego Restrepo
- Santiago Aguirre
- Ricardo Gottheil
  
* <h3>Arquitectura de referencia</h3>

A continuación encuentra la arquitectura de referencia para un mayor entendimiento del proyecto, en ella podrá ver el uso de una base de datos, un sistema distribuido de archivos y las instancias de EC2 con el Moodle dockerizado, es importante aclarar que usamos Auto-scaling de aws con balanceador para hacer la aplicación escalable de manera horizontal, tal cual como un acordeón.

![image](https://user-images.githubusercontent.com/43093044/168954788-16dd6df1-5177-441f-9e5d-bb5054d4f8f6.png)


* Base de datos Amazon RDS

Haciendo uso del servicio de Amazon RDS creamos una base de datos con motor MariaDB donde tendremos que tener en un segurity group el puerto 3306 abierto.

![image](https://user-images.githubusercontent.com/43093044/168956959-25f6ce58-f770-4dc2-9fa8-803cecda28fd.png)

Tenemos que tener en cuenta el punto de enlace y contraseña para acceder a ella y para configurar el docker-compose.yml del moodle

![image](https://user-images.githubusercontent.com/43093044/168957243-671171ea-df35-482a-9dc3-2304f13d99e2.png)


* Sistema distribuido de archivos EFS (Elastic File System) de Amazon

Una vez la base de datos ha sido creada procederemos a usar el servicio EFS, que utilizaremos para que nuestras isntancias de moodle almacenen la información y puedan compartir los datos entre ellas

![image](https://user-images.githubusercontent.com/43093044/168958178-be91c077-bca2-4ee4-ab9d-8048bb7a48fa.png)

Una prueba importante que podemos hacer es crear instancias de EC2 configurandolas con el EFS que acabamos de crear e intentar ver que comparten los archivos, por ejemplo

![image](https://user-images.githubusercontent.com/43093044/168958525-b0bac222-693c-4b1a-8ec2-548d8ea1d7f1.png)

* Instancia EC2 con el servicio Web Moodle en docker

Ahora procederemos a crear una instancia de EC2 que en el paso siguiente nos servira como AMI, usar como base este template:
https://github.com/bitnami/bitnami-docker-moodle/blob/master/docker-compose.yml


Deberá realizar la instalación de docker y docker-compose y configurar la maquina para que al hacer reboot no deje apagado el docker.

A continuación un ejemplo de como debe configurar el docker-compose.yml

![image](https://user-images.githubusercontent.com/43093044/168959484-2d575571-5736-4f77-b3de-916fa562222b.png)


* Crear AMI para el servicio de Auto scaling

![image](https://user-images.githubusercontent.com/43093044/168959937-89cf5534-83fd-47d6-bd61-2c13c1e948b4.png)

* Crear Target Group

![image](https://user-images.githubusercontent.com/43093044/168960019-fb3d0405-fff1-476f-9cd8-2764cda4f6e4.png)

* Crear y configurar Load Balancer

![image](https://user-images.githubusercontent.com/43093044/168960114-a3280ec1-ff6d-46d9-97e0-286840035f0d.png)

* Crear y configurar un Launch Template y Grupo de Auto Scaling

![image](https://user-images.githubusercontent.com/43093044/168960248-cf4e2206-cea3-4349-a3ac-84b4716b21d2.png)

![image](https://user-images.githubusercontent.com/43093044/168960359-cab0e0d5-fdfc-48de-88f4-d0112f3d03d5.png)






