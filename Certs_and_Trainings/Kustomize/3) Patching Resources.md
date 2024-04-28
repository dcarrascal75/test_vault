#kustomize
#kubernetes 

Se usan para sobreescribir o añadir campos a los recursos.

Dos tipos: 
- Strategic match
- JSON6902

![[Pasted image 20240401190746.png]]

se puede poner un path al patch (yml or json) o definir el patch inline.

AllowNameChange & AllowKindChange permiten cambiar el nombre de los resources. Si no, quedan como estaban.

==Definir el target==

Al definir el target, el group y el version se obtienen del tipo de recurso que se quiera modificar.
Eso se ve haciendo 
```
>kubectl api-resources
```
Y ahi buscas el recurso al que quieras aplicar el patch. La columna "ApiResources" te da los valores.
Por ejemplo para un deployment seria :

![[Pasted image 20240401192308.png]]

group: apps 
version: v1
kind: deploy

En realidad no son obligatorios pero aconsejables.
LabelSelector / annotationSelector tampoco son obligatorios

Ojo, si no existe matching con ningún elemento, no da error, simplemente no se produce el patch.


==Strategic Match hace un replace,== pero se pueden usar otros modos, por ejemplo borrar un elemento del deploy padre. Eso se haria poniendo el modo del patch: 

Pero la forma mas sencilla es usar el conveniencefield **==patchesStrategicMerge==**

```
patchesStrategicMerge:
- service_port_8888.yaml
- deployment_increase_replicas.yaml
- deployment_increase_memory.yaml
```

que tambien puede ser inline:

```yaml
patchesStrategicMerge:
- |-
  apiVersion: apps/v1
  kind: Deployment
```
Sin embargo:
![[Pasted image 20240401194545.png]]
se puede usar y luego convertirlo con ***kustomize edit fix***

El otro formato (aparte del yaml es el json6902, que es mas menos lo mismo pero usando json). los patch son asi: (ojo el fichero tb tiene extension yaml, entiendo que podria ser json)
```
[
  {"op": "replace", "path": "/spec/template/spec/containers/0/resources/limits", "value": {"cpu": "150m", "memory": "150Mi"}},
  {"op": "replace", "path": "/spec/template/spec/containers/0/resources/requests", "value": {"cpu": "120m", "memory": "120Mi"}}
]
```

Y el comando en el kustomize es:
```
patchesJson6902:
  - path: patches/patch-limits-2.yml
    target:
      group: apps
      version: v1
      kind: Deployment
      name: mysql
```


---

Parece ser que al final es tan sencillo como:

(https://devopscube.com/kustomize-tutorial/)

```
resources: 
- ../../base

patches: 
- path: deployment-dev.yaml 
- path: service-dev.yaml
```

donde los ficheros de patch solo contienen las diferencias
Y se ejecuta todo como 
>kustomize build overlays/dev


