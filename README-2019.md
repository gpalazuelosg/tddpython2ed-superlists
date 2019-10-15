# Python, Django y TDD
### Actualizado 2019-08
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


## Después de instalar Ubuntu en VirtualBox

Para acceder a los archivos del sistema host (en donde está instalado VirtualBox), debes hacer una configuración en VirtualBox para que el sistema host comparta archivos hacia las VMs.


### Asignar nuevo grupo a usuario de VM
Después de tener esa configuración, si desde la VM intentas acceder a los archivos del sistema host, normalmente dice "sf_<nombre_folder_compartido>", obtendremos error de acceso denegado.

Falta correr algo en la VM para tener acceso:

```bash
$ sudo adduser [your-user-name] vboxsf
```

### Agregar nuevo método de entrada de teclado
Después de eso, normalmente yo tengo que agregar el input keyboard method de Español. Ahora sería buen momento para aprovechar la próxima reiniciada del VM.

Después de reiniciar la VM y ya tendremos acceso a las carpetas compartidas.


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

#### Instalar NodeJS 8.x usando nvm (preferida)
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


#### Instalar NodeJS 8.x usando apt-get
```bash
$ cd Downloads/
$ sudo apt install curl
$ curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
$ rm nodesource_setup.sh 
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install -y build-essential
```


### Instalar Python 3.7

En Ubuntu 18.04.03 ya esta instalado Python 3.6. Eso puede ser suficiente. Pero yo quiero instalar Python 3.7. 

```bash
$ sudo apt-get update && sudo apt-get upgrade -y
$ sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
$ cd /usr/src
$ sudo wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
$ sudo tar xzf Python-3.7.4.tgz
$ cd Python-3.7.4
$ sudo ./configure --enable-optimizations
$ sudo make altinstall      # this step takes several seconds


$ python --version
$ python3 --version
$ python3.7 --version
```

```make altinstall``` is used to prevent replacing the default python binary file /usr/bin/python.


Instalar pip3 y pipenv:

```bash
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get-pip.py
$ pip3 install pipenv
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

Al momento de actualizar esta guía, era _geckodriver-v0.24.0-linux64.tar.gz_

```
$ cd ~/Downloads/
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz
$ tar -xvzf geckodriver-v0.24.0-linux64.tar.gz
$ rm geckodriver-v0.24.0-linux64.tar.gz
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

Ahora puedes hacer hacer cambios en el repositorio y publicarlos.


> Dependiendo el código actual, debe mostrar que todas las pruebas fueron satisfactorias ó algún. En cualquier caso, estamos bien pues se logró reinstalar todas las librerías y se puede continuar con el libro.



## Instalar Golang

Referencias: 
  - https://golang.org/doc/install
  - https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79

```
$ cd ~/Downloads/
$ sudo curl -O https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
$ sudo tar -xvf go1.12.5.linux-amd64.tar.gz
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

### Probar instalación de go

Comprobar que go está instalado. Debería imprimir algo como se muestra debajo de la ejecución del comando.
```
$ go version
go version go1.12.5 linux/amd64
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

### Ahora si, instalar Docker CE
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
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


## Instalar PHP y Laravel

ref. 
https://php.tutorials24x7.com/blog/how-to-install-php-7-on-ubuntu-1804-lts
https://apache.tutorials24x7.com/blog/how-to-install-apache-2-on-ubuntu-1804-lts
https://php.tutorials24x7.com/blog/how-to-install-php-for-nginx-on-ubuntu-1804-lts
https://nginx.tutorials24x7.com/blog/how-to-install-and-configure-nginx-on-ubuntu-18-04-lts
https://apache.tutorials24x7.com/blog/how-to-install-apache-2-on-ubuntu-1804-lts


----
https://php.tutorials24x7.com/blog/how-to-install-php-7-on-ubuntu-1804-lts

```bash
# Refresh package indexes
$ sudo apt-get update

# Install PHP 7.2 on Ubuntu 18.04 LTS
$ sudo apt-get install php7.2

# Autoclean
$ sudo apt-get autoclean

# Autoremove
$ sudo apt-get autoremove # or sudo apt-get --purge autoremove

# Install FPM Extension
$ sudo apt-get install php7.2-fpm

# Install MySQL Extension
$ sudo apt-get install php7.2-mysql


# required
$ sudo apt-get install -y php7.2 php7.2-mysql libapache2-mod-php7.2 php7.2-opcache php7.2-zip

# optionals:
$ sudo apt-get install php7.2-cgi php7.2-cli php7.2-curl php7.2-json php7.2-gd php-imagick php7.2-mbstring php7.2-intl php7.2-pspell php7.2-imap php7.2-sqlite3 php7.2-tidy php7.2-xmlrpc php7.2-xsl php7.2-fpm

```

It will ask to confirm the installation. Press Y and hit Enter to confirm the installation. The above-mentioned command will install the PHP at /usr/bin/php7.2 and place the configuration file at /etc/php/7.2/cli/php.ini.


Install OPcache to enable caching at the bytecode level.

```bash
# Install OPcache extension
$ sudo apt-get install php7.2-opcache
```

Install the extensions to handle compressed files.
```bash
# Install Zip
$ sudo apt-get install php7.2-zip
```

## Instalar Composer
ref. 
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-18-04
https://tecadmin.net/install-php-composer-on-ubuntu/
https://linuxize.com/post/how-to-install-and-use-composer-on-ubuntu-18-04/


```bash
$ sudo apt update
$ sudo apt install curl php-cli php-mbstring git unzip
$ cd ~
$ curl -sS https://getcomposer.org/installer -o composer-setup.php
$ sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
$ composer
$ sudo chown -R $USER ~/.composer/
```

## Instalar Laravel
Asumiendo que PHP y composer estan instalados

ref.
https://stackoverflow.com/questions/26376516/laravel-php-command-not-found

```bash
$ composer global require "laravel/installer"
$ laravel
laravel: command not found
```

Si obtenemos el error anterior, hacer lo siguiente:
```bash
$ nano ~/.bash_profile


# agregar al archivo la siguiente linea:
export PATH=~/.composer/vendor/bin:$PATH

# ahora grabar el archivo
# y entonces hacer source en bash:
$ source ~/.bash_profile

# intentar nuevamente commando laravel
$ laravel
Laravel Installer 2.1.0
...
```

Instalar Laravel/Valet
ref.
https://vander.host/knowledgebase/laravel/install-guide-for-laravel-valet/
https://cpriego.github.io/valet-linux/requirements
https://laravel-news.com/valet-for-ubuntu-linux

https://websiteforstudents.com/install-php-7-2-mcrypt-module-on-ubuntu-18-04-lts/


```bash
$ sudo apt-get install libnss3-tools jq xsel
$ $ sudo apt-get install -y php7.2-cli php7.2-common php7.2-curl php7.2-json php7.2-mbstring php7.2-opcache php7.2-readline php7.2-xml php7.2-zip
$ sudo apt install php-pear php7.2-dev libmcrypt-dev gcc make autoconf libc-dev pkg-config

$ sudo apt install php-dev libmcrypt-dev php-pear
$ sudo apt-get -y install gcc make autoconf libc-dev pkg-config
$ sudo apt-get -y install libmcrypt-dev

$ composer global require laravel/valet # no use esta
$ composer global require cpriego/valet-linux
$ valet install
```



## Instalar Java

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

Descargar el instalador para Amazon Corretto JDK 11, de 64 bits. Actualmente el archivo es java-11-amazon-corretto-jdk_11.0.4.11-1_amd64.deb
https://d3pxv6yz143wms.cloudfront.net/11.0.4.11.1/java-11-amazon-corretto-jdk_11.0.4.11-1_amd64.deb

```bash
$ sudo apt-get update && sudo apt-get install java-common
$ sudo dpkg --install ~/Downloads/java-11-amazon-corretto-jdk_11.0.4.11-1_amd64.deb
$ java -version
openjdk version "11.0.4" 2019-07-16 LTS
OpenJDK Runtime Environment Corretto-11.0.4.11.1 (build 11.0.4+11-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.4.11.1 (build 11.0.4+11-LTS, mixed mode)
```

Si acaso no veo la versión de Amazon Corretto como la default:
```bash
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
```



Si quisiera desinstalar Amazon Corretto:
```bash
$ sudo dpkg --remove java-11-amazon-corretto-jdk
```


(Existe una opción para JDK 8 de Amazon Corretto, pero intento documentar diferentes pasos).



## Instalar Eclipse
ref. 
https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/how-to-install-eclipse-ide-on-ubuntu-18-04-lts.html
https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-18-04-ubuntu-16-04.html
https://linuxize.com/post/how-to-install-the-latest-eclipse-ide-on-ubuntu-18-04/


https://php.tutorials24x7.com/blog/how-to-install-eclipse-for-php-on-ubuntu
https://php.tutorials24x7.com/blog/how-to-install-eclipse-for-php-on-ubuntu-using-pdt



Download and extract Eclipse package to your desired directory (Ex. /usr/).

Then, symlink the eclipse executable to /usr/bin path so that all users on your machine can able to use Eclipse IDE.

```bash
$ wget http://mirrors.xmission.com/eclipse/technology/epp/downloads/release/2019-09/R/eclipse-jee-2019-09-R-linux-gtk-x86_64.tar.gz
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


## Instalar IntelliJ IDEA Community Edition

En caso de que quiera instalar Intellij IDEA, la versión gratuita.

```bash
$ sudo snap install intellij-idea-community --classic
```

## Instalar apache2, mysql (MariaDB) en Ubuntu 18.04
re. 
https://tecadmin.net/install-mariadb-on-ubuntu-18-04-bionic/
https://computingforgeeks.com/install-mariadb-10-on-ubuntu-18-04-and-centos-7/


```bash
$ sudo apt-get install software-properties-common
$ sudo apt-get install apache2
$ sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
$ sudo nano /etc/apt/sources.list.d/mariadb.list
```

Grabar lo siguiente en el archivo y salir de nano:
```
# MariaDB 10.3 Repository
deb [arch=amd64,arm64,ppc64el] http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main
deb-src http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main
```
ó púde haber hecho lo siguiente:
```
$ sudo add-apt-repository "deb [arch=amd64,arm64,ppc64el] http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu $(lsb_release -cs) main"
```

Después instalar mariadb:
```bash
$ sudo apt update
$ sudo apt install mariadb-server mariadb-client
```

La instalación de MariaDB registra un symlink "mysql" y además inicia el servicio.

Verificar instalacion de MariaDB
```bash
$ sudo systemctl status mariadb
```

Deberia de leer algo como:
```Active: active (running) since Mon 2019-10-14 23:35:24 MDT; 50s ago```

Para conectar desde la shell a MariaDB:
```bash
$ mysql -u root -p
```

Asegurar la instalación de MariaDB:
```bash
$ sudo mysql_secure_installation
```


Stop, Start, status y restart de MariaDB:
```bash
$ sudo systemctl stop mariadb.service      # To Stop MariaDB service 
$ sudo systemctl start mariadb.service     # To Start MariaDB service 
$ sudo systemctl status mariadb.service    # To Check MariaDB service status 
$ sudo systemctl restart mariadb.service   # To Stop then Start MariaDB service 
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
