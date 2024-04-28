#kustomize
#kubernetes

Fecha: 2024-04-01


==**curso**:==
https://adidas-itlearning.udemy.com/course/kustomize-mastery-manage-kubernetes-configuration-with-ease/learn/lecture/36036748#overview

**==Otros recursos:==**

- [https://devopscube.com/kustomize-tutorial/](https://devopscube.com/kustomize-tutorial/)
- [https://devopscube.com/kuztomize-configmap-generators/](https://devopscube.com/kuztomize-configmap-generators/)

-----------
**==Doc Oficial:==**
https://kubectl.docs.kubernetes.io/references/kustomize/
--------------

Simplemente coge unos templates y les aplica (rendering) determinada configuración para producir una salida que se pueda aplicar.

Se pueden aplicar varias kustomization files como en una pila: 
![[Pasted image 20240401080101.png]]


killerkoda:

[https://killercoda.com/playgrounds/scenario/kubernetes](https://killercoda.com/playgrounds/scenario/kubernetes)

Repository: 
https://github.com/galonge/udemy-kustomize-mastery

git clone https://github.com/galonge/udemy-kustomize-mastery



==**Nota**==: Cuando estas esperando a un cambio en k8s, por ejemplo, pod created, en vez de estar haciendo "get pods" puedes poner **get pods -w** (watch) y cambia cuando haya un cambio.

==**Nota**==: Se puede hacer un apply -f o un delete -f sobre no solo un fichero sino sobre un directorio con varios ficheros (e.g varios deployments y services)

---
Existe un fichero kustomization, que dice que recursos están dentro de kustomize:

![[Pasted image 20240401085035.png]]


En este caso el directorio contiene dos ficheros normales (deployment y service) , este kustomization los aplica y pone simplemente una label comun a version: v1


O tambien podemos referenciar simplemente una folder donde este todo:
![[Pasted image 20240401090200.png]]

Si hago referencia a un directorio en un kustomize.yaml, kustomize espera que en ese directorio exista tambien un fichero kustomize.yaml. ==Así se crean las layers de kustomizacion.==


Si solo hacemos 
kubectl kustomize *folder* 

lo que nos saca por pantalla es el resultado pero sin aplicarlo.

Para aplicalo seria 
kubectl apply ==-k== *folder*

(es decir, igual que tenemos apply -f, tenemos apply -k para kustomize)


---
kustomize viene incluido en kubectl > 1.14, pero se puede gestionar como un paquete autonomo para evitar que quede outdated.

Tambien existen imagenes docker para kustomize:
![[Pasted image 20240401094353.png]]







