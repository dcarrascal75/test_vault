#aws #AWS_CERT 

En IAM (Identity and Access Management), un grupo y un rol tienen una relación indirecta pero complementaria. Aquí se explica cómo se relacionan:

1. **Grupo**: Es una ==colección de usuarios==. Los grupos se utilizan para gestionar y asignar políticas de manera colectiva. Los usuarios en un grupo heredan las políticas (permisos) asignadas a ese grupo.
    
2. **Rol**: Es una ==entidad con un conjunto de permisos== definido por políticas. Los roles pueden ser asumidos por usuarios o servicios para realizar acciones específicas.
    

### Relación Indirecta:

- ==Los **grupos** no tienen roles directamente asignados.== En lugar de eso, se asignan políticas a los grupos, que los usuarios miembros heredan.
- Los usuarios de ==un **grupo** pueden tener la capacidad de asumir **roles**== si las políticas del grupo lo permiten.

### Ejemplo:

- Un grupo "Desarrolladores" tiene una política que permite a sus miembros asumir el rol "AdministradorDeBaseDeDatos".
- Un usuario que pertenece al grupo "Desarrolladores" puede asumir el rol "AdministradorDeBaseDeDatos", adquiriendo así los permisos definidos por las políticas del rol.

En resumen, aunque no se asignan roles directamente a los grupos, las políticas de un grupo pueden permitir a sus miembros asumir ciertos roles, facilitando la administración de permisos de forma eficiente y organizada.

Un grupo puede  usar una politica que sea del tipo "AssumeRole"

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:**AssumeRole**",
      "Resource": "arn:aws:iam::123456789012:role/AdministradorDeBaseDeDatos"
    }
  ]
}

### Flujo de Asunción:

- El grupo "desarrolladores" tiene la policy del Assume role
- Juan pertenece a ese grupo.


1. **Juan llama a `sts:AssumeRole` con el ARN del rol `AdministradorDeBaseDeDatos`.**
2. **AWS verifica que la política del grupo `Desarrolladores` permite esta acción.**
3. **Juan asume el rol `AdministradorDeBaseDeDatos` y adquiere los permisos necesarios para administrar las bases de datos.**

Pare asumir el rol:

aws sts assume-role \
    --role-arn "arn:aws:iam::123456789012:role/AdministradorDeBaseDeDatos" \
    --role-session-name "SesionDeAdministrador"

Esto devuelve unas credenciales temporales y un session token, que se podrian poner como variables de entorno

{
    "Credentials": {
        "**AccessKeyId**": "AKIA....",
        "**SecretAccessKey**": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        "**SessionToken**": "AQoDYXdzEJr...<remainder of security token>",
        "Expiration": "2023-05-18T22:20:00Z"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROACLKWSDQRAEXAMPLE:SesionDeAdministrador",
        "Arn": "arn:aws:sts::123456789012:assumed-role/AdministradorDeBaseDeDatos/SesionDeAdministrador"
    }
}

Para que las solicitudes a AWS sean autenticadas correctamente, necesitas proporcionar las tres partes.


Un grupo no puede contener otros grupos