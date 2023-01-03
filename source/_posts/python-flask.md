---
title: flask 学习笔记
date: 2021-09-08 19:42:44
tags:
- 工作
- 技术
- flask
- python
categories: 技术
description: python web 入门
photos:
---
[toc]

# flask运行环境设置

在docker中安装  
base镜像 centos  
新建容器 centos-python-web  
centos 包依赖下载   
`yum install pkg-name`

```shell
# add user
adduser antpython
yum install passwd
passwd antpython
## add wheel user group for using 'sudo' cmd
gpasswd -a antpython wheel
## checkout antpython user
sudo -iu antpython

# init dev-env
sudo yum install epel-release
## install gcc and ngnix
sudo yum install gcc nginx
sudo yum install wget

# install anaconda
wget https://repo.continuum.io/archive/Anaconda3-2019.10-Linux-x86_64.sh
sh Anaconda3-2019.10-Linux-x86_64.sh
## activate the ana env
source anaconda3/bin/activate 

# creat virtual python env
## install virtualenv for python env management
pip install virtualenv
mkdir myproject
cd myproject/
## create virtual env
virtualenv myprojectenv
## activate env
source myprojectenv/bin/activate

# init flask app
pip install uwsgi flask

# write a flask app
    # from flask import Flask
    # application = Flask(__name__)

    # @application.route('/')
    # def Hello():
    #     return "Hello There!"

    # if __name__ == '__main__':
    #     application.run(host='0.0.0.0')

# in order to link container and local, need open sys 5000 port
# when the docker container creats, bind the port

# create the wsgi web server
## create the flask web app run methord
vim mywsgi.py
  #from myproject import application

  #if __name__ == '__main__':
  #    application.run()
## test the run by wsgi
nohup uwsgi --socket 0.0.0.0:5000 --protocol=http -w mywsgi &
## config uwsgi cfg-file
vim myproject.ini
    # [uwsgi]
    # module = mywsgi

    # master = true
    # processes = 5
    # threads = 100

    # http = 0.0.0.0:5000
    # virtualenv = /home/antpython/myproject/myprojectenv
    # die-on-term = true
# run uwsgi by cfg-file
uwsgi --ini myproject.ini

# set the restart script: the linux-sys restart then the python-web restart
vim myproject.service
    # [Unit]
    # Description=uWSGI instance to serve myproject
    # After=network.target

    # [Service]
    # User=antpython
    # Group=nginx
    # WorkingDirectory=/home/antpython/myproject
    # Environment="PATH=/home/antpython/myproject/myprojectenv/bin"
    # ExecStart=/home/antpython/myproject/myprojectenv/bin/uwsgi --ini myproject.ini
sudo systemctl start myproject.service
sudo systemctl enable myproject.service

##test
sudo reboot now
```

# python pip下载安装包以及其依赖，到指定目录

```python
pip freeze > requirements.txt
pip download -d DIR -r requirements.txt
pip install --no-index --find-links=DIR -r requirements.txt
```

# conda 多版本环境切换

```shell
conda create -n your_env_name python=x.x
conda activate your_env_name
conda deactivate
```

