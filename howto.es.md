

# Índice

* [Chuleta](#chuleta)
* [Paso a Paso](#paso-a-paso)
    * [1. Instalar y configurar Git y Retext y crear cuenta en Github](#1-instalar-y-configurar-git-y-retext-y-crear-una-cuenta-en-github)
    * [2. Forkear y clonar este repositorio](#2-forkear-y-clonar-este-repositorio)
    * [3. Editar archivos y realizar cambios](#3-editar-archivos-y-realizar-cambios)
    * [4. Actualizar cambios en el servidor remoto](#4-actualizar-cambios-en-el-servidor-remoto)
    * [5. Tu primer Pull Request](#5-tu-primer-pull-request)
    * [6. Trabajar en Ramas](#6-trabajar-en-ramas)
    * [7. Actualizar tus repositorios locales](#7-actualizar-tus-repositorios-locales)
* [Personas que están aprendiendo con este tutorial](#personas-que-están-aprendiendo-con-este-tutorial)
* [Referencias](#referencias)

# Chuleta

Puedes saltar esta sección, sirve de referencia una vez hayas realizado todos los pasos del tutorial.


## En Branch y Fork sin acceso a repositorio original

```shell
                            # PRIMERO: crear fork del respositorio
$ git clone URL_FORK        # clonar un repositorio remoto en local
$ git pull origin master    # actualizar el repositorio local con el remoto
$ git add *                 # añadir docs modificados a Staging
$ git commit -m "update"    # guardar cambios de los docs modificados
$ git push origin master    # subir los cambios al repositorio master
                            # Solicitar PR al repo original
$ git remote -v             # comprobar repositorios remotos declarados
$ git remote add upstream URL_ORIGINAL # añadir repositorio original 
$ git fetch upstream        # actualizar los cambios del repo original
$ git merge upstream/master # con esto fusionamos nuestro master local con el original
$ git status                # comprueba el estado de tus cambios y ramas
```


## En Master con acceso al repositorio original con permisos en Master

```shell
$ git clone URL             # clonar un repositorio remoto en local
$ git pull origin master    # actualizar el repositorio local con el remoto
$ git add *                 # añadir docs modificados a Staging
$ git commit -m "update"    # guardar cambios de los docs modificados
$ git push origin master    # subir los cambios al repositorio master
$ git status                # comprueba el estado de tus cambios y ramas
```

## En Branch con acceso al repositorio original sin permisos en Master

```shell
$ git pull origin RAMA      # actualizar el repositorio
$ git checkout -b RAMA      # crear y entrar en la rama
$ git add *                 # añadir docs modificados a Staging
$ git commit -m "update"    # guardar cambios de los docs modificados
$ git push origin RAMA      # subir los cambios al repositorio externo
                            # esto permite hacer un PR en GitHub
$ git checkout master       # cambiar a master
$ git merge RAMA            # actualizar master con los cambios de la rama
$ git branch -d NOMBRE_RAMA # borrar rama
$ git status                # comprueba el estado de tus cambios y ramas
```



# Paso a paso

A continuación te muestro un diagrama con los pasos que vamos a ir dando en el turorial. No te asustes con la complejidad del diagrama. Iremos paso a paso y al final del tutorial te servirá como referencia para ordenar todas las acciones que vamos a realizar. En el diagrama diferenciamos tres espacios de trabajo: arriba del todo mi ordenador, abajo del todo el tuyo, en medio GitHub, donde diferenciamos el espacio de tus respositorios y del mío. Las flechas en rojo aparecen las acciones, archivos y repositorios que me tocan a mi, en negro lo que vas a crear y hacer tú.

![Diagrama de flujo y acciones de este tutorial](img/diagram-git-github.png)

Aunque ahora mismo el diagrama tenga demasiado detalle para ti que acabas de empezar, podemos comenzar a usarlo para comprender la lógica básica de trabajar con Git y con Github. 

La idea fundamental que captura el diagrama es que en Git y en Github trabajamos con repositorios de documentos (que fundamentalmente contienen carpetas y documentos de texto). Para simplicarlo todo imaginemos que sólo hay un documento. Las líneas horizontales representan la historia de de este repositorio (p.e. las diferentes versiones de un documento, cada vez que modificamos algo en un documento se creará una nueva versión y nos deslizamos hacia la derecha en la líneas horizontales). 

Para el caso que nos ocupa existen cuatro versiones fundamentales de este documento que lees ahora: la que está en mi ordenador (la linea superior roja), la que está en mi cuenta de github (segunda línea roja), la que vas a crear en tú dentro de Github (primera línea negra) y finalmente la que vas a tener en tu ordenador. También existirá una versión (que se muestra en la última línea, discontínua) temporal en tu ordenador cada vez que abras el editor de textos ```retext```, la memoria RAM retendrá la copia de los cambios que vas realizando en el archivo. Cuando le das a guardar, esta versión se copia y modifica el archivo original. De aquí que la última "línea" que  hay en el diagrama sea discontínua (porque sólamente existe temporalmente mientras editas el texto).

Las líneas diagonales que salen de las horizontales para volver a juntarse con ella se llaman ramas (```branche``` en inglés). Las líneas verticales son, fundamentalemente, acciones orientadas descargar repositorios, o a subir o actualizar copias. En tu caso, cada vez que una flecha vaya de arriba-abajo indicará que te estas descargando algo o actualizando tus documentos con los repositorios o versiones de una línea de órden superior. Cuando la flecha vaya de abajo-arriba será que guardas un cambios o empujas un cambio de un repositorio o otro de órden superior. 

Por ahora basta con que comprendas esta lógica fundamental y comencemos paso a paso a completar las acciones el diagrama. 


## 1. Instalar y configurar Git y Retext y crear cuenta en Github

Instala git y retext en Ubuntu con el siguiente comando:

```shell
$ sudo apt-get install git-all  # Instalar git
$ sudo apt-get install retext   # Instalar editor Retext
```

`Retext` nos va a servir par poder editar archivos en lenguaje Markdown (como este mismo, si necesitas una introducción a Markdown puedes consultar [esta guía introductoria a Markdown](https://github.com/xabier/escritura-colaborativa-github/blob/master/lenguajes-de-marca.md)), puedes utilizar cualquier otro editor, incluso un editor de texto plano.

Si no la tiene aún [create una cuenta en github](https://github.com/join?source=login).

Usa tu nombre de usuario de GitHub para configurar la autoría de tus cambios:

```shell
$ git config --global user.name "TU_NOMBRE"
```

Define una dirección de correo electrónico:

```shell
$ git config --global user.email "TU_CORREO_ELECTRONICO"
```

Ahora creamos una carpeta donde vamos a almacenar todos los repositorios con los que trabajemos a partir de ahora, para crearla directamente en la home:

```shell
$ cd                    # Volver a Home
$ mkdir repos-git       # crear directorio o carpeta "repos-git"
```

## 2. Forkear y clonar este repositorio

Empezar con este repositorio no es tan fácil como empezar de cero porque este repositorio no es tuyo, tú no puedes contribuir directamente aquí. Pero empezar sin nada que clonar tampoco es fácil. Así que las cosas serán un poquito más complicadas al principio pero un poco más fáciles después. Estos son los pasos que vamos a dar:

Vas a crear un fork de este repositorio, un fork es una copia con la que podrás trabajar directamente, ya que este fork o copia será tuya, y en tus propio repositorio no tienes que pedir permiso a  nadie.

![Hacer un fork de este proyecto](img/fork.png)

Ahora que ya tienes tu fork puedes clonar y descargarte el repositorio a tu ordenador y comenzar a trabajar para ello harás:

```shell
$ cd ~/repos-git        # con esto nos movemos a nuestra carpeta de repositorios
$ git clone https://github.com/TU_NOMBRE_DE_USUARIO/github-minihowto
```

Sustituyendo tu ```TU_NOMBRE_DE_USUARIO``` por tu nombre de usuario en Github. Ahora podrás comprobar que en la carpeta ```repos-git``` tienes ya la carpeta o directorio en el que has descargado el repositorio remoto

```shell
$ ls                    # lista los directorios que hay
                        # verás que aparece github-minihowto
$ cd github-minihowto   # para moverse a la nueva carpeta
```

Ya estamos preparadas para editar archivos y contribuir.


## 3. Editar archivos y realizar cambios

Vas a la carpeta que tienes en local y editas un documento cualquiera, por ejemplo 

```shell
$ retext howto.es.md
```

Lo que permite Retext, a diferencia de otros editores, es ver en tiempo real cómo se formatea Markdown. Para eso tienes que activar la visualización en pantalla partida, a la izquierda verás el código de Markdown y a la derecha cómo queda visualmente. Para ello haz click en el icono siguiente y activa la opción ```live preview``` que sale cuando le das al botón desplegable:

![Preview Retext](img/retext-preview.png)

Ahora deberías de poder ver una imagen similar a esta:

![Imagen de este documento abierto en Retext](img/retext.png)

Realizas cambios (por ejemplo puedes añadir tu nombre al final del archivo), los guardas con el editor y luego avisas a git de que ha realizado cambios en ese archivo:

```shell
$ git add howto.es.md
```

Si has modificado varios documentos puedes añadir todos a la vez con:

```shell
$ git add *
```

Con esto lo que hemos conseguido es añadir los cambios al Index o Staging, que es, como el nombre indica, el índice de los archivos modificados, pero aún no hemos "guardado" esos cambios. Para eso tenemos que realizar un commit:

```shell
$ git commit -m "He añadido mi nombre en la lista"
```

El texto entrecomillado es el mensaje con el que se guardarán dichos cambios explicando a terceros cual es, en resumen, la aportación o modificaciones que has realizado.


## 4. Actualizar cambios en el servidor remoto

Lo que queremos hacer ahora es empujar (```push```) nuestros cambios en local (en nuestro ordenador) a la rama maestra (```master```) que está en nuestro repositorio forkeado en github, para eso hacemos:

```shell
$ git push origin master
```

La propia línea de comandos te pedirá tu nombre de usuario y tu password en github.

Enhorabuena, acabas de actualizar tu repositorio en Github con los cambios que has realizado.


## 5. Tu primer Pull Request

Ahora vamos a realizar tu primer Pull Request o PR: pedir que incorporen tus mejoras en el original del que forkeaste, para que este manual quede actualizado con los cambios que has realizado en tu fork. Si visitas tu repositorio forkeado verás que aparece un botón que te permitirá hacer le PR al original. 

A partir de aquí entrarás en una fase de revisión o discusión del PR, puede que te pida que hagas más cambios, o corregir errores, o nada. En un momento dado aceptaré tu PR y habrás contribuído satisfactoriamente a este manual :smile: Enhorabuena!

## 6. Trabajar en Ramas

Hasta ahora has modificado los archivos de la carpeta original de tu fork, lo que se llama rama maestra o master. Sin embargo, también puedes trabajar sobre una versión temporal que tenga un nombre propio y que recoja todo un conjunto de cambios. A esto se le llama una rama. Por ejemplo podrías querer actualizar todas las capturas de pantalla de este tutorial. Puedes crear una rama e introducir las nuevas capturas. Cuando terminas de realizar los cambios que desees puedes fusionarla (merge) con la versión en la rama maestra.

Primero tenemos que crear una rama:

```shell
$ git checkout -b NOMBRE_RAMA
```

Y para subir (empujar, push) la rama al repositorio remoto hacemos:

```shell
$ git push origin NOMBRE_RAMA
```

Ahora si quieres fusionar tu rama con la maestra. Primero tendras que cambiar a la rama maestra:

```shell
$ git checkout master
```

Y desde aquí ya puedes fusionar (merge) la rama nueva a la rama maestra:

```shell
$ git checkout master
$ git merge NOMBRE_RAMA
```

Para no acumular ramas es recomendable borrarlas una vez esten completadas:

```shell
$ git branch -d NOMBRE_RAMA
```

Ahora vamos a añadir el repositorio original como upstream:

```shell
$ git branch -d NOMBRE_RAMA
```


## 7. Actualizar o sincronizar tus repositorios locales

Si hay cambios en los servidores remotos y tienes que actualizar tu repositorio local. Con este comando actualizarás tu repositorio local con el repositorio master de github

```shell
$ git pull origin master
```

Lo que no hemos conseguido con el comando anterior son dos cosas fundamentales: 1. actualizar tu fork con el original mio, 2. actualizar tu copia en local con el original

Existen dos vías para resolver este problema: a) primero actualizar o sincronizar tu fork en github y luego descargar en local, o, b) hacerlo directamente en local.

### 7.a. Sincronizar tu fork en github y luego en local

Lo que tienes que hacer para sincronizar (actualizar tu repositorio) tu fork con el original es ir a tu Fork y realizar un Pull Request en la dirección original --> fork. Para debes realizar los siguientes pasos:

1. Haz click en ```Pull Requestes```
2. Cambia la base, por defecto Github considerará que los PR se hacen de tu fork al original, pero lo que queremos hacer es justamente lo contrario para poder actualizar nuestro repositorio forkeado. Así que haz click en ```switching the base```
3. Haz click en ```Create Pull Request``` y luego en ```Send Pull Request```
4. Baja hacia abajo para hacer ```Merge```

Ya tienes tu repositorio fork actualizado con el original. Ahora, para actualizar tu repositorio local, basta con que hagas lo siguiente:

```shell
$ git pull origin master
```

### 7.b. Sincronizar repositorios desde tu PC con git

Primero vamos a incluir el repositorio original (que vamos a llamar ```upstream```) como uno de los repositorios remotos. Ten en cuenta que, hasta ahora desde tu PC, sólo tienes referenciado tu Fork, no mi repositorio original. 

Primero, para comprobar qué repositorios remotos tienes declarados hacemos:

```shell
$ git remote -v     # comprobar repositorios remotos declarados
```
El resultado que te tiene que salir es:

```shell
origin https://github.com/TU_NOMBRE_DE_USUARIO/github-minihowto (fetch)
origin https://github.com/TU_NOMBRE_DE_USUARIO/github-minihowto (push)
```

Ahora metemos el repositorio original como upstream:

```shell
$ git remote add upstream https://github.com/xabier/github-minihowto
```

Al volver a revisar los repositorios declarados verás lo siguiente:

```shell
$ git remote -v
origin https://github.com/TU_NOMBRE_DE_USUARIO/github-minihowto (fetch)
origin https://github.com/TU_NOMBRE_DE_USUARIO/github-minihowto (push)
upstream https://github.com/xabier/github-minihowto (fetch)
upstream https://github.com/xabier/github-minihowto (push)
```

Ahora vas a actualizar tu repositorio local con el original

```shell
$ git fetch upstream        # actualizar los cambios del repo original
```

Pero aún no has fusionado tu repositorio local con las actualizaciones del original. Para ello hacemos:

```shell
$ git checkout master       # para estar seguros de estar en la rama maestra
$ git merge upstream/master # con esto fusionamos nuestro master local con el original
```

También podrías ejectuar el comando ```git pull upstream```. Básicamente el comando ```pull = fetch + merge``` por lo que te ahorras un paso, pero entonces perderías los cambios que has realizado en local porque ```pull``` los sobre-escribe, de ahí que se recomiende hacer ```fetch``` para actualizar y poder seguir trabajando sin perder los cambios en local. Esto es particularmente interesante si trabajas con ramas.


# Personas que están aprendiendo con este tutorial

* Xabier
* Vzy

# Referencias

* Markdown 
	* https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
	* https://daringfireball.net/projects/markdown/syntax
* Github
    * https://guides.github.com/activities/hello-world/
* Git and Github
	* https://services.github.com/on-demand/downloads/github-git-cheat-sheet/
	* https://rogerdudler.github.io/git-guide/
	* https://blog.udacity.com/2015/06/a-beginners-git-github-tutorial.html

