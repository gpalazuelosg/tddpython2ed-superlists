# Python, Django, TDD y extras

### Actualizado 2019-11
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
Los scripts mencionados en esta guía, deben ser ejecutados en un sistema Ubuntu Desktop 18.04.xy recién instalado. He detallado solo los pasos para instalar los programas, librerías y herramientas necesarias para trabajar con el libro. Asumo que sabes como usar la terminal.


> Para continuar, asumo que ya tienes VirtualBox instalado. Te has descargado, instalado y actualizado `Linux Ubuntu Desktop 18.04.03`. De lo contrario, no continues leyendo.


### Configurar VirtualBox VM


#### Shared Folder
Para acceder a los archivos del sistema host (en donde está instalado VirtualBox), debes crear un shared folder en VirtualBox para que el sistema host comparta archivos hacia la VM.


#### Instalar Guest Additions CD
Pues eso, instalarlo en el Ubuntu.


#### Asignar nuevo grupo a usuario de VM
Después de tener esa configuración, si desde la VM intentas acceder a los archivos del sistema host, normalmente dice "sf_<nombre_folder_compartido>", obtendremos error de acceso denegado.

Falta correr algo en la VM para tener acceso:

```bash
$ sudo adduser [your-user-name] vboxsf
```

#### Agregar nuevo método de entrada de teclado
En Ubuntu, busca por Region & Language, agrega nuevo Input Source para Spanish.

Después de reiniciar la VM y ya tendremos acceso a las carpetas compartidas e idioma nuevo de teclados.


### Instalaciones

#### Instalar git
```
$ sudo apt install git -y
$ git config --global user.email "doroteo.arango@gmail.com"
$ git config --global user.name "Pancho Villa"
$ git config --global push.default simple
```

#### Instalar Node

Conozco dos formas de instalar node. 
  - Usando NVM (mi preferida)
  - Usando apt-get

Puedo usar cualquier opción. La de apt-get recibe actualizaciones. Con nvm otra yo controlo cuando instalar, cual versión. Dejo las dos formas como referencia.

##### Instalar NodeJS 8.x usando nvm (preferida)
```bash
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


##### Instalar NodeJS 8.x usando apt-get
```bash
$ cd Downloads/
$ sudo apt install curl
$ curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
$ rm nodesource_setup.sh 
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install -y build-essential
```


#### Instalar Visual Studio Code

Referencia: https://code.visualstudio.com/docs/setup/linux

```bash
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg

$ sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/

$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

$ sudo apt-get install apt-transport-https -y
$ sudo apt-get update
$ sudo apt-get install code -y
```

#### Crear llaves ssh

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



#### Instalar LinuxBrew

En MAC existe Brew (Homebrew), una administrador de paquetes similar a pip (python) y npm (node). Instalar, desinstalar y actualizar paquetes es una tarea muy simple.

Ahora también existe en Linux, y se supone que funciona en WSL de Windows.

Con LinuxBrew, la intención es instalar Composer, PHP y Python3.8. Seguro hay más que descubriré.

En la página de [LinuxBrew](https://docs.brew.sh/Homebrew-on-Linux) esta toda la información.

Brew ó HomeBrew ó LinuxBrew es lo mismo.

Ahora ejecutar los siguientes comandos:

```bash
# esto intentará instalar todo Brew, no sin antes instalar/actualizar Ruby
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"

# instalar Homebrew dependencias (tarda varios minutos):
$ sudo apt-get install build-essential -y

# Configurar Homebrew en mi ~/.profile de bash:
$ echo 'eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >>~/.profile

# Agregar Brew al path de mi sesión terminal actual
$ eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)

# Instalar por recomendación la librería gcc <-- esta simplicidad es la belleza de Brew!
$ brew install gcc
```

#### Instalar Php 7.3

> esto tardará varios minutos

```bash
$ brew install php@7.3

```

Esto, no sé porque, pero ha instalado también python 3.7.5, lo cual no está mal.

Puede leerse en el sumatorio al finalizar instalar Php:
```bash
==> python
Python has been installed as
  /home/linuxbrew/.linuxbrew/bin/python3

$ python3 --version
Python 3.7.5
```


#### Instalar Composer

> Antes de ejecutar esto, asegúrate que Php esta instalado.

```bash
# tarda nada en ejecutarse este comando
$ brew install composer
==> Downloading https://getcomposer.org/download/1.9.1/composer.phar
######################################################################## 100.0%
🍺  /home/linuxbrew/.linuxbrew/Cellar/composer/1.9.1: 3 files, 1.9MB, built in 4 seconds


# ahorrate dolores de cabeza y reinicia de una vez!
$ sudo reboot
```

#### Instalar Laravel

Una vez que reiniciaste, después de instalar brew y composer, ahora intenta el Laravel installer.

```bash
$ composer global require "laravel/installer"

# entre otras cosas, el sumatorio nos dice esto:
Changed current directory to /home/gerardo/.config/composer
```

Si cierras terminal actual y abres nueva, el comando laravel no es reconocido. Lo que dicen los foros es que debemos agregar lo siguiente a nuestro ~/.profile:

```bash
export PATH=~/.composer/vendor/bin:$PATH
```

Pero como nosotros instalamos composer con Brew, la ruta es diferente.
Para encontrar el folder de interes, nos basaremos en el path que antes nos arrojó la instalación de composer:
```/home/gerardo/.config/composer```

Si vamos dicho folder esto encontraremos:
```bash
$ which composer
/home/linuxbrew/.linuxbrew/bin/composer
$ cd /home/gerardo/.config/composer
$ ls
composer.json  composer.lock  vendor
$ cd vendor
$ cd bin
$ ls
laravel
$ pwd
/home/gerardo/.config/composer/vendor/bin
```

Entonces, la ruta de interes es:
```/home/gerardo/.config/composer/vendor/bin```

Ahroa, hacer lo siguiente:
```bash
$ nano ~/.profile

# hasta el final del archivo, agregar una linea que sea como sigue:
export PATH=/home/gerardo/.config/composer/vendor/bin:$PATH

# ahorrate dolores de cabeza y reinicia de una vez!
$ sudo reboot
```

Brew, Composer, Laravel installer y Python 3.7.5 estarán disponibles la próxima vez.




#### Instalar Python 3.8

En caso de terquedad ó porque no tienes ninguna versión actual de Python, lo siguiente ayuda a instalar Python 3.8.

> Al instalar Php con Brew, se instaló Python 3.7.5. Eso ya es suficiente. Aún asi, los siguientes pasos son válidos para instala Python 3.8.

En Ubuntu 18.04.03 ya esta instalado Python 3.6. Eso puede ser suficiente. Pero yo quiero instalar Python 3.8. 

```bash
$ sudo apt-get update && sudo apt-get upgrade -y
$ sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
$ cd /usr/src
$ sudo wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
$ sudo tar xzf Python-3.8.0.tgz
$ cd Python-3.8.0
$ sudo ./configure --enable-optimizations
$ sudo make altinstall      # this step takes several seconds


$ python --version
$ python3 --version
$ python3.8 --version
```

```make altinstall``` is used to prevent replacing the default python binary file /usr/bin/python.


#### Instalar pip3

> Primero válida que no tengas pip3 instalado, ejecutando ```pip3 --version``` y procede solo si no lo tienes instalado.

```bash
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py
```

#### Instalar pipenv
```bash
$ pip3 install pipenv
```

#### Para revisar la historia de comandos hasta ahora:
```
$ nano ~/.bash_history
$ cp ~/.bash_history ~/Downloads/bashHistory.txt
```


#### Instalar venv

Actualmente, `venv` es la herramienta sugerida en la docu de Python.

```bash
$ sudo apt-get update && sudo apt-get upgrade -y
$ sudo apt install python3-venv

# para probar
$ python3 -m venv my-project-env
$ source my-project-env/bin/activate
```

Si quiero probar que esto funciona (opcional):

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



#### Instalar geckodriver
Encontrar la última versión del geckodriver para cualquier sistema operativo:
  - https://github.com/mozilla/geckodriver/releases

Al momento de actualizar esta guía, era _geckodriver-v0.26.0-linux64.tar.gz_

```
$ cd ~/Downloads/
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.26.0/geckodriver-v0.26.0-linux64.tar.gz
$ tar -xvzf geckodriver-v0.26.0-linux64.tar.gz
$ rm geckodriver-v0.26.0-linux64.tar.gz
$ chmod +x geckodriver
$ sudo cp geckodriver /usr/local/bin/
$ geckodriver --version
```


#### Instalar Golang

Referencias: 
  - https://golang.org/doc/install
  - https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79

```
$ cd ~/Downloads/
$ sudo curl -O https://dl.google.com/go/go1.13.4.linux-amd64.tar.gz
$ sudo tar -xvf go1.13.4.linux-amd64.tar.gz
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
export GOPATH=$HOME/go
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

```

Ahora refrescar el profile de mi usuario (para evitar cerrar la Terminal y volver a entrar):
```
$ source ~/.profile
```

##### Probar instalación de go

Comprobar que go está instalado. Debería imprimir algo como se muestra debajo de la ejecución del comando.
```
$ go version
go version go1.13.4 linux/amd64
```

Crear un Workspace para trabajar con go; donde go construirá/compilará archivos --- nota que esto va de la mano de la instalación de go, ver el punto previo:
```bash
$ mkdir $HOME/go
```

Ahora apuntar go al nuevo folder sandbox creado para ello.
  - Solo hacer cuando quiera cambiar de folder para mis programas go!
```
$ export GOPATH=$HOME/Projects/22gosandbox22
```

Dentro de $HOME/go, generar otros folders "src", "bin" y "pkg" que son los usados en los libros. Dentro de "src", generar otro folder "helloworld" y generar un archivo con extension ".go".
```bash
$ cd $HOME/go
$ mkdir src bin pkg
$ mkdir src/helloworld
$ cd src/helloworld
$ nano hello.go
```
Estando en nano editando hello.go, pegar el siguiente código:
```go
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```
Compilar el programa y ejecutarlo:
```bash
$ cd ../..    # moverse a raiz de gosandbox
$ go install src/helloworld/hello.go
$ ./bin/hello
```

El programa imprime el mensaje hello world:
```bash
$ ./bin/hello 
hello, world
```

O corre (compila y ejecuta) en una sola línea:
```bash
$ go run src/helloworld/hello.go
```


#### Clonar repositorio git

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


### La prueba final

Ahora, con el código actual del repositorio, probar que todo funcione (Python + VirtualEnv + Django + Geckodriver):

_lo siguiente es si YA tengo repositorio creado y requirements.txt tiene contenido_

Solo asegurate que ```requirements.txt``` tenga: `"django<1.12" "selenium<4"`

```bash
$ cd ~/Projects/
$ git clone git@github.com:gpalazuelosg/tddpython2ed-superlists.git
$ cd tddpython2ed-superlists/
$ python3 -m venv venv
$ source venv/bin/activate
$ pip3 install -r requirements.txt
$ python functional_tests.py 
$ deactivate
```


_lo siguiente es si NO tengo repositorio creado_

```bash
$ cd ~/Projects/
$ git clone git@github.com:gpalazuelosg/tddpython2ed-superlists.git
$ cd tddpython2ed-superlists/
$ python3 -m venv venv
$ source venv/bin/activate
$ pip3 install "django<1.12" "selenium<4"
$ pip freeze > requirements.txt
$ python functional_tests.py 
$ deactivate
```


Ahora puedes hacer hacer cambios en el repositorio y publicarlos.


> Dependiendo el código actual, debe mostrar que todas las pruebas fueron satisfactorias ó algún. En cualquier caso, estamos bien pues se logró reinstalar todas las librerías y se puede continuar con el libro.





#### Instalar Docker

##### Preparar paquetes apt en Ubuntu

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
    gnupg-agent \
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

##### Ahora si, instalar Docker CE
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

o instalar una version en especifico (esto no me interesa mucho):

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

##### Ejecutar comando docker sin utilizar sudo (opcional)

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

##### Comandos docker

```
$ docker run hello-world
$ docker search ubuntu
$ docker pull ubuntu
$ docker run ubuntu
$ docker images
```

##### Ejecutando un container docker

The combination of the -i and -t switches gives you interactive shell access into the container. We can also run linux commands while on the container sheel.

```
$ docker run -it ubuntu
root@d9b100f2f636:/#

$ apt-get update
$ apt-get install -y nodejs
```

##### Committing Changes in a Container to a Docker Image

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

##### Listing Docker Containers
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

##### Pushing Docker Images to a Docker Repository
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



#### Instalar Laravel/Valet

ref.
https://vander.host/knowledgebase/laravel/install-guide-for-laravel-valet/

https://cpriego.github.io/valet-linux/requirements
https://laravel-news.com/valet-for-ubuntu-linux


```bash
$ composer global require cpriego/valet-linux
$ sudo apt-get install network-manager libnss3-tools jq xsel

$ sudo apt-get install -y php7.2-cli php7.2-common php7.2-curl php7.2-json php7.2-mbstring php7.2-opcache php7.2-readline php7.2-xml php7.2-zip
$ sudo apt install php-pear php7.2-dev libmcrypt-dev gcc make autoconf libc-dev pkg-config

$ sudo apt install php-dev libmcrypt-dev php-pear
$ sudo apt-get -y install gcc make autoconf libc-dev pkg-config
$ sudo apt-get -y install libmcrypt-dev

$ composer global require laravel/valet # no use esta en linux

$ composer global require cpriego/valet-linux
$ valet install
```


#### Instalar Java

Evitaremos instalar las versiones de Oracle debido al tema de licencias.
Instaremos Java 8 de OpenJDK.
E instalaremos Java 11 de Amazon Corretto JDK 11.


```bash
$ sudo apt install -y wget
$ sudo apt install -y openjdk-8-jdk
```

Then verify the java version on the machine:
```bash
$ java -version
openjdk version "1.8.0_222"
OpenJDK Runtime Environment (build 1.8.0_222-8u222-b10-1ubuntu1~18.04.1-b10)
OpenJDK 64-Bit Server VM (build 25.222-b10, mixed mode)
```

Para Amazon Corretto JDK 11, navegar a la siguiente webpage:
https://aws.amazon.com/corretto/

Para instalar, las instrucciones son:
https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/generic-linux-install.html

y
https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html

Descargar el instalador para Amazon Corretto JDK 11, de 64 bits. Actualmente el archivo es java-11-amazon-corretto-jdk_11.0.5.10-1_amd64.deb

https://d3pxv6yz143wms.cloudfront.net/11.0.5.10.1/java-11-amazon-corretto-jdk_11.0.5.10-1_amd64.deb

```bash
$ sudo apt-get update && sudo apt-get install java-common
$ sudo dpkg --install ~/Downloads/java-11-amazon-corretto-jdk_11.0.5.10-1_amd64.deb
$ java -version
openjdk version "11.0.5" 2019-10-15 LTS
OpenJDK Runtime Environment Corretto-11.0.5.10.1 (build 11.0.5+10-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.5.10.1 (build 11.0.5+10-LTS, mixed mode)
```

Si acaso no veo la versión de Amazon Corretto como la default:
```bash
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac

$ rm ~/Downloads/java-11-amazon-corretto-jdk_11.0.5.10-1_amd64.deb
```



Si quisiera desinstalar Amazon Corretto:
```bash
$ sudo dpkg --remove java-11-amazon-corretto-jdk
```


(Existe una opción para JDK 8 de Amazon Corretto, pero intento documentar diferentes pasos).



## Instalar Eclipse
ref. 
https://www.eclipse.org/downloads/packages/

https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/how-to-install-eclipse-ide-on-ubuntu-18-04-lts.html
https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-18-04-ubuntu-16-04.html
https://linuxize.com/post/how-to-install-the-latest-eclipse-ide-on-ubuntu-18-04/


https://php.tutorials24x7.com/blog/how-to-install-eclipse-for-php-on-ubuntu
https://php.tutorials24x7.com/blog/how-to-install-eclipse-for-php-on-ubuntu-using-pdt



Download and extract Eclipse package to your desired directory (Ex. /usr/).

Then, symlink the eclipse executable to /usr/bin path so that all users on your machine can able to use Eclipse IDE.

```bash
$ cd ~/Downloads/
$ wget http://eclipse.mirror.rafal.ca/technology/epp/downloads/release/2019-09/R/eclipse-jee-2019-09-R-linux-gtk-x86_64.tar.gz

$ sudo tar -zxvf eclipse-jee-2019-09-R-linux-gtk-x86_64.tar.gz -C /usr/
$ sudo ln -s /usr/eclipse/eclipse /usr/bin/eclipse
```

Create Eclipse Launcher Icon.
Sometimes you may want to have Eclipse launcher icon in GNOME or Dash just like in the Start menu of Windows.

```bash
$ sudo nano /usr/share/applications/eclipse.desktop
```

Use the following information in the above file.
```bash
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse IDE
Comment=Eclipse IDE
Exec=/usr/bin/eclipse
Icon=/usr/eclipse/icon.xpm
Terminal=false
Type=Application
StartupNotify=false
```

Start Eclipse IDE
```bash
$ eclipse
```




#### Instalar IntelliJ IDEA Community Edition

En caso de que quiera instalar Intellij IDEA, la versión gratuita.

```bash
$ sudo snap install intellij-idea-community --classic
```


#### Instalar mysql (MariaDB) en Ubuntu 18.04

 > Otras referencias:
 > * [(itsfoss.com) How to Install MySQL in Ubuntu Linux](https://itsfoss.com/install-mysql-ubuntu/)
 > * [(itsfoss.com) How to Install and Configure PostgreSQL on Ubuntu](https://itsfoss.com/install-postgresql-ubuntu/)
 > * [(linuxize.com) How to Install PostgreSQL on Ubuntu 18.04](https://linuxize.com/post/how-to-install-postgresql-on-ubuntu-18-04/)


Instalar mariadb:
```bash
$ sudo apt update
$ sudo apt install mariadb-server mariadb-client
$ mysql --version
mysql  Ver 15.1 Distrib 10.1.41-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```


La instalación de MariaDB registra un symlink "mysql" y además inicia el servicio.

Verificar instalacion de MariaDB
```bash
$ sudo systemctl status mariadb
```

Deberia de leer algo como:
```Active: active (running) since Wed 2019-11-13 22:14:06 MST; 5min ago```


Asegurar la instalación de MariaDB:
```bash
$ sudo mysql_secure_installation
```

Para conectar desde la shell a MariaDB:
```bash
# https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost
# https://www.percona.com/blog/2016/03/16/change-user-password-in-mysql-5-7-with-plugin-auth_socket/
# https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost
# https://www.zeppelinux.es/corregir-error-1698-28000-access-denied-for-user-rootlocalhost-en-mariadb-mysql/

# esto no funciona porque el usuario root utiliza autenticacion de Unix Sockets
$ mysql -u root -p

# para no modificar el usuario root, lo mejor es crear un usuario mysql con mi nombre de usuario de sesion:
# dentro de sesion en mysql
mysql> USE mysql;
mysql> CREATE USER 'rufinus'@'localhost' IDENTIFIED BY '';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'rufinus'@'localhost';

# en la siguiente linea, puedo usar 
# auth_socket = utilizar mi usuario y clave de sesion mysql
# mysql_native_password = para poner una clave al usuario mysql creado
# voy por lo segundo
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='rufinus';
mysql> FLUSH PRIVILEGES;

# no jalo
mysql> ALTER USER 'rufinus'@'localhost' IDENTIFIED WITH mysql_native_password BY 'sancho-panza';

# si jalo
mysql> MariaDB [mysql]> update mysql.user set authentication_string=password('sancho-panza') where user='rufinus' and Host ='localhost';

mysql> flush privileges;

mysql> \q # salir

$ sudo service mariadb restart

# despues intentar loguearme con usuario gerardo y una contrase;a
$ mysql -u rufinus -p
```



Stop, Start, status y restart de MariaDB:
```bash
$ sudo systemctl stop mariadb.service      # To Stop MariaDB service 
$ sudo systemctl start mariadb.service     # To Start MariaDB service 
$ sudo systemctl status mariadb.service    # To Check MariaDB service status 
$ sudo systemctl restart mariadb.service   # To Stop then Start MariaDB service 
```




#### Instalar PostgreSQL en Ubuntu 18.04

 > Otras referencias:
  > * [(itsfoss.com) How to Install and Configure PostgreSQL on Ubuntu](https://itsfoss.com/install-postgresql-ubuntu/)
 > * [(linuxize.com) How to Install PostgreSQL on Ubuntu 18.04](https://linuxize.com/post/how-to-install-postgresql-on-ubuntu-18-04/)
 > * [(tecadmin.net) How to Install PostgreSQL 11 on Ubuntu 18.04 & 16.04 LTS](https://tecadmin.net/install-postgresql-server-on-ubuntu/)

Acá los pasos para instalar Postgresql desde repositorios de postgresql.org:

##### (_mi preferido_) Instalar postgresql desde repositorio Ubuntu --- tiene versión más nueva. Básicamente hay que actualizar la GPG key en el catálogo, agregar repositorio apt a local y bajar la versión más reciente usando apt normal.
```bash
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
$ sudo apt update
$ sudo apt install postgresql postgresql-contrib
...
Setting up postgresql-12 (12.1-1.pgdg18.04+1) ...
Creating new PostgreSQL cluster 12/main ...
/usr/lib/postgresql/12/bin/initdb -D /var/lib/postgresql/12/main --auth-local peer --auth-host md5
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locales
  COLLATE:  en_US.UTF-8
  CTYPE:    en_US.UTF-8
  MESSAGES: en_US.UTF-8
  MONETARY: es_MX.UTF-8
  NUMERIC:  es_MX.UTF-8
  TIME:     es_MX.UTF-8
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/12/main ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... America/Mazatlan
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

Success. You can now start the database server using:

    pg_ctlcluster 12 main start

Ver Cluster Port Status Owner    Data directory              Log file
12  main    5433 down   postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log
update-alternatives: using /usr/share/postgresql/12/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
Setting up postgresql-contrib (12+210.pgdg18.04+1) ...
Setting up postgresql (12+210.pgdg18.04+1) ...
Processing triggers for postgresql-common (210.pgdg18.04+1) ...
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
  en_us
Removing obsolete dictionary files:
$
```

##### (*no mi preferido*) Instalar postgresql desde repositorio Ubuntu --- tiene versión más vieja:
```bash
$ apt show postgresql
Package: postgresql
Version: 10+190ubuntu0.1
Priority: optional
Section: database
Source: postgresql-common (190ubuntu0.1)
Origin: Ubuntu
$ sudo apt update
$ sudo apt install postgresql postgresql-contrib
```

##### (_no mi preferido_) Sí y solo si tengo Homebrew instalado, podría verificar si postgresql existe en su repositorio del siguiente modo:
```bash
$ brew info postgres
postgresql: stable 12.1 (bottled), HEAD
Object-relational database system
https://www.postgresql.org/
Not installed
$ brew install postgres
...
You can manually execute the service instead with:
  pg_ctl -D /home/linuxbrew/.linuxbrew/var/postgres start
...

$ sudo -u postgres psql -c "SELECT version();"
```


#### Instalar pgadmin4

```bash
$ sudo apt install pgadmin4
```


#### Configurando postgresql

ref. [(digitalocean.com) How To Install and Use PostgreSQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)

```bash
$ pg_ctlcluster 12 main start
Warning: the cluster will not be running as a systemd service. Consider using systemctl:
  sudo systemctl start postgresql@12-main
Error: You must run this program as the cluster owner (postgres) or root


# You can check if PostgreSQL is running by executing
$ service postgresql status
● postgresql.service - PostgreSQL RDBMS
   Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
   Active: active (exited) since Wed 2019-11-27 23:21:26 MST; 18min ago

# also consider following commands:
$ service postgresql
Usage: /etc/init.d/postgresql {start|stop|restart|reload|force-reload|status} [version ..]
# so:
$ service postgresql stop
$ service postgresql restart
$ service postgresql reload
$ service postgresql force-reload

# By default, PostgreSQL creates a special user postgres that has all rights in Ubuntu. To actually use PostgreSQL, you must first log in to that account:
$ sudo su postgres
# Your prompt should change to something similar to:
postgres@ubuntu-VirtualBox:/home/ubuntu$
# Now, run the PostgreSQL Shell with the utility psql:
$ psql
# You should be prompted with:
psql (12.1 (Ubuntu 12.1-1.pgdg18.04+1), server 10.11 (Ubuntu 10.11-1.pgdg18.04+1))
Type "help" for help.

postgres=#

# You can type in \q to quit and \? for help

# To see all existing tables, enter:
\l

# With \du you can display the PostgreSQL users:
\du
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}


 # You can change the password of any user (including postgres) with:

ALTER USER postgres WITH PASSWORD 'my_password';

# Note: Replace postgres with the name of the user and my_password with the wanted password. Also, don’t forget the ; (semicolumn) after every statement.

# It is recommended that you create another user (it is bad practice to use the default postgres user). To do so, use the command:

CREATE USER my_user WITH PASSWORD 'my_password';

# If you run \du, you will see, however, that my_user has no attributes yet. Let’s add Superuser to it:

ALTER USER my_user WITH SUPERUSER;


# You can remove users with:

DROP USER my_user;

# To log in as another user, quit the prompt (\q) and then use the command:

psql -U my_user

# You can connect directly to a database with the -d flag:

psql -U my_user -d my_db

# You should call the PostgreSQL user the same as another existing user. For example, my user is ubuntu. To log in, from the terminal I use:

psql -U ubuntu -d postgres

# Note: You must specify a database (by default it will try connecting you to the database named the same as the user you are logged in as).


# If you have a the error:

psql: FATAL:  Peer authentication failed for user "my_user"

# Make sure you are logging as the correct user and edit /etc/postgresql/11/main/pg_hba.conf with administrator rights:

$ sudo vim /etc/postgresql/12/main/pg_hba.conf 

# Note: Replace 12 with your version (e.g. 10).

# Here, replace the line:
local   all             postgres                                peer

# With:
local   all             postgres                                md5

# Then restart PostgreSQL:
$ sudo service postgresql restart

```

Using PostgreSQL is the same as using any other SQL type database. I won’t go into the specific commands, since this article is about getting you started with a working setup. However, here is a very useful [gist to reference!](https://gist.github.com/Kartones/dd3ff5ec5ea238d4c546) Also, the man page (man psql) and the [documentation](https://www.postgresql.org/docs/manuals/) are very helpful.



> ref. [(digitalocean.com) How To Install and Use PostgreSQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)


* [How To Secure PostgreSQL on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)
* [How To Backup PostgreSQL Databases on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-backup-postgresql-databases-on-an-ubuntu-vps)
* [How to Build a Django and Gunicorn Application with Docker](https://www.digitalocean.com/community/tutorials/how-to-build-a-django-and-gunicorn-application-with-docker)


If you are logged in as the postgres account, you can create a new user by typing:
```bash
$ createuser --interactive
# or instead, you prefer to use sudo for each command without switching from your normal account, type:
$ sudo -u postgres createuser --interactive
# output:
Enter name of role to add: sammy
Shall the new role be a superuser? (y/n) y

```

##### Creating a New Database
Another assumption that the Postgres authentication system makes by default is that for any role used to log in, that role will have a database with the same name which it can access.

This means that, if the user you created in the last section is called sammy, that role will attempt to connect to a database which is also called “sammy” by default. You can create the appropriate database with the createdb command.

If you are logged in as the postgres account, you would type something like:
```bash
$ createdb sammy

# If, instead, you prefer to use sudo for each command without switching from your normal account, you would type:
$ sudo -u postgres createdb sammy

# This flexibility provides multiple paths for creating databases as needed.
```

##### Opening a Postgres Prompt with the New Role

To log in with ident based authentication, you’ll need a Linux user with the same name as your Postgres role and database.

If you don’t have a matching Linux user available, you can create one with the adduser command. You will have to do this from your non-root account with sudo privileges (meaning, not logged in as the postgres user):

```bash
$ sudo adduser sammy

# Once this new account is available, you can either switch over and connect to the database by typing:
$ sudo -i -u sammy
$ psql

# Or, you can do this inline:
$ sudo -u sammy psql
```

This command will log you in automatically, assuming that all of the components have been properly configured.

If you want your user to connect to a different database, you can do so by specifying the database like this:
```bash
$ psql -d postgres

# Once logged in, you can get check your current connection information by typing:
\conninfo
# output:
You are connected to database "sammy" as user "sammy" via socket in "/var/run/postgresql" at port "5432".
# This is useful if you are connecting to non-default databases or with non-default users.
```






> Revisa [esta liga](https://linuxize.com/post/how-to-install-postgresql-on-ubuntu-18-04/) que contiene más detalles de trabajar SQL en Posgresql.


PostgreSQL supports multiple authentication methods. The most commonly used are:
* Trust - With this method, the role can connect without a password, as long as the criteria defined in the pg_hba.conf are met.
* Password - A role can connect by providing a password. The passwords can be stored as scram-sha-256 md5 and password (clear-text)
* Ident - This method is only supported on TCP/IP connections. Works by obtaining the client’s operating system user name, with an optional user name mapping.
* Peer - Same as Ident but it is only supported on local connections.

PostgreSQL client authentication is defined in the configuration file named pg_hba.conf. By default for local connections, PostgreSQL is set to use the peer authentication method.

The postgres user is created automatically when you install PostgreSQL. This user is the superuser for the PostgreSQL instance and it is equivalent to the MySQL root user.

To log in to the PostgreSQL server as the postgres user first you need to switch to the user postgres and then you can access a PostgreSQL prompt using the psql utility:

```bash
$ sudo su - postgres
$ psql
\q

# You can also access the PostgreSQL prompt without switching users using the sudo command:
$ sudo -u postgres psql
```


##### Creating PostgreSQL Role and Database

You can create new roles from the command line using the createuser command. Only superusers and roles with CREATEROLE privilege can create new roles.

In the following example, we will create a new role named john a database named johndb and grant privileges on the database.

More info on [here](https://www.postgresql.org/docs/12/index.html)

```bash
# Create a new PostgreSQL Role
$ sudo su - postgres -c "createuser john"
# Create a new PostgreSQL Database
$ sudo su - postgres -c "createdb johndb"
# Grant privileges
$ sudo -u postgres psql
# and run the following query:
grant all privileges on database johndb to john;

```


#### Instalar MongoDB 4.0

[(linuxize.com) How to Install MongoDB on Ubuntu 18.04](https://linuxize.com/post/how-to-install-mongodb-on-ubuntu-18-04/)


You can consult [The MongoDB 4.0 Manual](https://docs.mongodb.com/manual/) for more information on this topic.


```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
# to avoid error _add-apt-repository command not found_
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository 'deb [arch=amd64] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse'
$ sudo apt update
$ sudo apt install mongodb-org
```

The following packages will be installed on your system as a part of the mongodb-org package:
* mongodb-org-server - The mongod daemon and corresponding init scripts and configurations.
* mongodb-org-mongos - The mongos daemon.
* mongodb-org-shell - The mongo shell is an interactive JavaScript interface to MongoDB. It is used to perform administrative tasks through the command line.
* mongodb-org-tools - Contains several MongoDB tools for to importing and exporting data, statistics, as well as other utilities.


##### Starting MongoDB

```bash
# Starting MongoDB
$ sudo systemctl start mongod
$ sudo systemctl enable mongod

# Verifying MongoDB Installation
$ mongo --eval 'db.runCommand({ connectionStatus: 1 })'
MongoDB shell version v4.0.10
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.0.10
{
  "authInfo" : {
    "authenticatedUsers" : [ ],
    "authenticatedUserRoles" : [ ]
  },
  "ok" : 1
}
# A value of 1 for the ok field indicates success.
```

##### Configuring MongoDB

MongoDB uses a YAML formatted configuration file, ```/etc/mongod.conf```. You can configure your MongoDB instance by editing this file.

The default configuration settings are sufficient for most users. However, for production environments, it is recommended to uncomment the security section and enable authorization as shown below:

```/etc/mongod.conf```
```bash
security:
  authorization: enabled
```

The authorization option enables [Role-Based Access Control (RBAC)](https://docs.mongodb.com/manual/core/authorization/) that regulates users access to database resources and operations. If this option is disabled each user will have access to all databases and perform any action.

After making changes to the MongoDB configuration file, restart the mongod service with:
```bash
$ sudo systemctl restart mongod
```

##### Creating Administrative MongoDB User

If you enabled the MongoDB authentication, create an administrative MongoDB user that will be used to access and manage the MongoDB instance.

First access the mongo shell with:
```bash
$ mongo
# Once you are inside the MongoDB shell type the following command to connect to the admin database:
use admin
# output:
switched to db admin

# Issue the following command to create a new user named mongoAdmin with the userAdminAnyDatabase role:
db.createUser(
  {
    user: "mongoAdmin", 
    pwd: "changeMe", 
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
# output:
Successfully added user: {
	"user" : "mongoAdmin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}

# exit the mongo shell
quit()
```

To test the changes, access the mongo shell using the administrative user you have previously created:
```bash
$ mongo -u mongoAdmin -p --authenticationDatabase admin

use admin
# output:
switched to db admin

# Now, print the users with:
show users
# output:
{
	"_id" : "admin.mongoAdmin",
	"user" : "mongoAdmin",
	"db" : "admin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	],
	"mechanisms" : [
		"SCRAM-SHA-1",
		"SCRAM-SHA-256"
	]
}
```

You can also try to access the mongo shell without any arguments ( just type mongo) and see if you can list the users using the same commands as above.





***
***
_esta guia necesita ser actualizado despues de esta linea_
***




## Instalar Netbeans
ref.
https://linuxize.com/post/how-to-install-netbeans-on-ubuntu-18-04/

https://www.gpsos.es/2019/05/instalacion-de-netbeans-11-y-jdk-8-en-ubuntu-18-04/

https://php.tutorials24x7.com/blog/how-to-install-netbeans-11-for-php-on-ubuntu

https://linuxize.com/post/how-to-install-netbeans-on-ubuntu-18-04/
https://ubunlog.com/como-instalar-flatpak-en-ubuntu/


https://computingforgeeks.com/install-netbeans-ide-on-debian-ubuntu-and-linux-mint/
https://computingforgeeks.com/how-to-install-java-11-on-ubuntu-18-04-16-04-debian-9/



Una vez instalado Java, hacer lo siguiente:

```bash
$ sudo snap install netbeans --classic
```

Si todo termina bien, leeremos:
```bash
netbeans 11.1 from 'apache-netbeans' installed
```


#### Instalar apache2 en Ubuntu 18.04
re. 
https://tecadmin.net/install-mariadb-on-ubuntu-18-04-bionic/
https://computingforgeeks.com/install-mariadb-10-on-ubuntu-18-04-and-centos-7/


```bash
$ sudo apt-get install software-properties-common
$ sudo apt-get install apache2
$ sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
$ sudo nano /etc/apt/sources.list.d/mariadb.list
```


## Install DBeaver on Ubuntu 18.04
ref. 
https://computingforgeeks.com/install-and-configure-dbeaver-on-ubuntu-debian/
https://dbeaver.io/download/


Esto es un cliente de DB que depende de tener Java instalado.

Una vez que Java esta instalado:
```bash
$ sudo wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
$ sudo echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
$ sudo apt-get update && sudo apt-get -y install dbeaver-ce
$ apt policy  dbeaver-ce # check DBeaver version installed
```

https://netbeans.org/kb/docs/php/configure-php-environment-ubuntu.html
https://computingforgeeks.com/how-to-install-latest-phpmyadmin-on-ubuntu-18-04-debian-9/
https://computingforgeeks.com/
https://tecadmin.net/install-phpmyadmin-in-ubuntu/

https://netbeans.org/kb/docs/php/configure-php-environment-ubuntu.html



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


## Referencias:
  - [Instalar Python 3.7 en Ubuntu 18.04](https://tecadmin.net/install-python-3-7-on-ubuntu-linuxmint/)
  - [Versiones en requirements.txt](https://stackoverflow.com/questions/50842144/requirements-txt-greater-than-equal-to-and-then-less-than)
  - [Instalar pypenv](https://djangoforbeginners.com/initial-setup/)
  - [Instalar pip](https://pip.pypa.io/en/stable/installing/)





## Errores

### Visual Studio Code is unable to watch for file changes in this large workspace" (error ENOSPC)

Referencia: 
https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc

When you see this notification, it indicates that the VS Code file watcher is running out of handles because the workspace is large and contains many files. The current limit can be viewed by running:
```bash
$ cat /proc/sys/fs/inotify/max_user_watches
8192
```

The limit can be increased to its maximum by editing ```/etc/sysctl.conf``` and adding this line to the end of the file:
```bash
fs.inotify.max_user_watches=524288
```

The new value can then be loaded in by running ```sudo sysctl -p```.





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
