#kustomize
#kubernetes 

Notas sobre el caso de uso de kustomize por parte de BCT
[https://bitbucket.tools.3stripes.net/projects/PC/repos/bctf-deployments/browse](https://bitbucket.tools.3stripes.net/projects/PC/repos/bctf-deployments/browse "https://bitbucket.tools.3stripes.net/projects/pc/repos/bctf-deployments/browse")

#### Uso de Resources:
 Son componentes que directamente se añaden al fichero de salida
```
resources:
- ../../../../bases/deployment-api
- ../../../../bases/autoscaling
- ../../../../bases/service
```

#### Uso de Components:
Es muy similar a Resources, la diferencia es que estos se utilizan para organizar y reutilizar configuraciones de manera modular en forma de "componentes".

#### Uso de Patches

Se pasa un fichero o un patch y se dice el "target" al quse se va a aplicar el patch: 

```
patches:
  - path: patch-env-variables-autoscaler.yaml
    target:
      version: v1
      kind: HorizontalPodAutoscaler
```

Y en el fichero patch se hace referencia a la ruta dentro del target:
```
- op: replace
  path: /spec/maxReplicas
  value: 2

- op: replace
  path: /spec/minReplicas
  value: 1
```

El patch se puede escribir directamente: 
```
#use specific patch for modifying just one specific ingress
patches:
  - target:
      kind: Ingress
    patch: |
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: (ignored)
        annotations:
          nginx.ingress.kubernetes.io/enable-cors: "true"
          nginx.ingress.kubernetes.io/cors-allow-methods: "GET, POST, PUT, PATCH, OPTIONS"
```

Valores que puede tomar el campo "op" en un patch en Kustomize:

1. `add`: Agrega un nuevo objeto al recurso.
2. `remove`: Elimina un objeto del recurso.
3. `replace`: Reemplaza un objeto existente en el recurso.
4. `merge`: Fusiona los objetos del patch con los objetos existentes en el recurso.
5. `copy`: Copia un objeto de un lugar a otro en el recurso.
6. `move`: Mueve un objeto de un lugar a otro en el recurso.

#### Uso de patchesStrategicMerge

Son patches que usan la estrategia de mergeo de "strategic merge". son útiles para realizar cambios específicos en los recursos de Kubernetes de una manera más precisa y controlada.
`patches`: Por otro lado, el comando `patches` se utiliza para especificar parches que se aplicarán a los recursos de Kubernetes, pero no especifica una estrategia de fusión específica.
