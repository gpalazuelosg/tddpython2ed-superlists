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

### Instalación

```sh
$ gedit
$ sudo apt-get update && sudo apt-get upgrade -y
$ sudo apt-get dist-upgrade
$ sudo apt-get update && sudo apt-get upgrade -y
$ exit
$ sudo add-apt-repository ppa:jonathonf/python-3.6
$ sudo apt-get update
$ sudo apt install python3.6
$ python --version
$ python3 --version
$ python3.6 --version
$ pip3 --version
$ wget -O get-pip.py 'https://bootstrap.pypa.io/get-pip.py';
$ python get-pip.py --disable-pip-version-check --no-cache-dir pip==9.0.1
$  sudo python get-pip.py --disable-pip-version-check --no-cache-dir pip==9.0.1
$ pip --version
$ pip3 --version
$ sudo python3 get-pip.py --disable-pip-version-check --no-cache-dir pip==9.0.1
$ pip3 --version
$ sudo python3.6 get-pip.py --disable-pip-version-check --no-cache-dir pip==9.0.1
$ pip3 --version
$ exit
$ ls
$ cd Downloads/
$ ls
$ sudo apt-get install build-essential libssl-dev libffi-dev python-dev
$ sudo apt-get install build-essential libssl-dev libffi-dev python3-dev
$ sudo apt-get install build-essential libssl-dev libffi-dev python3.6-dev
$ which pip3
$ pip3 --version
$ nano ~/.bash_history
$ exit
$ nano ~/.bash_history
$ exit
$ which pip3
$ which pip
$ sudo pip3 install virtualenvwrapper
$ sudo apt-get update && sudo apt-get upgrade
$ pip3 --version
$ sudo pip3 install virtualenvwrapper
$ mkdir ~/.virtualenvs
$ export WORKON_HOME=~/.virtualenvs
$ nano ~/.bashrc
$ which python3
$ python3 --version
$ which python3.6
$ python3.6 --version
$ nano ~/.bashrc
$ cd /usr/local/bin
$ ls
$ nano ~/.bashrc
$ exit
$ mkvirtualenv superlists
$ deactivate
$ workon superlists
$ python --version
$ deactivate
$ mk_project xx
$ exit
$ nano ~/.bash_history
$ pwd
$ ls
$ cp ~/.bash_history ~/Downloads/bashHistory.txt
$ cd Downloads/
$ ls
$ curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
$ sudo apt install curl
$ curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
$ ls
$ rm nodesource_setup.sh 
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install -y build-essential
$ exit
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

### To-Do:
  - Depurar lista de comandos.
  - Agrupar comandos detallando qué hace cada uno.
  - Agregar el texto que incluye al ./bash_rc cuando se configura geckodriver

   [Python TDD Book]: <https://www.amazon.com/Test-Driven-Development-Python-Selenium-JavaScript/dp/1491958707/ref=sr_1_1/131-5649203-4499659?s=books&ie=UTF8&qid=1519889044&sr=1-1&refinements=p_27%3AHarry+Percival>
   [Dillinger.io]: <https://dillinger.io/>
