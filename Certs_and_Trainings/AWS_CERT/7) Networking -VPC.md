#aws #AWS_CERT 


CIDR - Classless Inter-domain Routing 

<base_ip> / <subnet_mask>   (cuantos bits pueden cambiar)

![[Pasted image 20240602185440.png]]

es contar cuanto nos falta hasta 32, y eso es el exponente del 2 

![[Pasted image 20240602185621.png]]

La autoridad que gestiona las IPs es IANA (internet Assigned Number Authority) 

Las IPs privadas solo admiten ciertos valores: 
	10.0.0.0/8  --> Big networks
	172.16.0.0/12  --> AWS default para AWS
	192.168.0.0/16:  -->  Home networks

Todo el resto de IPS de internet son PUBLICAS.


LAs subnets son para poner las cosas en diferentes AZ

**El maximo de AWS VPCs por ==region== es 5**

Tamaño 
- minimo:  **/28**     (16 IPs) 
- máximo  **/16**     (65536 IPs)

No tiene que haber Overlap con otras VPC.

SE pueden AÑADIR y borrar cidrs a la VPC una vez creada. Se pueden tener hasta 5 cidrs por VPC.


![[Pasted image 20240602192738.png]]

##### Subnets

- se reservan 5 IPs en cada subnet por defecto. Se vinculan a una AZ (pueden estar todas en la misma eh, eso se elije). Es posible tener todas las subredes de una VPC en la misma zona de disponibilidad. Sin embargo, esto va en contra de las mejores prácticas de diseño de arquitectura en la nube.
- Lo habitual es crear 2 public y 2 privates en diferentes AZ, de modo que queden 1 privada y una publica en cada AZ. Asi tienes mas resiliencia a fallos.

**Internet Gateway** es lo que permite conectara internet desde una subred. 
Una VPC solo puede tener una IGW y viceversa 

1 vpc <--> 1 igw

Funcionana en conjunto con las Route Tables.

![[Pasted image 20240602194232.png]]
(en este dibujo )

Para que una Subnet se publica, hay que poder añadir public ips. Se puede activar esto:
![[Pasted image 20240602194514.png]]

pero lo importante es elegir el enable al lanzar la instancia Ec2
![[Pasted image 20240602194711.png]]

ESo te asigna la IP, pero aun no tienes conectividad solo con eso. 
Hay que crear el IGW y atacharlo a una ys olo una VPC.

Ahora todavia falta preparar la Route Table

Las Route Table se pueden asociar varias a la misma VPC.
En este caso lo habitual es crear una para las Public subnets y otra para privates subnet.

Vinculo las subnets publicas al Route Publico y lo mismo con las privdas.

Las subnets solo pueden estar asociadas a UNA route Table. 
sin embargo, una Route table puede tener N subnets.

 1 Route Table <---->      subnet1
				    subnet2
				    subnet 3 ...
				    

Entonces añado a la route el enrutamiento adecuado, apuntando al GW:

![[Pasted image 20240602195919.png]]

ojo: Las reglas se ejecutan en orden. Si pusieras primero la 0.0.0.0/0, todo se iria por el IGW


Vale, y si quiero acceder a los Ec2 de la privada? --> **Bastion Hosts**

**Los Bastion** estan en la subnet publica y pueden acceder al EC2 privado.
Al bastion se accede desde el puerto 22, su SG tiene habilitada nuestro cidr local

El SG del EC2 admite como entrada el SG del bastion.


![[Pasted image 20240602201248.png]]

---

###### NAT Instance: Network Address Translation (deprected, but in exam)

Es lo que permite a las Ec2 privadas conectarse a internet.

Se crean en lo público. es un ec2 especifico

Ojo, lo suyo es usar NAT Gateway

###### Nat Gateway

la administra AWS,  Se paga por uso y por bandwidth
Se crea en lo publico para usarlo desde otra subnet (privada)

Necesita un IGW. No necesita SG.

Tiene high availability pero solo en su AZ, asi que si uso varias AZ necesitare varias Nat Gateways.

