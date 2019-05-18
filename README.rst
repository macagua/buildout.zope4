==============
buildout.zope4
==============

Configuración de buildout para el servidor de aplicaciones Zope 4 con Python 3.5.

.. image:: https://travis-ci.org/macagua/buildout.zope4.svg?branch=master
   :target: https://travis-ci.org/macagua/buildout.zope4


Características
===============

- `Zope 4.0 <https://pypi.org/project/Zope/4.0/>`_, la ultima versión soporta Zope 4.

- `Zope 4.0b10 <https://pypi.org/project/Zope/4.0b10/>`_, la ultima versión de Zope 4, la cual soporta 
  el proyecto Plone en su versión 5.2.

- `Python 3.5.x <https://www.python.org/downloads/release/python-356/>`_.


Requerimientos
==============

Estos son los requerimientos mínimos de instalación: ::

  sudo apt-get update && sudo apt-get upgrade -y
  sudo apt install git build-essential zlib1g-dev python3-dev python3-pip python3-virtualenv


Descargar
=========

Para la descargar del proyecto Buildout, ejecute el siguiente comando: ::

  git clone https://github.com/macagua/buildout.zope4.git


Entorno virtual
===============

Se requiere crear y activar un entorno virtual Python para proyecto Buildout, ejecute los siguientes comando: ::

  virtualenv --python=/usr/bin/python3 venv
  source ./venv/bin/activate


Inicialización del proyecto
===========================

Para la inicialización del proyecto Buildout, ejecute el siguiente comando: ::

  pip install -r requirements.txt


Construcción del proyecto
=========================

Para la construcción del proyecto Buildout para obtener Zope 4.0, ejecute el siguiente comando: ::

  buildout

Para la construcción del proyecto Buildout para obtener Zope 4.0b10, ejecute el siguiente comando: ::

  ./bin/buildout -c 4.0b10.cfg


Crear instancia Zope 4
======================

Luego de terminar la construcción del proyecto Buildout, debe crear instancia Zope 4, ejecute el 
siguiente comando: ::

  ./bin/mkwsgiinstance -d $PWD/zopeinstance -u admin:admin --p ./venv/bin/python3

La ejecución crea un directorio ``zopeinstance`` con la siguiente estructura: ::

  zopeinstance/
  ├── etc/
  │   ├── site.zcml
  │   ├── zope.conf
  │   └── zope.ini
  ├── inituser
  └── var/
      ├── cache/
      │   └── README.txt
      ├── log/
      │   └── README.txt
      └── README.txt


Ejecutar servidor Zope 4
========================

Para ejecutar la instancia ``zopeinstance`` del servidor Zope 4, ejecute el siguiente comando: ::

  ./bin/runwsgi $PWD/zopeinstance/etc/zope.ini -n zope --server-name=main
  2019-05-18 08:33:02 INFO [chameleon.config:38][MainThread] directory cache: /home/zope/buildout.zope4/zopeinstance/var/cache.
  2019-05-18 08:33:02 INFO [Zope:45][MainThread] Ready to handle requests
  Starting server in PID 8275.
  Serving on http://localhost:8080

Como lo indica el ultimo mensaje de consola, el servidor Zope 4 puede ser consultado en la dirección http://localhost:8080 
entonces abra un navegador con esa dirección y le presentara la siguiente pantalla:

.. image:: https://github.com/macagua/buildout.zope4/raw/master/zope4_index_html.png
   :target: http://localhost:8080

Para acceder a la Zope Managment Interface - ZMI abra en su navegador la dirección http://localhost:8080/manage la cual le 
solicitara el nombre de usuario **admin** y contraseña **admin** y le presentara la siguiente pantalla: 

.. image:: https://github.com/macagua/buildout.zope4/raw/master/zope4_manage.png
   :target: http://localhost:8080/manage

Luego de la ejecución crean nuevos directorios y archivos como se presentan en la siguiente 
estructura: ::

  zopeinstance/
  ├── etc/
  │   ├── site.zcml
  │   ├── zope.conf
  │   └── zope.ini
  └── var/
      ├── cache/
      │   ├── 160ef7d206a5183fd315af509d689dba.py
      │   ├── __pycache__
      │   │   └── 160ef7d206a5183fd315af509d689dba.cpython-35.pyc
      │   └── README.txt
      ├── Data.fs
      ├── Data.fs.index
      ├── Data.fs.lock
      ├── Data.fs.tmp
      ├── log/
      │   ├── event.log
      │   ├── README.txt
      │   └── Z4.log
      ├── README.txt
      └── Z4.pid

Comandos disponibles
====================

./bin/addzope2user

  Permite agregar un nuevo usuario Zope, ejecutando el siguiente comando: ::

    ./bin/addzope2user <username> <password>

  Para más información consulte la ayuda incluida en el script con el siguiente comando ``./bin/addzope2user -h``.


./bin/mkwsgiinstance

  Permite crear una instancia WSGI de Zope. agregar un nuevo usuario Zope, ejecutando el siguiente comando: ::

    ./bin/mkwsgiinstance -d $PWD/zopeinstance -u admin:admin --python=$PWD/bin/zopepy

  Cuando se ejecuta sin argumentos, este script solicitará la información necesaria para crear una instancia de 
  inicio de Zope WSGI.

  Para más información consulte la ayuda incluida en el script con el siguiente comando ``./bin/mkwsgiinstance -h``.


./bin/mkzopeinstance

  Es una utilidad descontinuada en Zope 4.0, en remplazo use el script ``./bin/mkwsgiinstance``.


./bin/runwsgi

  Uso: runwsgi config_uri [var=valor]

  Es el script ejecutor del ZDaemon (servicio) Zope, para ejecutarlo ejecute el siguiente comando: ::

    ./bin/runwsgi $PWD/zopeinstance/etc/zope.ini -n zope --server-name=main

  Este comando sirve a una aplicación web que utiliza un archivo de configuración del paquete ``PasteDeploy`` 
  para el servidor y la aplicación. También puede incluir asignaciones de variables como ``'http_port=8080'`` y 
  luego usar ``%(http_port)s`` en sus archivos de configuración.

  Para más información consulte la ayuda incluida en el script con el siguiente comando ``./bin/runwsgi -h``.


./bin/zconsole

  Uso: zconsole [-h] {run,debug} zopeconf ...

  Es el script ejecutor de la consola Zope, este posee los siguientes argumentos posicionales:

    {run,debug}  modo de operación, run: ejecutar script; debug: consola interactiva
    zopeconf     ruta al archivo de configuración zope.conf
    scriptargs

  Para ejecutarlo en modo ``debug``, debe ejecute el siguiente comando: ::

    ./bin/zconsole debug $PWD/zopeinstance/etc/zope.conf
    Starting debugger (the name "app" is bound to the top-level Zope object)
    >>> 
    >>> app.__doc__
    'Top-level system object'
    >>> app.Control_Panel.__doc__
    'System management\n    '
    >>> app.acl_users.__doc__
    'Standard UserFolder object\n\n    A UserFolder holds User objects which contain information\n    about users including name, password domain, and roles.\n    UserFolders function chiefly to control access by authenticating\n    users and binding them to a collection of roles.'
    >>> app.index_html.__doc__
    'Zope wrapper for Page Template using TAL, TALES, and METAL'
    >>> app.temp_folder.__doc__
    'Folders are basic container objects that provide a standard\n    interface for object management. Folder objects also implement\n    a management interface and can have arbitrary properties.\n    '
    >>> app.virtual_hosting.__doc__
    'Provide a simple drop-in solution for virtual hosting.\n    '
    >>> exit()

  Para ejecutarlo en modo ``run``, debe ejecute el siguiente comando: ::

    ./bin/zconsole run $PWD/zopeinstance/etc/zope.conf

  Para más información consulte la ayuda incluida en el script con el siguiente comando ``./bin/zconsole -h``.


./bin/zpasswd

  Es una utilidad descontinuada en Zope 4.0.


Comandos extras recetas buildout
================================

./bin/instance


  Es el script que lleva por nombre de la sección ``[instance]`` buildout que construye automáticamente Zope 4, 
  eso quiere decir, controla la instancia Zope usando ZDaemon (Zope Daemon), como lo hace el script ``zopectl``. 

  Para instalarlo ejecute el siguiente comando: ::

    buildout install instance

  Luego de la ejecución crean nuevos directorios y archivos como se presentan en la siguiente estructura: ::

    parts/instance/
    ├── bin/
    │   ├── interpreter
    │   └── README.txt
    ├── etc/
    │   ├── site.zcml
    │   ├── wsgi.ini
    │   └── zope.conf
    ├── inituser
    └── var/
        └── README.txt

  Use: zopectl [opciones] [acción [argumentos]]

  Opciones:
    -h/--help -- imprimir el mensaje de uso y salir.

    -i/--interactive -- inicia un shell interactivo después de ejecutar los comandos
         acción [argumentos] -- ver más abajo.

  Las acciones son comandos como los siguientes:

    - "start" (inicia el servicio).

    - "stop" (detiene el servicio).

    - "status" (estado del servicio). 

  Si se especifica la opción ``-i`` o no se especifica ninguna acción en la línea 
  de comando, se inicia una acción de interpretación "shell" escrita interactivamente. 
  Utilice la acción "ayuda" para conocer las acciones disponibles.

  Para instalarlo ejecute el siguiente comando: ::

    ./bin/instance -i
    Program: ./venv/bin/python3 ./parts/instance/bin/interpreter ./eggs/Zope-4.0-py3.5.egg/Zope2/Startup/serve.py ./parts/instance/etc/wsgi.ini
    daemon manager not running
    instance> help

    Documented commands (type help <topic>):
    ========================================
    adduser  fg          kill       reopen_transcript  show    stop
    console  foreground  logreopen  restart            start   wait
    debug    help        logtail    run                status

    Miscellaneous help topics:
    ==========================
    startup_command

    Undocumented commands:
    ======================
    test

  Para el script ``instance status`` en modo **estado del servicio**, ejecute el siguiente comando: ::

    ./bin/instance status

  Para el script ``instance show`` en modo **mostrar variables de configuración del servicio**, ejecute el siguiente comando: ::

    ./bin/instance show

  Para el script ``instance fg`` en modo **fore ground**, ejecute el siguiente comando: ::

    ./bin/instance fg

  Para el script ``instance start`` en modo **inicia el servicio**, ejecute el siguiente comando: ::

    ./bin/instance start

  Para el script ``instance stop`` en modo **detiene el servicio**, ejecute el siguiente comando: ::

    ./bin/instance stop

  Para el script ``instance restart`` en modo **reinicia el servicio**, ejecute el siguiente comando: ::

    ./bin/instance restart

  Para el script ``instance run`` en modo **ejecutar script con argumentos en el servicio**, ejecute el siguiente comando: ::

    ./bin/instance run <script> [args]

  Para más información consulte la ayuda incluida en el script con el siguiente comando ``./bin/instance -h``.
  Adicionalmente consulte el articulo `Installing Zope with zc.buildout — Zope documentation 4.0 documentation <https://zope.readthedocs.io/en/latest/INSTALL.html#installing-zope-with-zc-buildout>`_.


./bin/zopepy

  Es el script que acceder a una consola interactiva de Python al contexto de la instalación de Zope 4, para 
  instalarlo ejecute el siguiente comando: ::

    buildout install zopepy

  Para el script ``zopepy`` ejecute el siguiente comando: ::

    ./bin/zopepy
    >>> import Zope2
    >>> Zope2.__doc__
    'Zope application package.'
    >>> exit()

  Este script puede ser usado tanto por el comando ``mkwsgiinstance`` para crear una instancia nueva de Zope, como hacer 
  introspección de Python al contexto de la instalación de Zope 4.
