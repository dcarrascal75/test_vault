#aws #AWS_CERT 
#DNS

Route 53 es un **servicio de DNS** (Sistema de Nombres de Dominio) proporcionado por AWS que permite a los usuarios gestionar la resolución de nombres de dominio para sus aplicaciones y recursos en la nube de AWS. **Route 53 sirve para enrutar el tráfico de Internet a los recursos dentro de AWS**

- Registrar: Puede ser Amazon route 53, Godaddy...

tipos de records:

- **A (Address) Record:** Mapea un nombre de dominio a una dirección IPv4.
  Los registros A apuntan **directamente a direcciones IP** y NO son adecuados para direcciones dinámicas
    
- **AAAA (IPv6 Address) Record:** Mapea un nombre de dominio a una dirección IPv6.
    
- **CNAME (Canonical Name) Record:** Mapea un nombre de dominio a ==otro nombre de dominio==, permitiendo crear alias. 
  Importante: Cname tiene in registro A subyacente que apunta a las IPs. Si la IP es dinámica, Route 53 gestiona y actualiza el registro A subyacente con la IP dinámica de la instancia EC2.
    
- **MX (Mail Exchange) Record:** Especifica el servidor de correo para el dominio, utilizado para enrutar correos electrónicos.
    
- **TXT (Text) Record:** Almacena texto arbitrario asociado con un nombre de dominio, utilizado para verificación y autenticación, entre otros.
    
- **PTR (Pointer) Record:** Realiza la operación inversa de un registro A, mapeando una dirección IP a un nombre de dominio.
    
- **NS (Name Server) Record:** Especifica los servidores de nombres autoritativos para el dominio, indicando dónde encontrar información DNS sobre el dominio. Por ejemplo, pregunto por "ejemplo.com", y el DNS no lo sabe pero me pasa la IP del DNS server que gestiona ".com"...
    
- **SOA (Start of Authority) Record:** Proporciona información sobre la autoridad del dominio, incluyendo detalles sobre el servidor primario y otros parámetros de configuración.

El límite máximo de registros que puede tener una zona hospedada en Route 53 por defecto es 10.000.

Partes de la url:
![[Pasted image 20240601140706.png]]

garantiza 100% availability 

---
Hosted Zones:

![[Pasted image 20240601142722.png]]


La public Hosted zone permite a cualquiera en internet consultar los registros de DNS
La private Hosted Zoe solo permite consultas internas. 

**TTL**: time to live: el tiempo que se queda cacheado en cliente un registro de DNS lo defines en route 53, no depende del cliente.
El TTL es obligatorio para todos los registros excepto alias
El TTL mínimo permitido en Route 53 es **1 segundo**

Los cambios por tanto tienen efecto cuando el TTL se agota


**CNAME vs ALIAS:**

Cname: redireciona de una url a otra. No vale con root domain (tambien llamados naked o apex)

si piede tener un alias
```
www.ejemplo.com   CNAME     otrodominio.com
```


En el ejemplo de `www.ejemplo.com`, el dominio raíz es `ejemplo.com`

AWS Route 53 proporciona una solución llamada "**Alias**" que permite mapear un dominio raíz a otro nombre de dominio sin violar las reglas del protocolo DNS. Los registros Alias pueden apuntar *a **recursos** de AWS como Elastic Load Balancers, buckets de S3 configurados como sitios web estáticos, CloudFront distributions, y otros recursos.* Es de tipo "A" pero apunta a un resource AWS.
```
ejemplo.com A (Alias)  my-load-balancer-1234567890.us-west-2.elb.amazonaws.com
```

Cuando usas un alias en lugar de un CNAME, Route 53 puede gestionar y actualizar automáticamente el registro A subyacente con la IP dinámica de una instancia EC2

alias funciona para root y non-root domains

### Comparación entre Alias y CNAME

| Característica                | Alias                                                                                                                                                                                                                         | CNAME                                                                                                                                                                                                                              |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Propósito**                 | Apuntar a recursos de AWS y otros nombres de dominio.                                                                                                                                                                         | Apuntar a otro nombre de dominio.                                                                                                                                                                                                  |
| **Dominio raíz (apex)**       | Sí, se puede usar en el dominio raíz.                                                                                                                                                                                         | No, no se puede usar en el dominio raíz.                                                                                                                                                                                           |
| **Soporte para recursos AWS** | Sí, específicamente diseñado para recursos de AWS: <br><br>Elastic Load Balancers, CloudFront, <br>Api Gateway,<br>S3 websites,<br>Global Accel.<br>VPC interface endpoints...<br>Otro R53 record.<br><br>(no admite EC2 DNS) | *No, no se puede usar directamente con recursos AWS.*                                                                                                                                                                              |
| **Velocidad de resolución**   | Resuelve a una IP directamente, potencialmente más rápido.                                                                                                                                                                    | Añade una capa adicional de resolución (puede ser más lento).                                                                                                                                                                      |
| **Costos**                    | **Gratis** cuando se utiliza con Route 53.                                                                                                                                                                                    | No tiene costo adicional.                                                                                                                                                                                                          |
| **TTL**                       | no se puede definir (lo define AWS)                                                                                                                                                                                           | Obligatorio                                                                                                                                                                                                                        |
| **Otros registros**           | Puede coexistir con otros registros (A, AAAA, etc.).                                                                                                                                                                          | No puede coexistir con otros registros en el mismo nombre de dominio.<br><br>Un registro CNAME redirige completamente el nombre de dominio a otro dominio y no puede coexistir con otros registros en ese mismo nombre de dominio. |


![[Pasted image 20240601184019.png]]


###### Routing Policies:

![[Pasted image 20240601185003.png]]

definen como R53 responde a las queries DNS

- **Simple**: route traffic to single / multiple resource.
	  Si devuelve varios, el cliente **elige uno al azar.**
	  Si es de tipo ALias, solo se puede especificar UN recurso AWS
	  No hace health checks --> en esto se diferencia del multi-value. 
	  Es decir, "Simple" puede devolver endpoints no sanos.
	  ==**Recuerda**==: no distribuye tráfico; en su lugar, simplemente devuelve uno de los valores posibles en el conjunto de registros ==de manera aleatoria==, sin tener en cuenta el peso. 
	  Si te preguntan por una distribución homogénea, entonces es **Weighted Routing.**
	  
	  
- **Weighted**: Se establece un porcentaje para cada destino (varios destinos)
		Admite health checks
- **Latency**: Redirecciona al que tenga menos latencia en responder (el mas cercano)
- **Failover**: Lo que hace es, estblecemos un health check y un recurso es master y otro failover. si falla master, va al otro, simplemente.
  ![[Pasted image 20240601192231.png]]
- Geolocation**: se basa en la location, ojo, es diferente a Latency!!
	    p.ej version inglesa o version alemana de la app. Existe un loaction "default" para lo no definido. Va por continentes Y paises.

- **GeoProximity**: se debe usar junto con ==Route 53 Traffic Flow.==
        Se pueden redireccionar recursos AWS (dando la región) o recursos NO-AWS  por Longitud y Latitud. Se les puede dar un peso (biass).
        
- **IP-based Routing**: Se da una serie de CIDRS y a que endpoints tienen que ir.
- **Multi-Value:** Se retornan varios valores. Si hay yn health check vinculado, solo se retornan los buenos. Entonces es el cliente el que elige (por eso no se puede considerar un "sustituto" de Load Balancer). 
  
  
- **Health Checks / calculated Health Check**
	  Los health checks sobre recursos internos se hacen usando CloudWatch ya que R53 no puede acceder a endpoints internas !!
	  Se les puede pedir que creen uns alarma o SNS mensaje si falla+
	  
	  

---
Se puede perfectamente coger un registro en 3rd party registrar como Godaddy y usar route 53 con ello. Te creas una **Public Hosted Zone** y le actualizas los nameservers de AWS al proveedor, de forma que use los de AWS