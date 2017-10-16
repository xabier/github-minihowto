

# Índice

* [Superfast](#Superfast)
* [Paso a Paso](#paso-a-paso)
    * [1. Instalar y configurar git y retext y crear cuenta en github](#instalar-git)
    * [2. Forkear y clonar este repositorio](#forkear-y-clonar-este-repositorio)
    * [3. Editar archivos y realizar cambios](#editar-archivos-y-realizar-cambios)
    * [4. Actualizar cambios en el servidor remoto](#actualizar-cambios-en-el-servidor-remoto)
    * [5. Tu primer Pull Request](#tu-primer-pull-request)
    * [6. Trabajar en Ramas](#trabajar-en-ramas)
    * [7. Actualizar tus repositorios locales](#actualizar-tus-repositorios-locales)
* [Personas que están aprendido con este tutorial](#personas-que-estan-aprendido-con-este-tutorial)
* [Referencias](#referencias)

# Superfast


## En Branch y Fork sin acceso a repositorio original

```shell
                            # PRIMERO: crear fork del respositorio
$ git clone URL_FORK        # clonar un repositorio remoto en local
$ git pull origin master    # actualizar el repositorio local con el remoto
$ git add *                 # añadir docs modificados a Staging
$ git commit -m "update"    # guardar cambios de los docs modificados
$ git push origin master    # subir los cambios al repositorio master
                            # Solicitar PR al repo original
```


## En Master con acceso al repositorio original con permisos en Master

```shell
$ git clone URL             # clonar un repositorio remoto en local
$ git pull origin master    # actualizar el repositorio local con el remoto
$ git add *                 # añadir docs modificados a Staging
$ git commit -m "update"    # guardar cambios de los docs modificados
$ git push origin master    # subir los cambios al repositorio master
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

```



# Paso a paso

Aquí tienes un esquema de todos los pasos que vamos a ir dando:

![Diagrama de flujo y acciones de este tutorial](img/diagram-git-github.png)

No te asustes con la complejidad del diagrama. Iremos paso a paso y al final del tutorial te servirá como referencia para ordenar todas las acciones que vamos a realizar. En el diagrama diferenciamos tres espacios de trabajo: arriba del todo mi ordenador, abajo del todo el tuyo, en medio GitHub, donde diferenciamos el espacio de tus respositorios y del mío. Las flechas en rojo aparecen las acciones, archivos y repositorios que me tocan a mi, en negro lo que vas a crear y hacer tú.


## 1. Instalar y configurar git y retext y crear cuenta en github

Instala git y retext en Ubuntu con el siguiente comando:

```shell
$ sudo apt-get install git-all  # Instalar git
$ sudo apt-get install retext   # Instalar editor Retext
```

`Retext` nos va a servir par poder editar archivos en lenguaje Markdown (como este mismo, si necesitas una introducción a Markdown puedes consultar [esta guía introductoria a Markdown](https://github.com/xabier/escritura-colaborativa-github/blob/master/lenguajes-de-marca.md)), puedes utilizar cualquier otro editor, incluso un editor de texto plano. Lo que permite Retext, a diferencia de otros editores, es ver en tiempo real cómo se formatea Markdown. Para eso tienes que activar la visualización en pantalla partida, a la izquierda verás el código de Markdown y a la derecha cómo queda visualmente. Para ello haz click en el icono siguiente:

![Preview Retext](img/retext-preview.png)

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

Lo que queremos hacer ahora es empujar (push) nuestros cambios en local (en nuestro ordenador) a la rama maestra (master) que está en nuestro repositorio forkeado en github, para eso hacemos:

```shell
$ git push origin master
```

La propia línea de comandos te pedirá tu nombre de usuario y tu password en github.

Enhorabuena, acabas de actualizar tu repositorio en Github con los cambios que has realizado.


## 5. Tu primer Pull Request

Ahora vamos a realizar tu primer Pull Request o PR: pedir que incorporen tus mejoras en el original del que forkeaste, para que este manual quede actualizado con los cambios que has realizado en tu fork.


## 6. Trabajar en Ramas

Si en lugar de actualizar los cambios directamente en master queremos crear una rama, actualizar y sólo posteriormente fusionar o actualizar los cambios en la rama maestra (master), entonces, primero tenemos que crear una rama:

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


## 7. Actualizar tus repositorios locales

Si hay cambios en los servidores remotos y tienes que actualizar tu repositorio local.

```shell
$ git pull origin master
```



# Personas que están aprendido con este tutorial

* Xabier

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

