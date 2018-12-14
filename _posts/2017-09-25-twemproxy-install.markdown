---
layout: post
title:  "twemproxy install"
date:   2017-09-25 04:26:38 +0000
categories: twemproxy
permalink: twemproxy
---
>twemproxy的安装依赖twemproxy的安装要求autoconf的版本在2.64以上，否则提示”error: Autoconf version 2.64 or higher is required“。

### 安装依赖    
    sudo yum install automake
    sudo yum install libtool

### 登录
    su www
    cd ~
### 下载autoconf
    wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
### 解压 
    tar xvf autoconf-2.69.tar.gz

### 编译，安装
    cd autoconf-2.69
    ./configure --prefix=/home/autoconf-2.69
    make
    sudo make install


### 建立软连接
    sudo ln -s /home/autoconf-2.69 /usr/local/autoconf

### 删除旧版本ac
    sudo rm /usr/bin/autoreconf
    sudo rm /usr/bin/autoconf

### 增加ac配置
    sudo ln -s /usr/local/autoconf/bin/autoreconf /usr/bin/autoreconf
    sudo ln -s /usr/local/autoconf/bin/autoconf /usr/bin/autoconf

### 下载twemproxy
    cd ~ 
   wget  https://github.com/twitter/twemproxy/archive/v0.4.1.tar.gz

### 修改名字
    mv v0.4.1 twemproxy-v0.4.1.tar.gz
### 解压
    tar xvf  twemproxy-v0.4.1.tar.gz
### 编译,安装
    cd twemproxy-0.4.1 
    autoreconf -fvi
    ./configure --prefix=/home/twemproxy-0.4.1 
    make
    sudo make install

### 建立软连接
    sudo ln -s /home/twemproxy-0.4.1 /usr/local/twemproxy
    

