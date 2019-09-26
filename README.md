# 压测工具安装流程

## 1、准备工作

### 1）系统 Ubuntu 16.04 以上

### 2) python 2.7 级以上

### 3) 安装包：

​			get-pip.py

​			google-chrome-stable_73.0.3683.103-1_amd64.deb

## 安装过程			

```shell
# 
mkdir -p /opt/unpages
cd /opt/unpages
#安装 python pip
python get-pip.py
# 安装 Chrome浏览器
dpkg -i google-chrome-stable_73.0.3683.103-1_amd64.deb
# 安装失败 补齐依赖
sudo apt-get install -f -y
#再次安装chrome
dpkg -i google-chrome-stable_73.0.3683.103-1_amd64.deb
#安装chromedriver
wget http://npm.taobao.org/mirrors/chromedriver/73.0.3683.68/chromedriver_linux64.zip
# 安装dockeer
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update && sudo apt-get -y install docker-ce
#拉去docker镜像
docker pull nginx
# 安装虚拟图像化工具
sudo apt-get install xvfb -y
# 安装程序依赖
mkdir -p /opt/pages
pip install flask
pip install selenium
pip install EasyProcess
pip install PyVirtualDisplay
pip install threadpool
#启动程序
ln -s /opt/pages/chromedriver /bin/chromedriver
export PYTHONPATH=$PYTHONPATH:/opt/pages/dgAutoWeb/src
nohup python /opt/pages/dgAutoWeb/dgAutoWeb.py &
docker run -t -i -v /opt/pages/dgAutoWeb/mork:/usr/share/nginx/html:ro -p 40001:80 -d --name Meet40001 nginx
#防火墙启动窗口
sudo ufw allow prot
```



### 压测工具使用办法（仅适合特定环境）

```shell
#1、清除遗留线程：
ps -ef | grep google-chrome | grep -v grep | cut -c 9-15 | xargs kill -9
ps -ef | grep chromedriver | grep -v grep | cut -c 9-15 | xargs kill -9
ps -ef | grep Xvfb | grep -v grep | cut -c 9-15 | xargs kill -9
#2、压测应用主要端口 62000
netstat -ltpn
#观察端口是否正常启动
#3、停止压测工具
#3.1 ：kill -9 {pid}  {pid} 为 62000端口所占用线程。
#3.2 ：执行 1 步骤
#4、启动压测工具
#4.1:  
export PYTHONPATH=$PYTHONPATH:/opt/pages/dgAutoWeb/src
#4.2： 
cd /opt/pages/dgAutoWeb/ && nohup python dgAutoWeb.py &
#4.3： netstat -ltpn 查看62000端口是否启动
#5、执行完1-4步骤还无法使用压测 请重启服务器。
```

