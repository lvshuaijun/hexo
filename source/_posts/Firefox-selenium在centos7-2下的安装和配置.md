---
title: Firefox+selenium在centos7.2下的安装和配置
date: 2019-07-04 18:44:52
tags: 
- linux
- selenium
- firefox
header_image: /intro/simptab-wallpaper-20190705164728.png
---

#### 之前在linux使用firefox+selenium破解某网站的滑块，在这里着重写一下环境配置：

--- 
##### 相关软硬件版本和要求
- selenium : 3.14.0
- firefox : firefox-61.0.2
- linux : centos6.7x86_64
- geckodriver : v0.24.0
- python : 2.7
- 需要root权限

--- 

##### 相关安装流程

1. 将之前selenium版本卸载...
    ```bash
    pip uninstall selenium  
    pip install selenium==3.14.0
    ```
2. 安装firefox
    ```bash 
    cd /usr/local/          
    # 这个速度比较快～
    sudo  wget http://ftp.mozilla.org/pub/firefox/releases/61.0.2/linux-x86_64/en-US/firefox-61.0.2.tar.bz2
    sudo tar xjvf firefox-61.0.2.tar.bz2 
    rm -f /usr/bin/firefox
    ```
3. 设置软连接 **（这里注意请不要使用相对路径！）** 
    ```bash
    sudo ln -s /usr/local/firefox/firefox /usr/bin/firefox
    ```
4. 安装依赖gtk3和gtk2
    ```bash
    sudo yum install gtk3
    sudo yum install gtk2
    ```
5. 安装geckodriver
    ```bash
    cd /usr/local/bin
    wget https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz
    sudo  wget https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz
    sudo tar xvzf geckodriver-v0.24.0-linux64.tar.gz 
    rm -f /usr/bin/geckodriver
    sudo ln -s /usr/local/geckodriver /usr/bin/geckodriver
    geckodriver --version
    ```

--- 
####  **总结：这就是基本步骤，配置完成之后，就可以在linux下使用无头的firefox跑selenium了！** 