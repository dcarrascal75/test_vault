#helm

Añadir repos, p.ej si quiero usar wordpress tengo que añadir a mi helm los repos de bitnami, y entre ellos esta el helm.

- lista de repos: 

```
helm repo list
```

Añadir repo:

```
helm repo add
helm install ...
```

![[Pasted image 20240428202636.png]]

Hay repos por ejemplo en artifactshub.

 otra forma es usar el buscador en línea de comandos de helm
```
helm search hub wordpress
```

 cuando ya haya elegido que paquete instalar puedo ver los valores configurables usando el comando show values:
 ```
 helm show values bitnami/wordpress
 ```
 esos valores se pueden pasar directamente en el comando helm install como parámetros 
 ```
 helm install miwordpress bitnami/wordpress --wordpressUser=admin ....
 ```
 
 o se puede usar un fichero de configuración yaml:
 ```
 install.yaml:
 wordpressUser: admin
 wordpressPasseord: xfdfdfsf
 ```
Y ya seria 
```
helm install miwordpress -f install.yaml bitnami/wordpress
```

**==OJO==**: Aqui no me queda claro como sabe el helm en que namespace etc tiene que instalar.

Par borrae el chart del cluster: 

helm delete miwordpress

**crear un chart para distribuir**

```
helm create miapp
```

Esto te crea una carpeta esqueleto "miapp" con el contenido de templates, charts, values.yaml...

Helm esta hecho en Go.

En los templates es donde se have referencia a los values de values.yaml

Una vez la tengo, se puede empaquetar con 
```
helm package miapp
helm repo index .   //genera descriptor index.yaml del directoio actual (.)
//subirlo a un repo
//copiar a una carpeta remota en el servidor que corresponda. Con Harbor se hara como sea.
```

Y luego ya se puede usar helm repo add como se ha visto




 
