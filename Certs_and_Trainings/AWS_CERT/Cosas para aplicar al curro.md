
#tech_improvement 

---

**Aurora Cloning**: Hay una cosa que es "Aurora Cloning", que parece ser mas rapido que crear un snapshot y regenerarlo.

![[Pasted image 20240504081534.png]]

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Managing.Clone.html

En mi caso de uso actual, necesito los snapshots, habria que ver si hay alguna ventaja en hacer el clone.

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