
#tech_improvement 

---

**Aurora Cloning**: Hay una cosa que es "Aurora Cloning", que parece ser mas rapido que crear un snapshot y regenerarlo.

![[Pasted image 20240504081534.png]]

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Managing.Clone.html

En mi caso de uso actual, necesito los snapshots, habria que ver si hay alguna ventaja en hacer el clone.

*¿Se puede usar Aurora Database Cloning entre diferentes AWS accounts? Tengo una account en prod y una account en STG, me gustaria hacer copias regulares de mi bbdd de prod a la account de stg* 

**Sí, es posible utilizar la funcionalidad de clonación de bases de datos Aurora entre diferentes cuentas de AWS**. Esto permite realizar copias regulares de una base de datos de producción en una cuenta de entorno de ensayo o pruebas, como en el caso de su cuenta de STG.

##### Proceso de Clonación

1. **Conexión entre Cuentas**: Es necesario configurar permisos de IAM que permitan a la cuenta de STG acceder a los recursos de la cuenta de producción, específicamente a la base de datos Aurora que se desea clonar.

2. **Configuración de Políticas de IAM**: Se deben crear y configurar políticas de IAM tanto en la cuenta de producción como en la cuenta de STG para permitir el acceso y la clonación de la base de datos.

3. **Utilización de la Consola de AWS**: Una vez configuradas las políticas de IAM, se puede utilizar la consola de AWS para iniciar el proceso de clonación de la base de datos Aurora desde la cuenta de producción a la cuenta de STG.

##### Consideraciones

- **Seguridad**: Es importante asegurarse de que se sigan las mejores prácticas de seguridad al configurar los permisos de IAM para garantizar que solo los usuarios autorizados tengan acceso a los recursos.

- **Costos**: Se deben tener en cuenta los costos asociados con la transferencia de datos entre cuentas de AWS, especialmente si la base de datos de producción es grande y se realizan copias regulares.

##### Resumen

La clonación de bases de datos Aurora entre diferentes cuentas de AWS es posible mediante la configuración adecuada de permisos de IAM y el uso de la consola de AWS para iniciar el proceso de clonación. Esto permite realizar copias regulares de una base de datos de producción en una cuenta de entorno de ensayo o pruebas.


---

**Uso de RDS Proxy**
El problema es, 
- hay que probar la performance
- tambien ver que pasa con el acceso, ya que se hace almacenando un secret en secretmanager. La duda es, como se accede con diferentes roles en ese caso? Al acceder desde la app, que rol se toma, el de la app o el del secret manager? O el rol que se usa en el secretmanager solo se usa para conectar el proxy a la bbdd pero la conexion de la app usa sus credenciales definidas?
  
Preguntado en el curso de AWS


----

**Uso de Redis Cache distribuidas**

la opcion mas sencilla es usar "Global Datastore" y configurar varias regiones.

video: https://www.youtube.com/watch?v=IKcIz7EUP-0

De chatgpt:

Sí, es posible configurar el cluster distribuido de ElastiCache para que solo se pueda escribir en el nodo primario en Irlanda (IRE) y que las operaciones de lectura puedan realizarse tanto en el nodo primario (IRE) como en el nodo secundario en Frankfurt (FRA). Esta configuración aseguraría que todas las operaciones de escritura se realicen en el nodo primario y que las operaciones de lectura puedan distribuirse entre ambos nodos.

Para lograr esta configuración, puedes seguir estas recomendaciones:

1. **Configurar aplicaciones para escribir solo en el nodo primario (IRE)**: Asegúrate de que tus aplicaciones estén configuradas para enviar todas las operaciones de escritura al nodo primario en Irlanda (IRE). Esto se puede lograr utilizando el endpoint de escritura proporcionado por ElastiCache para el nodo primario en Irlanda y configurando tus aplicaciones para que utilicen este endpoint específico para las operaciones de escritura.

2. **Configurar aplicaciones para leer desde ambos nodos (IRE y FRA)**: Para las operaciones de lectura, puedes configurar tus aplicaciones para que lean desde ambos nodos en Irlanda y Frankfurt. Esto se puede lograr utilizando el endpoint de lectura proporcionado por ElastiCache para ambos nodos y distribuyendo las operaciones de lectura entre estos endpoints según sea necesario.

3. **Considerar el uso de grupos de seguridad**: Para reforzar esta configuración, puedes utilizar grupos de seguridad de AWS para controlar el acceso a los nodos del clúster. Puedes configurar los grupos de seguridad de modo que solo el nodo primario en Irlanda permita conexiones de escritura, mientras que ambos nodos (IRE y FRA) permiten conexiones de lectura.

4. **Pruebas y monitoreo**: Después de configurar esta configuración, es importante realizar pruebas exhaustivas para asegurarse de que tus aplicaciones funcionen según lo esperado. Además, asegúrate de monitorear el rendimiento y la salud del clúster distribuido para detectar cualquier problema potencial y realizar ajustes según sea necesario.

En resumen, al configurar el clúster distribuido de ElastiCache de esta manera, puedes asegurar que todas las operaciones de escritura se realicen en el nodo primario en Irlanda, mientras que las operaciones de lectura pueden distribuirse entre ambos nodos en Irlanda y Frankfurt según sea necesario. Esto puede ayudar a optimizar el rendimiento y la consistencia de tus aplicaciones mientras garantiza la alta disponibilidad y la durabilidad de los datos.

---
**Usar Proxy con RDS, mediante validacion con IAM:**
#aws 
https://chat.openai.com/share/161562b2-1156-464d-8d0a-9fc4874f9134

la combinacion ideal seria RDS con IAM, Proxy con IAM.
Eso permite varios perfiles, por lo visto. --> Verificarlo


---

Existe la posibilidad de crear replicas de **lectura** que escalen automaticamente en funcion de CPU o connections


![[Pasted image 20240512185316.png]]

---

#terraform

### ¿Puedo traducir una arquitectura AWS existente a formato CloudFormation o Terraform?

Sí, es posible traducir una arquitectura existente a formato CloudFormation o Terraform, pero no es un proceso completamente automatizado y puede requerir algunos pasos manuales. Aquí te explico cómo hacerlo:

#### 1. **Usando CloudFormer para CloudFormation**

AWS proporciona una herramienta llamada **CloudFormer**, que puede ayudarte a crear una plantilla de CloudFormation basada en tus recursos existentes en AWS.

1. **Iniciar CloudFormer:**
    
    - Lanza una instancia de CloudFormer desde la consola de AWS Management.
    - Accede a la interfaz web de CloudFormer.
2. **Seleccionar Recursos:**
    
    - Utiliza la interfaz de CloudFormer para seleccionar los recursos que deseas incluir en tu plantilla de CloudFormation.
3. **Generar la Plantilla:**
    
    - CloudFormer generará una plantilla de CloudFormation basada en los recursos seleccionados.
    - Descarga la plantilla generada y adáptala según tus necesidades.

#### 2. **Usando Terraformer para Terraform**

Para Terraform, puedes usar una herramienta de código abierto llamada **Terraformer**.

1. **Instalar Terraformer:**
    
    - Instala Terraformer siguiendo las instrucciones disponibles en el [repositorio oficial de GitHub](https://github.com/GoogleCloudPlatform/terraformer).
2. **Configurar y Ejecutar Terraformer:**
    
    - Configura tus credenciales de AWS.
    - Ejecuta Terraformer para extraer los recursos de AWS y generar archivos de configuración de Terraform.
    - Comando de ejemplo:
        
        `terraformer import aws --resources=vpc,subnet,ec2 --regions=us-east-1`
        
3. **Refinar la Configuración:**
    
    - Revisa y ajusta los archivos generados para asegurar que cumplen con tus requerimientos.

----

###### Bastion de acceso a BBDD

puedes usar un bastion host como punto de entrada seguro para acceder a una base de datos Amazon Aurora que esté ubicada en una VPC privada. Un bastion host actúa como un punto de entrada seguro para acceder a instancias dentro de una red privada desde una red pública, como Internet.

Aquí te muestro un ejemplo de cómo podrías configurarlo:

1. **Crea un Bastion Host**: Configura un bastion host en una subred pública de tu VPC. Asegúrate de que tenga las medidas de seguridad adecuadas, como grupos de seguridad que limiten el acceso a direcciones IP específicas y claves SSH para autenticación.
    
2. **Configura la Seguridad del Bastion Host**: Ajusta los grupos de seguridad del bastion host para permitir el acceso desde tu dirección IP pública y desde la VPC donde se encuentra tu Amazon Aurora.
    
3. **Accede al Bastion Host**: Utiliza una conexión SSH para acceder al bastion host desde tu máquina local o desde cualquier otro lugar con acceso a Internet.
    
4. **Accede a la Base de Datos Aurora**: Desde el bastion host, puedes utilizar una conexión de línea de comandos o una herramienta de administración de bases de datos para acceder a la base de datos Amazon Aurora que está en una subred privada de tu VPC.
   
5. **Configura un túnel SSH desde tu máquina local al bastion host**: Utiliza el comando SSH en tu terminal local para crear un túnel SSH que redirija un puerto local hacia el puerto de la base de datos en el bastion host. Por ejemplo:
    
    `ssh -i tu_llave_privada.pem -N -L 3306:nombre_host_aurora:3306 usuario@bastion_host`
    
    Esto crea un túnel SSH que redirige el tráfico desde el puerto 3306 de tu máquina local al puerto 3306 del host de Aurora a través del bastion host.
    
- **Configura la conexión a la base de datos desde tu máquina local**: Ahora puedes configurar tu aplicación o herramienta de administración de bases de datos para que se conecte a `localhost:3306` (o al puerto local que hayas especificado en el túnel SSH) como si estuviera conectándose directamente a la base de datos en Aurora. El tráfico se redirigirá a través del túnel SSH y pasará por el bastion host hacia la base de datos.


En caso de que el bastion sea para un EC2: 
En este caso hay dos .pem, el del bastion y el del Ec2.

es posible configurar todo el túnel SSH desde tu máquina local utilizando un solo comando, combinando las dos conexiones SSH necesarias. Aquí está el comando que puedes utilizar:


`ssh -i llave_privada_bastion.pem -N -L puerto_local:nombre_host_ec2:puerto_remoto usuario@direccion_ip_bastion -i llave_privada_ec2.pem -N -L puerto_local:nombre_host_ec2:puerto_remoto usuario@direccion_ip_ec2`




