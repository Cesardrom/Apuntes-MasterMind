## Apuntes de MasterMind: GIT

#### César Domínguez Romero

#### ¿Que es Git y para que sirve?

 1. Es un sistema de control de versiones que inicialmente un sistema dedicado a creacion de versiones de programas software
 2. Es un sistema automatizado que te permite tener un log con diferentes versiones de los trabajos o proyectos que tengas permitiendo asi tener diferentes estados de tu proyecto en caso de fallos etc.
 3. En git hay tres areas principales: 
    - Working Tree o directorio de trabajo: Es en donde se encuentra el trabajo en si con carpetas archivos etc.
    - Stagin Area: Es en donde se encuentran los cambios que hayamos realizado en el Working Tree que esten pendientes a ser confirmados.
    - Commits : Es el area donde se encuentran las versiones ya confirmadas del proyecto a las que podremos acceder en cualquier momento.

#### Instalacion de Git:

    ##### En Windows:
        1. Buscamos su pagina en google la pagina web de git y pinchamos donde pone descargar para Windows ç
        2. Ejecutamos el .exe que nos dan y le damos permiso de admin para la instalacion.
        3. Le damos a Next y elegimos donde queremos que se descargue.
        4. En la pagina de select Components debemos de verificar si esta marcado el Windows Explorer Integration y sus anidaciones y a partir de aqui todo a next.
        5. Abrir un gestor de archivos y dar click derecho y confirmar que te sale en el desplegable dos opciones de Git
        6. Abrimos el navegador y buscamos nerd fonts y descargas la fuente que mas te guste y extraemos su contenido seleciionas todos los archivos fuentes y pones instalar en todos los usuarios.
        7. Click derecho en la terminal de bash y le das a options text y seleccionas el descargado.


    ##### En Linux:

        Sencillo sudo apt upgrade, sudo apt update y sudo apt install git

#### Creacion del Proyecto

    1. Creamos una carpeta y abrimos dicha carpeta con visual studio code
    2. Abrimos un termial con Ctrl+Ñ o sino Ctrl+Shift+P y pones create new terminal y abrimos un git bash.

#### Pull request.
    El pull request es una peticion para mezclar las ramas que elijas y se hace desde github en la seccion con el mismo nombre teniendo que crear la peticion y haciendo un merge de las ramas que hayas elegido. Esto tambien se puede hacer el local, recordar siempre que en local despues de hacer esta accion hacer un pull.

#### Forks.
    Los Forks son utilizados para trabajar en proyectos de "Open Source". Los Forks son como "copias" del repositorio principal del Open Source en el cual despues haremos un merge de nuestro Fork a el repositorio de Open Source.

#### Gitignore
    Sirve para ignorar archivos y carpetas que no quieres o debes subir a tu repositorio a Github

#### Comandos de Git

    - git init: Crear un repositorio desde termina.
    - git status: Ver el estado del repositorio. 
    - git add: Pasar archivos a el Stangin Area
    - git commit -m "mensaje": Pasar los archivos del Stangin Area al Commit con un mensaje explicativo
    - git config --global user.email "email": Poner el email de github al que se subira el repositorio
    - git config --global user.name "username": Poner el username de github al que se subira el repositorio
    - git log: Ver los commits del repositorio con toda su informacion, entre ellos el hash.
    - git checkout iniciales del hash del commit: Retroceder a otras versiones del proyecto pero en modo ditach head.
    - git checkout nombre de la rama: Te posicionas en el ultimo commit de la rama indicada.
    - git reset hash: Se eliminan los commits que se hayan echo despues del que elegimos el hash manteniendo los cambios en el Working Tree.
    - git reset --hard hash: Se eliminan los commits que se hayan echo despues del que elegimos el hash sin mantener los cambios en el Working Tree.
    - git restore: Restauras los archivos que se hayn subido a commits pero que hayas borrado en el Workin Tree.
    - git remote add  origin link: Conectar el local con el servidor de github con SSH
    - git remote -v: ver todos los repositorios remotos a los que estamos conectados
    - git push -u rama: Subir el ultimo commit realizado a la rama que hayas especificado
    - git clone url o ssh: Bajar un repositorio a local.
    - rm -rf carpeta repo: Borrar el repositorio entero de git del local.
    - git pull :Bajamos los cambios realizados en el repositorio
    - git checkout -b nombre rama: Crear una rama y cambiar a ella de manera automatica en local. Se pueden crear ramas dentro de ramas.
    - git branch: Visualizar todas las ramas que hay en el proyecto.
    - git commit -a -m: Es como hacer el git add . y el commit en la misma linea.
    - **git push -u origin nombre de la rama: Creamos una rama en el repo de github y subimos el ultimo commit realizado en ella**
    - git merge nombre de la rama: Se hace un pull request y merge en local de la rama en la que te encuentras situado con la que escribes en el comando.
    - git log --graph: Muestra un "grafico" donde se puede ver la actividad de las ramas en los commits.
    - git pull rama en la que estes rama remota: Bajar los cambios de una rama que no a sido combinada con la rama en la que estas. Tambien es una forma en la que resolver los conflictos que se puedan causar en ramas.
    - git commit: Es un comando para crear un commit pero a traves del editor de texto llamado vim donde se puede añadir mas informacion al commit.
