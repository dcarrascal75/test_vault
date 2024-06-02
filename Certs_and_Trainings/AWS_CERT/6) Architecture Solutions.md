#aws #AWS_CERT 

se puede habilitar el stickyness para mantener stateful, pero si se muere la instancia es un problema- Por eso otra opcion es usar cookies a nivel de usuario, para que la cookie mantenga p.ej el carrito del usuario vaya a la instancia que vaya. 

Aparte las cookies se pueden alterar y tienen que ser < 4kb

Entonces lo mejor es mantener un ElastiCache con la info del user, y mandar solo la sesionid en el cookie

---
Para arrancar apps lo mas rapido posible:
- usar "golden AMI" --> que tenga todo ya instalado
- usar Bootstrapping usando Datos de Usuario

Pra EBS y RDS:  Restaurar desde snapshots!