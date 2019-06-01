# Python, Django y TDD - 2019
Documentar los pasos ejecutados para habilitar un sistema de pruebas mientras leo el libro [Python TDD Book] en 2da. edición.
![](https://images-na.ssl-images-amazon.com/images/I/51q3VZT%2BseL._SX379_BO1,204,203,200_.jpg)]

La razón de documentar es sencilla:
  - Repetir exactamente los pasos para habilitar, en 1 hora, una nueva Virtual Machine (VM) con Ubuntu Desktop 18.04.02 con Python 3.6 y Python 3.7 para desarrollar.
  - Instalar NVM y NodeJS
  - Instalar VSCode 
  - Generar llaves SSH para actualizar en GitHub y BitBucket.
  - Python 3.6 y Python 3.7
  - Geckodriver
  - Django
  - Selenium

## Cómo se supone que debo leer esto?
Los scripts mencionados en esta guía, deben ser ejecutados en un sistema Ubuntu Desktop 18.04.02 recién instalado. He detallado solo los pasos para instalar los programas, librerías y herramientas necesarias para trabajar con el libro. Debes saber cómo usar la terminal.

Tengo pensado utilizar la misma guía para indicar cómo habilitar mis sistema para programar en golang. Pendiente.

> Para continuar, asumo que ya tienes VirtualBox instalado. Te has descargado e instalado `Linux Ubuntu Desktop 18.04.02`. De lo contrario, no continues leyendo.

## Instalación

### Instalar git
```
$ sudo apt install git
$ git config --global user.email "doroteo.arango@gmail.com"
$ git config --global user.name "Pancho Villa"
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
$ curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
$ rm nodesource_setup.sh 
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install -y build-essential
```

#### Instalar NodeJS 8.x usando nvm
```
$ cd Downloads/
$ sudo apt install curl
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
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

### Instalar Python 3.6 y Python 3.7

En Ubuntu 18.04.02 ya esta instalado Python 3.6. Eso puede ser suficiente. Si quieres instalar Python 3.7, prosigue. 

```
$ sudo apt-get update && sudo apt-get upgrade -y
$ sudo apt install software-properties-common
$ exit
$ sudo add-apt-repository ppa:deadsnakes/ppa
>> presionar ENTER cuando pregunta *Press [ENTER] to continue or Ctrl-c to cancel adding it*. <<
$ sudo apt-get update
$ sudo apt install python3.7
$ python --version
$ python3 --version
$ python3.7 --version
```

Instalar pip3:

```
$ sudo apt-get install build-essential libssl-dev libffi-dev python3.6-dev
$ sudo apt-get install build-essential libssl-dev libffi-dev python3.7-dev
$ sudo apt install python3-pip
```

Para revisar la historia de comandos hasta ahora:
```
$ nano ~/.bash_history
$ cp ~/.bash_history ~/Downloads/bashHistory.txt
```

### Instalar venv

Actualmente, `venv` es la herramienta sugerida en la docu de Python.

```bash
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt install python3-venv
$ python3 -m venv my-project-env
$ source my-project-env/bin/activate
```

Si quiero probar que esto funciona:

```bash
$ pip3 install requests
$ nano testing.py
```

Escribir en archivo lo siguiente:
```python
import requests

r = requests.get('http://httpbin.org/get')  
print(r.headers) 
```

Close and save the file.

We can now run the script by typing:
```bash
$ python testing.py
Output:
{'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Date': 'Tue, 18 Sep 2018 16:50:03 GMT', 'Content-Type': 'application/json', 'Content-Length': '266', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true', 'Via': '1.1 vegur'}

$ deactivate
```



#### Nota: Para Windows
La siguiente referencia es para Windows. Es posible usar el mkvirtualenv en Windows. Revisa:

  - [virtualenvwrapper - Instalación](http://virtualenvwrapper.readthedocs.io/en/latest/install.html) 
  - [virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/)



### Instalar Visual Studio Code

Referencia: https://code.visualstudio.com/docs/setup/linux

```bash
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg

$ sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/

$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

$ sudo apt-get install apt-transport-https
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
$ xclip -sel clip < ~/.ssh/matehuala_id_rsa.pub
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



***
_esta guia necesita ser actualizado despues de esta linea_
***


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

## Instalar Docker

### Preparar paquetes apt en Ubuntu

Referencias: 
  - https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository
  - https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

```
$ cd ~/Downloads/
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.

```
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or test repositories as well. To add the edge or test repository, add the word edge or test (or both) after the word stable in the commands below.

    Note: The lsb_release -cs sub-command below returns the name of your Ubuntu distribution, such as xenial. Sometimes, in a distribution like Linux Mint, you might need to change $(lsb_release -cs) to your parent Ubuntu distribution. For example, if you are using Linux Mint Rafaela, you could use trusty.

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### Ahora si, instalar Docker CE
```
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

o instalar una version en especifico:

List the versions available in your repo:
```
$ apt-cache madison docker-ce
```

Install a specific version by its fully qualified package name, which is the package name (docker-ce) plus the version string (2nd column) up to the first hyphen, separated by a an equals sign (=), for example, docker-ce=18.03.0.ce.
```
$ sudo apt-get install docker-ce=<VERSION>
```

Verify that Docker CE is installed correctly by running the hello-world image.
```
$ sudo docker run hello-world
```

### Ejecutar comando docker sin utilizar sudo (opcional)

By default, running the docker command requires root privileges — that is, you have to prefix the command with sudo. It can also be run by a user in the docker group, which is automatically created during the installation of Docker. If you attempt to run the docker command without prefixing it with sudo or without being in the docker group, you'll get an output like this:
```
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
```

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:

```
$ sudo usermod -aG docker ${USER}
```

To apply the new group membership, you can log out of the server and back in, or you can type the following:
```
$ su - ${USER}
```

You will be prompted to enter your user's password to continue. Afterwards, you can confirm that your user is now added to the docker group by typing:
```
$ id -nG

sammy sudo docker
```

### Comandos docker

```
$ docker run hello-world
$ docker search ubuntu
$ docker pull ubuntu
$ docker run ubuntu
$ docker images
```

### Ejecutando un container docker

The combination of the -i and -t switches gives you interactive shell access into the container. We can also run linux commands while on the container sheel.

```
$ docker run -it ubuntu
root@d9b100f2f636:/#

$ apt-get update
$ apt-get install -y nodejs
```

### Committing Changes in a Container to a Docker Image

When you start up a Docker image, you can create, modify, and delete files just like you can with a virtual machine. The changes that you make will only apply to that container. You can start and stop it, but once you destroy it with the ```docker rm``` command, the changes will be lost for good.

This section shows you how to save the state of a container as a new Docker image.

After installing nodejs inside the Ubuntu container, you now have a container running off an image, but the container is different from the image you used to create it.

To save the state of the container as a new image, first exit from it:
```
exit
```

Then commit the changes to a new Docker image instance using the following command. The -m switch is for the commit message that helps you and others know what changes you made, while -a is used to specify the author. The container ID is the one you noted earlier in the tutorial when you started the interactive docker session. Unless you created additional repositories on Docker Hub, the repository is usually your Docker Hub username:

```
docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name
docker commit -m "added node.js" -a "Sunday Ogwu-Chinuwa" d9b100f2f636 finid/ubuntu-nodejs
```

> Note: When you commit an image, the new image is saved locally, that is, on your computer. Later in this tutorial, you'll learn how to push an image to a Docker registry like Docker Hub so that it may be assessed and used by you and others.

After that operation has completed, listing the Docker images now on your computer should show the new image, as well as the old one that it was derived from:

```
$ docker images

finid/ubuntu-nodejs       latest              62359544c9ba        50 seconds ago      206.6 MB
```

In the above example, ubuntu-nodejs is the new image, which was derived from the existing ubuntu image from Docker Hub. The size difference reflects the changes that were made. And in this example, the change was that NodeJS was installed. So next time you need to run a container using Ubuntu with NodeJS pre-installed, you can just use the new image. Images may also be built from what's called a Dockerfile. But that's a very involved process that's well outside the scope of this article.

### Listing Docker Containers
```
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f7c79cc556dd        ubuntu              "/bin/bash"         3 hours ago         Up 3 hours                              silly_spence
```

To view all containers — active and inactive, pass it the -a switch:
```
$ docker ps -a
```

To view the latest container you created, pass it the -l switch:
```
$ docker ps -l
```

Stopping a running or active container is as simple as typing:
```
$ docker stop container-id
```

### Pushing Docker Images to a Docker Repository
The next logical step after creating a new image from an existing image is to share it with a select few of your friends, the whole world on Docker Hub, or other Docker registry that you have access to. To push an image to Docker Hub or any other Docker registry, you must have an account there.

This section shows you how to push a Docker image to Docker Hub. To learn how to create your own private Docker registry, check out [How To Set Up a Private Docker Registry on Ubuntu 14.04](http://virtualenvwrapper.readthedocs.io/en/latest/install.html).


To create an account on [Docker Hub](https://hub.docker.com/), register at Docker Hub. Afterwards, to push your image, first log into Docker Hub. You'll be prompted to authenticate:
```
$ docker login -u docker-registry-username
```

If you specified the correct password, authentication should succeed. Then you may push your own image using:
```
$ docker push docker-registry-username/docker-image-name

The push refers to a repository [docker.io/finid/ubuntu-nodejs]
...
```

After pushing an image to a registry, it should be listed on your account's dashboard


## Generar Markdown
Para generar el markdown me apoye con [Dillinger.io].

También use:
  - [pandao.github.io/editor.md](https://pandao.github.io/editor.md/examples/full.html)
  - [stackedit.io](https://stackedit.io/app)
  - [markdowntutorial.com](https://www.markdowntutorial.com/)


## To-Do:
  - Agrupar comandos detallando qué hace cada uno.

   [Python TDD Book]: <https://www.amazon.com/Test-Driven-Development-Python-Selenium-JavaScript/dp/1491958707/ref=sr_1_1/131-5649203-4499659?s=books&ie=UTF8&qid=1519889044&sr=1-1&refinements=p_27%3AHarry+Percival>
   [Dillinger.io]: <https://dillinger.io/>

## Errores

### Python y pip
Nota: 
(2018-05-11) Después de una actualización de sistema (al hacer apt-get update...), pip comenzó a tirar un error:
```
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
```

El error se ve aquí: ImportError: cannot import name 'sysconfig'

En este [link](https://github.com/pypa/pip/issues/5367#issuecomment-386864941) comentan lo siguiente: Está relacionado con instalaciones hechas con el ppa: 
```ppa:jonathonf/python-3.6```

Para solucionarlo, ejecute el siguiente comando:
```
$ sudo apt-get install python3-distutils
```

Y mi pip comenzó a funcionar nuevamente.
