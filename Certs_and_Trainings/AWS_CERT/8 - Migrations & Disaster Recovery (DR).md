
**RPO** : **Recovery Point Objective:**
La frecuencia de backups que se hace.

O dicho de otra forma, ==que nivel de 'data loss' estamos dispuestos a aceptar.==
E.g Si hago backups cada 6 horas, estoy dispuesto a perder 6 horas de datos.


**RTO**: **Recovery Time Objective:** 

El tiempo que tardamos desde que ocurre un desastre en recuperarnos.
(Downtime)

![[Pasted image 20240609131711.png]]
##### Estrategias de DR
- Backup / restore                 (más lento, más barato)
- Pilot Light
- Warm standBy
- Hot site / multisite approach.   (más rápido, más caro)

![[Pasted image 20240609131950.png]]

**1- Backup / Restore:** 

**Not expensive, High RPO, High RTO**

Si por ejemplo hacemos copias mediante un snowball o medianta un Storage Gateway cada semana, tendremos un RPO de 1 semana
O mediante snapshots, sin salir de AWS
![[Pasted image 20240609132410.png]]

**2- Pilot Light**

Consiste en mantener una versión mínima y esencial de tu entorno en la nube. Esta versión mínima incluye los componentes críticos (generalmente la BBDD) de tu sistema que están siempre en funcionamiento ("encendidos"), listos para escalarse rápidamente en caso de un desastre.

**3- Warm StandBy:**

Es como pilot light pero tenemos todo desplegado, y se escala en caso de desastre.
![[Pasted image 20240609133318.png]]
Escenario inicial

Y si ocurre un desastre, el route 53 se apunta a AWS y se escalan los EC2:

![[Pasted image 20240609133427.png]]

**4- Multi Site / Hot Site Approach**

Es lo mismo que el Warm StandBy, pero tienes todo escalado ya.


read more about Disaster Recovery options in AWS here: 
https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html

------

![[Pasted image 20240609133911.png]]

------------


#### AWS DataSync

##### ¿Qué es AWS DataSync?

AWS DataSync es un servicio que facilita y automatiza la transferencia de datos entre sistemas de almacenamiento on-premises (en las instalaciones de la empresa) y los servicios de almacenamiento de AWS, como Amazon S3, Amazon EFS (Elastic File System) y Amazon FSx for Windows File Server.
##### Características Principales

1. **Transferencia Rápida y Segura**: AWS DataSync acelera la transferencia de datos mediante la optimización del uso de la red, la compresión de datos y la verificación automática de la integridad de los datos.
2. **Automatización de Tareas**: Permite programar y ejecutar tareas de transferencia de datos de manera automática y recurrente, lo que reduce la necesidad de intervención manual.
3. **Compatibilidad con Múltiples Orígenes y Destinos**: Soporta la transferencia de datos desde y hacia una variedad de sistemas de almacenamiento, tanto locales como en la nube.
4. **Seguridad Integrada**: Los datos se transfieren de manera segura mediante cifrado en tránsito y en reposo. Además, se integra con AWS Identity and Access Management (IAM) para el control de acceso.
5. **Escalabilidad**: Diseñado para escalar y manejar grandes volúmenes de datos, permitiendo la transferencia de petabytes de datos de manera eficiente.

-----------

#### Database Migration Service (DMS)

- migraciones que pueden ser heterogeneas. La source pertenece activa
- Soports Continuous Data Replication con CDC (Change Data Capture)
- Se crea una instancia EC2 y esa instancia hace al tarea

![[Pasted image 20240609134515.png]]

Los source pueden ser OnPremise o EC3, Oracle, SAP, Azure , RDS, DocumentDB o incluso S3
Los targets: parecido, incluso S3, Redshift, ==kafka==, Redis, Opensearch...

Para esto necesitamos **SCT** : ==Schema Conversion Too==l, Convierte el esquema de un formato a otro.

> **SCT solo hace falta si las engines SON diferentes**

![[Pasted image 20240609134923.png]]

Si la bbdd esta en MultiZone, se sincronizan todas automaticamente


**Migracion de RDS a Aurora: (MySQL)**

- Opcion 1 Con un Snapshot
- Opcion 2 Crear un a Read Replica de la RDS y promoverla

Si es desde una Mysql Externa a Aurora MySQL:
- Opcion 1: Usar Percona *XtraBackup* y restaurar el backup
- Opcion 2: usar un mysqldump para restaurar la bbdd 

Tambien se puede usar DMS si ambas bbdd estan arriba y funcionando

**Migracion de RDS a Aurora: (PostGre)**
- Opcion 1 Con un Snapshot
- Opcion 2 Crear un a Read Replica de la RDS y promoverla

Si es de una externa::
 - Crear un backuo, ponerlo en S3, importarlo usando la *aws_s3* Extension
 
 Tambien se puede usar DMS si ambas bbdd estan arriba y funcionando

DMS te permite tanto ir de AWS a Onprem como viceversa!

-------------

On.Premise Strategy: 

- Se puede descargar la Amazon Linux 2 AMI como una Virtual Machine (.iso) para VirtualBox, por ejemplo.
- Esto te permite ejecutar Amazon Linux 2 en tu on-premise

Tambien se puede migrar VMs existentes en EC2


**AWS Server Migration Service (SMS)** : migraciones incrementales de servidores onprem

------------

#### AWS Backup

automatiza y centraliza los backups

- Ec2
- S3
- RDS, aurora, dynamo
- documentdb, Neptune
- EFS
- AWS storage Gateway

Soporta backups entre regiones y entre accounts

Soport PITR (point in time recovery)

Se Almacenan los Backups en S3 (en un bucket interno especifico para backup)

![[Pasted image 20240609184925.png]]

Vault Lock: impide que se borren. mi siquiera el root user puede.


Se crea el plan, se dice cada cuanto se hara, etc... Y luego se añaden los recursos
(se pueden usar tags)


![[Pasted image 20240609185347.png]]

Si añado mas elementos con ese tag, se añaden automaticamente

---

#### AWS Application Discovery Service

AWS Application Discovery Service : Hace un "discovery" de lo que tienes en OnPrem para planificar un migracion a AWS

Envia sus datos al AWS Migration Hub


#### AWS Application Migration Service:

Reemplaza el SMS (server Migration Service)

Necesita un Replication agent en On PRem
![[Pasted image 20240609190127.png]]

----

#### Migracion de Grandes cantidades de datos a AWS

se puede usar: 
- Site-to-site VPN: Se pone rapido pero tardarias muchos dias en transferir datos
- Direc Connect (DX): tardas un mes en ponerlo y luego tardarias otro mes
- Snowball: About a week (several in parallel)

Para on-going replications, se puede usar site-to site o DX tambien.

---

WMware Cloud on AWS
