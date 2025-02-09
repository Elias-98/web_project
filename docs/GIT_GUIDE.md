# Git guide

Guía de configuración y comandos de Git y GitHub.

Por [Joel Hernández](https://github.com/JoelHernandez343).

***

> Nota:
> 
> `Git` es un software de control de versiones.
>
> `GitHub` es un sitio web que aloja repositorios usando `Git`.

***

## Índice
- [Instalación](#instalación)
    - [Windows](#windows)
    - [Linux](#linux)
    - [Mac](#mac)
- [Configuración inicial](#configuración-inicial)
- [Clonando el repositorio](#clonando-el-repositorio)
- [Commits](#commits)
    - [Agregando cambios al Staging Area](#agregando-cambios-al-staging-area)
    - [Creando un commit](#creando-un-commit)
- [Subiendo tus commits al repositorio](#subiendo-tus-commits-al-repositorio)
- [Conflictos](#conflictos)
- [Resumen](#resumen)
- [Ver también](#ver-también)

***

## Instalación
### Windows
1. Descargar instalador ([aquí](https://git-scm.com/download/win))
2. Abrir el instalador y prácticamente dar **siguiente** y **siguiente**, a menos que se quiera otra configuración.
    - Recomendado usar Code para el editor de texto por defecto.
    - Recomendado usar la opción *Git from the command line and also from 3rd-party software*:
    <img src="win01.png" width="60%">

### Linux
- Si no lo tienes aun instalado (Debian/Ubuntu):
```ssh
sudo apt install git
```
- Para otras distros, instrucciones [aquí](https://git-scm.com/download/linux).

### Mac
- Lista de instrucciones [aquí](https://hackernoon.com/install-git-on-mac-a884f0c9d32c).

Para comprobar que tienes instalado Git, debes poder ejecutar el siguiente comando:
```ssh
git --version
# Salida:
git version 2.20.10
```
***

## Configuración inicial
1. Crear una [cuenta en GitHub](https://github.com/).
2. Configurar con tu **nombre de usuario** y **correo** desde la terminal o desde Git Bash (Windows):
```ssh
git config --global user.name "Tu nombre aquí"
git config --global user.email "tu_correo@correo.com"
```
3. (Opcional) Colores en git:
```ssh
git config --global color.ui true
```
4. (Opcional) Editor de tu preferencia (Windows desde el instalador te lo configura):
```ssh
git config --global core.editor code
```

***

## Clonando el repositorio
1. Crear una carpeta donde alojar el proyecto.
2. Clonar el repositorio:
```ssh
git clone https://github.com/JoelHernandez343/web_project.git
```

***

## Commits
Un **commit** es una `snapshot` local de tu trabajo actual que puede ser compartida con los demás miembros del equipo. Mientras tus cambios _no_ estén en un **commit**, estos _no_ podrán ser compartidos.

Es importante que los **commits** que tu hagas sean _constantes_ pero _representativos_.

### Agregando cambios al Staging Area
Es importante que los documentos que edites sean _rastreados_ por Git para que los cambios puedan ser controlados. Para que un documento nuevo o un cambio nuevo sea _rastreado_ por Git, tenemos que agregarlos al `staging area`, que es la fase de preparación para hacer un **commit**.

Podemos observar el estado de nuestro repositorio con:
```ssh
git status
```
Este comando nos mostrará los documentos editados, borrados, creados o que ya se encuentran preparados para hacer el **commit**, por ejemplo:
```ssh
git status
# Salida:
En la rama master
Tu rama está actualizada con 'origin/master'

Cambios no rastreados para el commit:
...
    modificado:  GIT_GUIDE.md
    borrado:     prueba1.txt
...
```
Para decirle a Git que un archivo está listo para el **commit**, ejecutamos el siguiente comando:
```ssh
git add <archivo>
```
Si queremos agregar ***todos los cambios*** que hayamos hecho, ejecutamos:
```ssh
git add .
```

### Creando un commit
Ya teniendo todos nuestros cambios _rastreados_ por Git, podemos crear el **commit** de la siguiente forma:

```ssh
git commit -m "Descripción del commit"
```

***

## Subiendo tus commits al repositorio
Asegúrate que haz hecho el **commit** o los **commits** que deseas subir.
1. Primero sincroniza la rama **origin/master** con el repositorio local:
```ssh
git pull --rebase
```
> Si hay un conflicto, ve [Solucionando conflictos con origin/master](#conflictos)
2. Una vez que no hay conflictos, puedes hacer el `push` a **origin/master**:
```ssh
git push
```
Este comando te pedirá tu **usuario** y **contraseña** de tu cuenta de GitHub.

Y eso es todo, tus cambios estarán disponibles para los demás! :blush:

***

## Conflictos
Un conflicto entre uno o varios **commits** puede darse a diversas razones:
1. Modificaste un archivo que ha sido eliminado en **origin/master**.
2. Modificaste las mismas líneas de código que otra persona y Git no sabe qué líneas conservar y cuales no.
3. Se combinaron **commits** en **origin/master** y Git no puede comparar apropiadamente tu árbol histórico de **commits** con el del repositorio (cosa que no haremos).

### Conflictos de fusión
Por ejemplo, digamos que en **origin/master** alguien escribió en `main.js` lo siguiente:
```js
...
console.log('Hi there!');
...
```
Y nosotros _commiteamos_ en `main.js` lo siguiente:
```js
...
console.log('Hello World!');
...
```
Al momento de ejecutar nuestra sincronización con el repositorio, veremos lo siguiente:
```ssh
git pull --rebase
# Salida:
...
CONFLICTO (contenido): Conflicto de fusión en main.js
error: Falló al fusionar en los cambios.
...
Resuelva todos los conflictos manualmente ya sea con 
"git add/rm <archivo_conflictivo>", luego ejecute "git rebase --continue".
...
```
Como esperábamos, hay un conflicto que tendremos que resolver antes de poder _pushear_ nuestros **comits**.

Al observar `main.js` veremos lo siguiente:
```js
<<<<<< HEAD
console.log('Hi there!');
=======
console.log('Hello World!');
>>>>>> Nombre del commit problemático
```
Editamos el archivo eliminando las marcas de conflicto y dejando las líneas que deseemos que se conserven, por ejemplo:
```js
// main.js
console.log('Hi there!');
console.log('Hello World!');
```
Una vez resuelto el problema, volvemos a preparar `main.js` para continuar con el proceso de sincronización:
```ssh
git add .
```
Continuamos con la sincronización:
```ssh
git rebase --continue
```
Y ahora sí, podremos _pushear_ nuestros cambios que hayamos hecho :ok_hand::
```ssh
git push
```

### Conflictos de eliminación:
Digamos que en **origin/master** se ha eliminado `main.js` porque ya no era necesario, pero en nuestra copia local, nosotros hemos modificado el código.
Al hacer la sincronización, veremos lo siguiente:
```ssh
git pull --rebase
# Salida:
...
CONFLICTO (modificar/borrar): main.js borrado en HEAD y modificado en ... 
Falta versión Conflicto de eliminación de main.js en el árbol.
error: Falló al fusionar en los cambios.
...
```
Como vemos que es un conflicto de borrado, decidimos si eliminar `main.js` o dejarlo. Si lo conservamos, tenemos que agregarlo y luego hacer _push_. De lo contrario, eliminamos `main.js`.

Una vez arreglado los conflictos, ejecutamos:
```ssh
git add .
git rebase --continue
```
Y ahora sí, podremos _pushear_ nuestros cambios que hayamos hecho :ok_hand::
```ssh
git push
```
***

## Resumen

1. Para agregar los cambios que hayamos hecho **antes** de hacer algún **commit**, ejecutamos el siguiente comando:
```ssh
git add .
```
2. Para crear el **commit**:
```ssh
git commit -m "Descripción del commit"
```
3. Sincronizamos nuestro repositorio local con el repositorio en línea **origin/master**:
```ssh
git pull --rebase
```
4. Resolvemos [cualquier conflicto](#conflictos).
5. Compartimos nuestros cambios / **commits** con el resto del equipo al _pushearlos_:
```ssh
git push
```

***

## Ver también
- [Your first time with git and github](https://kbroman.org/github_tutorial/pages/first_time.html)
- [git - the simple guide](https://rogerdudler.github.io/git-guide/)
- [Configuración de un repositorio (Bitbucket)](https://www.atlassian.com/es/git/tutorials/setting-up-a-repository)