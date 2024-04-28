#kustomize
#kubernetes

![[Pasted image 20240401095414.png]]

La cabecera de apiversion es opcional.

==Transformers==: modifican algun field
==Generators==: Crean algun recurso, p.ej configMaps

Ademas de estos campos se añaden varios tags "convenience fields": commonLabels, namePrefix...

----

Aquí hay una lista de algunos de los "convenience fields" más comunes que puedes encontrar en Kustomize:

1. `namePrefix`: Prefijo que se agrega al nombre de todos los recursos generados.
2. `nameSuffix`: Sufijo que se agrega al nombre de todos los recursos generados.
3. `namespace`: Define el namespace en el que se desplegarán los recursos.
4. `commonLabels`: Etiquetas comunes que se aplican a todos los recursos generados.
5. `commonAnnotations`: Anotaciones comunes que se aplican a todos los recursos generados.
6. `resources`: Lista de recursos que se despliegan en el clúster.
7. `patches`: Especifica los parches que se aplicarán a los recursos base.
8. `patchesJson6902`: Parches JSON6902 que se aplican a los recursos base.
9. `patchesStrategicMerge`: Parches Strategic Merge que se aplican a los recursos base.
10. `configMapGenerator`: Genera ConfigMaps automáticamente basándose en archivos de configuración.
11. `secretGenerator`: Genera Secrets automáticamente basándose en archivos de configuración.
12. `vars`: Define variables que pueden ser utilizadas en otros campos.
13. `configurations`: Permite definir configuraciones personalizadas para su uso en todo el archivo Kustomization.
14. `images`: Permite definir imágenes de contenedor y sus tags para realizar sustituciones en los recursos YAML.
15. generatorOptions: Se añaden opciones al generator, como por ejemplo deshabilitar el hash que se crea en cada version del config map

---
(al usar el configmap generator, cada cambio que se añade a un configmap genera una version nueva con un hash al final del nombre, pero se puede deshabilitar con *disablenamesuffixhash* )

```
configMapGenerator:
  - name: mysql-config
    envs:
      - mysql-config.properties

generatorOptions:
  disableNameSuffixHash: true
  labels:
    generated: "true"
```


==Resources==: 
Contienenen un path a un file o una url. incluso puede ser un git repository.
Esto seria interesante para poder poner una base comun a todos los componentes

```
resources:
- mysql
- https://github.com/galonge/udemy-kustomize-mastery.git//code-samples/3-the-kfile/wordpress/kustomize/base
```

Fijate que el formato es el de hashicorp: https://github.com/hashicorp/go-getter#subdirectories
Se pone doble slash (//) al final del .git para los subdirectorios !!

(seria curioso ver que svc account usa desde jenkins para traer un git)


==Namespace== convenience fields: 
Si se pone a alto nivel, sobreescribe los de los niveles inferiores. Es decir, no tiene mas prioridad el de nivel inferior sino el de arriba (parece logico para poder reutilizar modulos).
El convenience field de namespace por supuesto no crea el recurso namespace.
Si queremos crearlo, lo que se haria seria usar una definición yaml del namespace y ponerla a nivel de resource:

```
resources:
- mysql
- ../base
- namespace.yaml
```

Eso si te la crearia

Otra opción es definir un transformer y configurarlo para que sólo te añada el namespace de alto nivel si no está definido en el recurso:

transformers/namespace.yaml
```
apiVersion: builtin
kind: NamespaceTransformer
metadata:
  name: only-if-unset //este nombre es irrelevante
  namespace: lec-15
unsetOnly: true
```

unsetOnly: true --> sobreescribo solo si no existe 

El transformer se referencia asi: 
```
transformers:
- ./transformers/namespace.yaml
```

---
Labels and annotations:

Existen dos convenience fields:
- Labels: Por defecto, solo actualiza las labels, no los selectores
- commonLabels: Por defecto, actualiza Labels Y selectores (ojo!)

(lo logico pues es usar labels).
Puedes usar "includeSelectors" e "includeTemplates" 

![[Pasted image 20240401185642.png]]

- CommonAnnotations: añade anotaciones 
```
commonAnnotations:
  mi-anotacion: valor
```


Ojo con tener notaciones de commonLabels a primer nivel, afecta a todo!

