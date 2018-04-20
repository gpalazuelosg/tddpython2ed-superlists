# Python, Django y TDD
Documentar los pasos ejecutados mientras leo libro [Python TDD Book] en 2da. edición.
![](https://images-na.ssl-images-amazon.com/images/I/51q3VZT%2BseL._SX379_BO1,204,203,200_.jpg)]

La razón de documentar es sencilla:
  - Repetir exactamente los pasos inicializar un nuevo VM con servidor Ubuntu con Python 3.6 para desarrollar, en 1 hora
  - Instalar NVM y NodeJS
  - Instalar VSCode 
  - Generar llaves SSH para actualizar en GitHub y BitBucket.

### Preparando mi VM con Linux Ubuntu 16.04
Scripts ejecutados en Ubuntu 16.04 primero para instalar los programas, librerías y herramientas necesarias para trabajar con el libro. Algunos pasos deben ser actualizados, como version de geckodriver. 

> Es ideal para iniciar en un nuevo Ubuntu, instalando Python 3.6, VSCode, NVM, Node, Geckodriver, Django y Selenium.

## Instalación

### Instalar git
```
$ sudo apt install git
```

### Instalar Node

Conozco dos formas de instalar node. 
  - Usando NVM
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
$ sudo apt install curl
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.9/install.sh | bash
$ nvm --version
exit       # close Terminal and open a new one
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


```
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
$ sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
$ sudo apt-get update
$ sudo apt-get install code
$ exit
$ dir
$ mkdir sandbox
$ workon superlists
$ python --version
$ deactivate
$ ls
$ cd Downloads/
$ ls
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-linux64.tar.gz
$ tar -xvzf geckodriver-v0.18.0-linux64.tar.gz
$ rm geckodriver-v0.18.0-linux64.tar.gz
$ chmod +x geckodriver
$ cp geckodriver /usr/local/bin/
$ sudo cp geckodriver /usr/local/bin/
$ exit
$ workon superlists
$ python functional_tests.py 
$ deactivate
$ exit
$ geckodriver --version
$ pwd
$ ls
$ cd sandbox/
$ ls
$ dir
$ workon superlists
$ pip3 install "django<1.12" "selenium<4"
$ mkdir tddpython2ed && cd _
$ mkdir tddpython2ed
$ cd tddpython2ed/
$ git
$ sudo apt install git
$ cd ~
$ dir
$ ls -la
$ mkdir .ssh
$ cd .ssh
$ ssh-keygen -t rsa -b 4096
$ sudo apt-get install xclip
$ xclip -sel clip < ~/.ssh/id_rsa.pub
$ cd ~/sandbox/tddpython2ed/
$ ls
$ clear
$ git clone git@github.com:gpalazuelosg/tddpython2ed-superlists.git
$ ls
$ cd tddpython2ed-superlists/
$ ls
$ python manage.py runserver
$ deactivate
$ exit
```


#### Generar Markdown
Para generar el markdown me apoye con [Dillinger.io].

También use:
  - [pandao.github.io/editor.md](https://pandao.github.io/editor.md/examples/full.html)
  - [stackedit.io](https://stackedit.io/app)


### To-Do:
  - Depurar lista de comandos.
  - Agrupar comandos detallando qué hace cada uno.
  - Agregar el texto que incluye al ./bash_rc cuando se configura geckodriver

   [Python TDD Book]: <https://www.amazon.com/Test-Driven-Development-Python-Selenium-JavaScript/dp/1491958707/ref=sr_1_1/131-5649203-4499659?s=books&ie=UTF8&qid=1519889044&sr=1-1&refinements=p_27%3AHarry+Percival>
   [Dillinger.io]: <https://dillinger.io/>
