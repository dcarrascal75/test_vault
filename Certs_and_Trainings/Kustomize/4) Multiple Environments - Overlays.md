#kustomize
#kubernetes 

Va a usar un ejemplo de Google para microservicios: 

https://github.com/GoogleCloudPlatform/microservices-demo

Puede ser interesante para probar cosas, tiene un servicio de generacion de carga.

---

Se puede usar el tag replicas:

dev/kustomization.yaml

```
namespace: prd

replicas:
-name: frontend
  count: 3

resources:
- "../base"
- namespace.yaml
```

en este caso, como hay varios componentes, en replicas especifica el nombre del componente.

La duda aqui seria, igual kustomize tiene sentido si ponemos todos los deployments en el mismo repositorio, pero para una app que tiene los repositorios separados, no sé si mereceria la pena modificar 200 repos para poner kustomize... O igual se puede poner un base en determinado directorio y luego añadir un kustomization en cada repo. Eso nos permitiria añadir cosas comunes.

---

Un ejemplo de organizacion seria:

https://github.com/galonge/udemy-kustomize-mastery/tree/main/code-samples/6-multiple-envs/day2

dev/kustomization.yaml:

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: dev

resources:
  - "../../base2"
  - namespace.yml

components:
  - ../components/google-cloud-ops
  - ../components/security-context
  - ../components/resource-group-a
```

En base2 estarian todos los componentes sin las cosas comunes.

En components estan las cosas comunes, por ejemplo, el security context para todo lo que sean deployments:

../components/security-context/kustomize.yaml:

```
apiVersion: kustomize.config.k8s.io/v1alpha1
kind: Component

patches:
  - target:
      group: apps
      version: v1
      kind: Deployment
    patch: |-
      - op: replace
        path: /spec/template/spec/securityContext
        value:
          fsGroup: 1000
          runAsGroup: 1000
          runAsNonRoot: true
          runAsUser: 1000
```

O diferentes patches para requests/limits o variables de entorno comunes.

Por ejemplo, preparo diferentes ficheros de patch para diferentes requests/limits:
- ../components/resource-group-a
- ../components/resource-group-b
...

Y como vinculo cada componente-x.yaml con un resource-group?

Lo que se hace es añadir por labels: 

ej: en base2/adservice.yaml:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adservice-deployment
  labels:
    resource-group: resource-group-a
spec
 ....
```

Y luego en el patch: components/resource-group-a:
Usamos un **labelSelector**

```
apiVersion: kustomize.config.k8s.io/v1alpha1
kind: Component

patches:
  - target:
      group: apps
      version: v1
      kind: Deployment
      labelSelector: "resource-group=resource-group-a"

    patch: |-
      - op: replace
        path: /spec/template/spec/containers/0/resources
        value:
          requests:
            cpu: 100m
            memory: 64Mi
          limits:
            cpu: 200m
            memory: 128Mi
```


es decir, ==solo se aplica el patch para los componentes que tienen ese label==

ojo, hay dos securityContext, uno para el pod y otro para cada componente:

![[Pasted image 20240402081830.png]]

esto ojo a la hora de extrapolar el securitycontext.

