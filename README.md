# Python, Django y TDD
Documentar los pasos ejecutados para habilitar un sistema de pruebas mientras leo el libro [Python TDD Book] en 2da. edición.
![](https://images-na.ssl-images-amazon.com/images/I/51q3VZT%2BseL._SX379_BO1,204,203,200_.jpg)]

La razón de documentar es sencilla:
  - Repetir exactamente los pasos para habilitar, en 1 hora, una nueva Virtual Machine (VM) con Ubuntu Desktop 16.04 con Python 3.6 para desarrollar.
  - Instalar NVM y NodeJS
  - Instalar VSCode 
  - Generar llaves SSH para actualizar en GitHub y BitBucket.
  - Python 3.6
  - Geckodriver
  - Django
  - Selenium

## Cómo se supone que debo leer esto?
Los scripts mencionados en esta guía, deben ser ejecutados en un sistema Ubuntu Desktop 16.04 recién instalado. He detallado solo los pasos para instalar los programas, librerías y herramientas necesarias para trabajar con el libro. Debes saber cómo usar la terminal.

Tengo pensado utilizar la misma guía para indicar cómo habilitar mis sistema para programar en golang. Pendiente.

> Para continuar, asumo que ya tienes VirtualBox instalado. Te has descargado e instalado Linux Ubuntu Desktop 16.04. De lo contrario, no continues leyendo.

## Instalación

### Instalar git
```
$ sudo apt install git
$ git config --global user.email "juan.escutia@gmail.com"
$ git config --global user.name "Juan Escutia"
$ git config --global push.default simple
```

### Instalar Node

Conozco dos formas de instalar node. 
  - Usando NVM (mi preferida)
  - Usando apt-get

Puedo usar cualquier opción. La de apt-get recibe actualizaciones. Con nvm otra yo controlo cuando instalar, cual versión. Dejo las dos formas como referencia.

#### Instalar NodeJS 8.x usando apt-get
```
$ cd Downloads/
$ sudo apt install curl
$ curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
$ rm nodesource_setup.sh 
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install -y build-essential
```

#### Instalar NodeJS 8.x usando nvm
```
$ cd Downloads/
$ sudo apt install curl
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.9/install.sh | bash
$ nvm --version
$ exit       # close Terminal and open a new one
$ nvm --version
$ nvm ls-remote --lts
$ nvm install --lts    # this install latest LTS Node version
$ nvm list
$ node --version
$ npm --version
$ nvm --version
```

### Instalar Python 3.6

```
$ sudo apt-get update && sudo apt-get upgrade -y
$ exit
$ sudo add-apt-repository ppa:jonathonf/python-3.6
$ sudo apt-get update
$ sudo apt install python3.6
$ python --version
$ python3 --version
$ python3.6 --version
```

Para installar pip, debes consultar los pasos en https://pip.pypa.io/en/stable/installing/

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python3.6 get-pip.py
$ pip3 --version

$ sudo apt-get install build-essential libssl-dev libffi-dev python-dev
$ sudo apt-get install build-essential libssl-dev libffi-dev python3-dev
$ sudo apt-get install build-essential libssl-dev libffi-dev python3.6-dev
```

Para revisar la historia de comandos hasta ahora:
```
$ nano ~/.bash_history
$ cp ~/.bash_history ~/Downloads/bashHistory.txt
```

### Instalar virtualenvwrapper

Esta herramienta permite generar ambientes virtuales desde cualquier lugar, y siempre guardándolos en un solo lugar para fácil acceso. Es mejor a venv.

```
$ sudo apt-get update && sudo apt-get upgrade
$ sudo pip3 install virtualenvwrapper
$ mkdir ~/.virtualenvs
$ export WORKON_HOME=~/.virtualenvs
$ nano ~/.bashrc
```

En el archivo .bashrc, agregar las siguientes líneas al final:
```
VIRTUALENVWRAPPER_PYTHON='/usr/bin/python3.6' # This needs to be placed before the virtualenvwrapper command

source /usr/local/bin/virtualenvwrapper.sh
```

Ahora, grabar archivo y cerrar nano. Al cerrar la Terminal y abrir una nueva, ya puedo ejecutar los siguientes comandos de virtualenv:
```
$ lsvirtuaenv # lists all virtual environments created
$ mkvirtualenv virtualenv_name # Create virtualenv
$ workon virtualenv_name # Activate/switch to a virtualenv
$ deactivate virtualenv_name # Deactivate virtualenv
```

Para probar, creo el ambiente para continuar el curso:
```
$ mkvirtualenv superlists
(superlists) gerardo@humaya:~$
(superlists) gerardo@humaya:~$ deactivate
$ workon superlists
(superlists) gerardo@humaya:~$ python --version
(superlists) gerardo@humaya:~$ deactivate
```

#### Nota: Para Windows
La siguiente referencia es para Windows. Es posible usar el mkvirtualenv en Windows. Revisa:

  - [virtualenvwrapper - Instalación](http://virtualenvwrapper.readthedocs.io/en/latest/install.html) 
  - [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/)



### Instalar Visual Studio Code

Referencia: https://code.visualstudio.com/docs/setup/linux

```
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
$ sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
$ sudo apt-get update
$ sudo apt-get install code
```

### Crear llaves ssh

```
$ cd ~
$ mkdir .ssh
$ cd .ssh
$ ssh-keygen -t rsa -b 4096
$ sudo apt-get install xclip
$ xclip -sel clip < ~/.ssh/id_rsa.pub
```

#### Agregar llaves ssh a github o bitbucket

Debemos agregar las llaves a github y bitbucket para manejar mayor seguridad. Evitar utilizar password.

Como referencia, revisa la páginas de github:

  - [Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/) 
  - [Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
  - [Adding a new SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
  - [Set up GitHub push with SSH keys](https://gist.github.com/gpalazuelosg/f2f1f2e2b526a4e288759aea4cd6f9ca)
  - [How to generate ssh key for git]( https://www.w3docs.com/snippets/git/how-to-generate-ssh-key-for-git.html)
  - [Push to github without password using ssh-key](https://stackoverflow.com/questions/14762034/push-to-github-without-password-using-ssh-key)


### Instalar geckodriver
Encontrar la última versión del geckodriver para cualquier sistema operativo:
  - https://github.com/mozilla/geckodriver/releases

Al momento de actualizar esta guía, era _geckodriver-v0.20.1-linux64.tar.gz_

```
$ cd ~/Downloads/
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.20.1/geckodriver-v0.20.1-linux64.tar.gz
$ tar -xvzf geckodriver-v0.18.0-linux64.tar.gz
$ rm geckodriver-v0.18.0-linux64.tar.gz
$ chmod +x geckodriver
$ cp geckodriver /usr/local/bin/
$ sudo cp geckodriver /usr/local/bin/
$ geckodriver --version
```


### Clonar repositorio git

```
$ cd ~/Projects/
$ git clone git@github.com:gpalazuelosg/tddpython2ed-superlists.git
$ cd tddpython2ed-superlists/
```

Nota: si por alguna razón, al descargar el repositorio, no usaste *git@github.com* pero en su lugar usaste *https://github.com*, tendrás que ajustar algo para poder hacer ```git push``` sin pedirte contraseñas.

En particular revisa los cambios que se hacen a ```git remote``` en las ligas siguientes:

  - [Set up GitHub push with SSH keys](https://gist.github.com/gpalazuelosg/f2f1f2e2b526a4e288759aea4cd6f9ca)
  - [Push to github without password using ssh-key](https://stackoverflow.com/questions/14762034/push-to-github-without-password-using-ssh-key)

Básicamente esto es lo que hice:
```
$ git remote -v
origin	https://github.com/gpalazuelosg/tddpython2ed-superlists.git (fetch)
origin	https://github.com/gpalazuelosg/tddpython2ed-superlists.git (push)
$ git remote set-url origin git@github.com:gpalazuelosg/tddpython2ed-superlists.git

$ git remote -v
origin	git@github.com:gpalazuelosg/tddpython2ed-superlists.git (fetch)
origin	git@github.com:gpalazuelosg/tddpython2ed-superlists.git (push)
```


## La prueba final

Ahora, con el código actual del repositorio, probar que todo funcione (Python + VirtualEnv + Django + Geckodriver):

```
$ cd ~/Projects/
$ git clone git@github.com:gpalazuelosg/tddpython2ed-superlists.git
$ cd tddpython2ed-superlists/
$ workon superlists
$ pip3 install "django<1.12" "selenium<4"
$ pip freeze > requirements.txt
$ python functional_tests.py 
$ deactivate
```

Ahora puedes hacer hacer cambios en el repositorio y publicarlos.


> Dependiendo el código actual, debe mostrar que todas las pruebas fueron satisfactorias ó algún. En cualquier caso, estamos bien pues se logró reinstalar todas las librerías y se puede continuar con el libro.



## Instalar Golang

Referencias: 
  - https://golang.org/doc/install
  - https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79

```
$ cd ~/Downloads/
$ sudo curl -O https://dl.google.com/go/go1.10.1.linux-amd64.tar.gz
$ sudo tar -xvf go1.10.1.linux-amd64.tar.gz
$ sudo mv go /usr/local
```
En este punto, go ha sido descargado, descomprimido y movido al folder /usr/local. Ahora seguimos actualizando las rutas de go (paths).

En nano, abrir el siguiente archivo:

```
$ sudo nano ~/.profile
```

Hasta el final del archivo, agregar las siguientes líneas. Grabar archivo y salir de nano.

  - Actualizar la ruta de GOROOT si go está instalado en otro folder.
  - Actualizar la ruta de GOPATH si el folder de trabajo deseado es otro

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/Projects/gosandbox
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

```

Ahora refrescar el profile de mi usuario (para evitar cerrar la Terminal y volver a entrar):
```
$ source ~/.profile
```

### Probar instalación de go

Comprobar que go está instalado. Debería imprimir algo como se muestra debajo de la ejecución del comando.
```
$ go version
go version go1.10.1 linux/amd64
```

Crear un Workspace para trabajar con go; donde go construirá/compilará archivos --- nota que esto va de la mano de la instalación de go, ver el punto previo:
```
$ mkdir $HOME/Projects/gosandbox
```

Ahora apuntar go al nuevo folder sandbox creado para ello.
  - Solo hacer cuando quiera cambiar de folder para mis programas go!
```
$ export GOPATH=$HOME/Projects/22gosandbox22
```

"gosandbox" es el folder de pruebas. Dentro de el, generar otro folder "src", generar otro folder "helloworld" y generar un archivo con extension ".go".
```
$ cd $HOME/Projects/gosandbox
$ mkdir src/helloworld
$ cd src/helloworld
$ nano hello.go
```
Estando en nano editando hello.go, pegar el siguiente código:
```
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```
Compilar el programa y ejecutarlo:
```
$ cd ../..    # moverse a raiz de gosandbox
$ go install src/helloworld/hello.go
$ ./bin/hello
```

El programa imprime el mensaje hello world:
```
$ ./bin/hello 
hello, world
```

O corre (compila y ejecuta) en una sola línea:
```
$ go run src/helloworld/hello.go
```


## Generar Markdown
Para generar el markdown me apoye con [Dillinger.io].

También use:
  - [pandao.github.io/editor.md](https://pandao.github.io/editor.md/examples/full.html)
  - [stackedit.io](https://stackedit.io/app)


## To-Do:
  - Agrupar comandos detallando qué hace cada uno.

   [Python TDD Book]: <https://www.amazon.com/Test-Driven-Development-Python-Selenium-JavaScript/dp/1491958707/ref=sr_1_1/131-5649203-4499659?s=books&ie=UTF8&qid=1519889044&sr=1-1&refinements=p_27%3AHarry+Percival>
   [Dillinger.io]: <https://dillinger.io/>

## Errores

### Python y pip
Nota: 
(2018-05-11) Después de una actualización de sistema (al hacer apt-get update...), pip comenzó a tirar un error:
´´´
none@horizon:/usr/bin$ pip3 --version
Traceback (most recent call last):
  File "/usr/local/bin/pip3", line 7, in <module>
    from pip._internal import main
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/__init__.py", line 42, in <module>
    from pip._internal import cmdoptions
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/cmdoptions.py", line 16, in <module>
    from pip._internal.index import (
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/index.py", line 25, in <module>
    from pip._internal.download import HAS_TLS, is_url, path_to_url, url_to_path
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/download.py", line 35, in <module>
    from pip._internal.locations import write_delete_marker_file
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/locations.py", line 10, in <module>
    from distutils import sysconfig as distutils_sysconfig
ImportError: cannot import name 'sysconfig'
Error in sys.excepthook:
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/apport_python_hook.py", line 63, in apport_excepthook
    from apport.fileutils import likely_packaged, get_recent_crashes
  File "/usr/lib/python3/dist-packages/apport/__init__.py", line 5, in <module>
    from apport.report import Report
  File "/usr/lib/python3/dist-packages/apport/report.py", line 30, in <module>
    import apport.fileutils
  File "/usr/lib/python3/dist-packages/apport/fileutils.py", line 23, in <module>
    from apport.packaging_impl import impl as packaging
  File "/usr/lib/python3/dist-packages/apport/packaging_impl.py", line 23, in <module>
    import apt
  File "/usr/lib/python3/dist-packages/apt/__init__.py", line 23, in <module>
    import apt_pkg
ModuleNotFoundError: No module named 'apt_pkg'

Original exception was:
Traceback (most recent call last):
  File "/usr/local/bin/pip3", line 7, in <module>
    from pip._internal import main
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/__init__.py", line 42, in <module>
    from pip._internal import cmdoptions
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/cmdoptions.py", line 16, in <module>
    from pip._internal.index import (
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/index.py", line 25, in <module>
    from pip._internal.download import HAS_TLS, is_url, path_to_url, url_to_path
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/download.py", line 35, in <module>
    from pip._internal.locations import write_delete_marker_file
  File "/usr/local/lib/python3.6/dist-packages/pip/_internal/locations.py", line 10, in <module>
    from distutils import sysconfig as distutils_sysconfig
**ImportError: cannot import name 'sysconfig'**
none@horizon:/usr/bin$
´´´

El error se ve aquí: ImportError: cannot import name 'sysconfig'

En este link comentan lo siguiente: Está relacionado con instalaciones hechas con el ppa: ´´´ppa:jonathonf/python-3.6´´´

Para solucionarlo, ejecute el siguiente comando:
´´´sudo apt-get install python3-distutils´´´

Y mi pip comenzó a funcionar nuevamente.
