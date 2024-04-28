#kustomize
#kubernetes 


build : 

completion 

cfg 

create: genera un kustomize

edit

help

version

--------

Se pueden añadir helmCharts a un kustomization para que los despliegue.

Se debe ejecutar kustomize on el flag --enable-helm
![[Pasted image 20240402202014.png]]

Los Helm charts se pueden sacar de: artifactHUB

![[Pasted image 20240402202245.png]]

kustomize build . > deploy.yaml *--enable-helm*

Al lanzar un kustomization con helm, te crea un dir "charts" y descarga en el dir los charts de helm.




