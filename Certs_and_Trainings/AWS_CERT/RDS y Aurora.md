Diferencias multi AZ, multi Region y Read Replicas: 

- LAs multiAZ son copias en otra AZ para la RDS, no tienen capacidad de ofrecer conectividad para R/W: Solo sirven para FailOver.
- LAs Read Replica ofrecen capacidad para Read, y ademas se pueden alojar en otra AZ y por tanto también ofrecen failover.



![[Pasted image 20240505203345.png]]


### Aurora

Ojo, no es lo mismo RDS que Aurora. Aurora lleva su propia forma de gestionar las cosas

  Aurora se administra automáticamente en un entorno de múltiples zonas de disponibilidad (Multi-AZ), lo que significa que, **aunque tengas solo una instancia**, esta se replica automáticamente en al menos dos zonas de disponibilidad dentro de una región de AWS.
  
Aunque Amazon Aurora ofrece alta disponibilidad con su configuración Multi-AZ, las réplicas de lectura siguen siendo útiles para mejorar el rendimiento, distribuir la carga de trabajo y proporcionar redundancia en caso de desastres. Aunque Aurora tiene failover automático, las réplicas de lectura pueden servir como puntos de failover adicionales en ciertos escenarios, garantizando la continuidad del servicio. En resumen, las réplicas de lectura complementan la arquitectura de alta disponibilidad de Aurora.

Con Aurora se puede autoescalar el numero de replicas de lectura. Master solo hay uno.

#todo
investigar el backtrack para Point in time recovery

![[Pasted image 20240505213933.png]]
