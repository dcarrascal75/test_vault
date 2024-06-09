#aws #AWS_CERT 


CIDR - Classless Inter-domain Routing 

<base_ip> / <subnet_mask>   (cuantos bits pueden cambiar)

![[Pasted image 20240602185440.png]]

por ejemplo: /28 means 16 IPs (=2^(32-28) = 2^4) --> 16, de 0 a 15

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
- ==minimo:  **/28**     (16 IPs)== 
- ==máximo  **/16**     (65536 IPs)== ---> Por ejemplo una cidr /15 o menos ya no seria aceptable

No tiene que haber Overlap con otras VPC.

SE pueden AÑADIR y borrar cidrs a la VPC una vez creada. Se pueden tener hasta 5 cidrs por VPC.


![[Pasted image 20240602192738.png]]

##### Subnets #aws_subnets

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

Ahora todavia falta preparar la **Route Table**

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

###### Nat Gateway #AWS_NATGW

Es lo que permite a las Ec2 privadas conectarse a internet.

la administra AWS,  Se paga por uso y por bandwidth
Se crea **en lo publico** para usarlo desde otra subnet (privada)
usa Elastic IP
Necesita un IGW. No necesita SG.

private subnet --> private Route Table--> NAT GW in Public Subnet--> IGW

Tiene high availability pero solo en su AZ, asi que si uso varias AZ necesitare varias Nat Gateways.

--------

###### NACL (Network ACLS) 

Permite ciertas inbound rules. Es stateless

Los security groups son stateful.
El límite máximo predeterminado es **5 Security Groups por instancia de EC2**

So remember NACLs are stateless
and security groups are stateful --> Todo lo que entra puede salir

entonces, las salidas se evaluan en el nacl aunque hayan pasado el SG

Solo puede haber un NACL por Subnet
Se evaluan en precedencia. La primera regla que se cumpla marca la decision.
Si no se cumple nada, se deniega. ( * ) --> ultima regla siempre

Exite asociada una "default NACL" que acepta todo por defecto

![[Pasted image 20240607232228.png]]

---

##### Ephemeral ports

Los clientes se conectan a un puerto definido y esperan una respuesta en un puerto efímero. El rango depende del SO.

 **Paso 1)**
 El cliente se conecta al servidor (IP y puerto conocidos, p.ej 443). Para ello, elige un puerto libre de los que tiene disponibles y lo envia en la request como puerto de destino (p.ej el 50105):
![[Pasted image 20240607233611.png]]
 Paso 2)
 El servidor responde apuntando a ese mismo puerto 
 ![[Pasted image 20240607233739.png]]

A la hora de definir los NACLS en las subnets, hay wque tener en cuenta los ephemeral ports:

![[Pasted image 20240607234023.png]]

Comparativa #AWS_NACL vs #AWS_SG

![[Pasted image 20240607234215.png]]

> **Importante**: Un SG sabemos que es **stateful**. Entonces que ocurre si borro las outbound rules?
> 
> Eso implicaria que si recibo una petición externa a mi EC2 y la quiero responder, funcionaria por ser stateful. PEro si la peticion se inicia desde dentro de la Ec2, no funcionaria si no hay outbound rules. 
> Del mismo modo, si el trafico puede salir del SG, siempre puede vilver a entrar, por ser stateful
> 


Por eso importan los outbounds en un SG

> ==Recuerda que los NACLs operan a nivel de Subred y los SG a nivel de EC2==

##### Uso de Security Groups (SG)
- **A nivel de instancia**: Controlan el tráfico entrante y saliente de las instancias de EC2.
- **Stateful**: Permiten automáticamente el tráfico de retorno.
- **Configuración específica**: Ideal para definir reglas de seguridad precisas para cada instancia.
##### Uso de NACLs
- **A nivel de subred**: Controlan el tráfico entrante y saliente de toda una subred en una VPC.
- **Stateless**: Necesitan reglas para el tráfico de ida y vuelta.
- **Bloqueo de tráfico específico**: Útil para bloquear tipos de tráfico específicos entre subredes.

