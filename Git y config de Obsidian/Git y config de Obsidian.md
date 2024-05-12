
Te puedes crear un repo en el mac que apunte a una carpeta de GitHub.

Lo mas importante es, al crear el repo o importarlo, vas a .git/config y pones el usuario de gitHub personal. 

ver config global:
git config --global --list

ver config local:

git config --list

----
Proceso para poner un repo con tu configuracion de git local:
git init

 añadir el user a .git/config:

  - mediante consola:
  
   git config user.name "Tu Nombre"
   git config user.email "tu@email.com"

  - O editando el .git/config a mano: 
    [user]
      name = Tu Nombre
      email = tu@email.com

- Ir a gitHub y crear / coger la url del repositorio
- conectar el repo local con el remoto:
git remote add origin https://github.com/dcarrascal75/obsidian.git

He tenido que crear un token nuevo: 

```
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  --> toquen for obsidian

```


Problema con varios vaults: 
- Al crear un vault nuevo, los plugins no se copian. Parece ser que una solucion es copiar la carpeta oculta .obsidian del vault.

**Contraseña de gitHub:**

En Mac, con incluirlo una vez ya funciona. Se queda grabado en el keychainAceess
login/passwords/github

- Qué hacer si quiero cambiar mi contraseña de gitHub o eliminarla:
- ![[Pasted image 20240428193259.png]]
  
  
