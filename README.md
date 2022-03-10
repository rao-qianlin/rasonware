# rasonware
论文笔记
## Ransomware

### Ransomware -Threats, Vulnerabilities And Recommendations

###### 勒索病毒简介

##### 06 June 2017

* Malicious Email Attachments
  * macros
* 解密工具：不充分，需要主动防御
* Internet of Things incorporates (IoT) 物联网
* sandbox 
* proactive

关注并一些敏感命令的执行



### Redemption: Real-Time Protection Against Ransomware at End-Hosts

###### 操作系统保护，监控I/O请求， 防止数据受损，轻量级透明技术

##### 12 October 2017

#### 1. introduction

* Ransomware threat 
* Exisiting approaches
  * Backups of data
  * Reverse analyse cryptography
  * Augment the operating system 
    *  transparent techniques 
    *  make the operating system resistant against ransomware-like behavior.
    *  Immediately stop attacks
    *  Recover any lost data
* This paper: REDEMPTION
  * Augment the operating systerm
  * Transparent

#### 2. Related Work

* 
* Kharraz et al.   Unveil
  * dynamic analysis system, for rverse analyze
  * *not* an end-point solution and no real end-user interaction was involved in their test. Redemption is an end-point solution that aims differentiate between benign and malicious ransomware-like access requests to the file system.
* Scaife et al.   CryptoDrop
  * detect a ransomware attack
  * *does not provide any recovery or minimal data loss guarantees*.
* Continella et al.  ShieldFS
* Kolodenker et al. PayBreak
  * relies on identifying functions that implement cryptographic primitives
  * Redemption does not depend on any hooking technique to identify cryptographic functions. 
* Redemption 核心贡献：防止数据丢失，加强操作系统

#### 3. Threat Model

* ransomware can employ any standard, popular techniques to attack machines similar to other types of malware
* can employ any techniques to generate the encryption key
* malicious code can execute with the privileges of the user
* ransomware attacks mainly occur in the user-mode

#### 4. Design Overview

* a lightweight kernel module that intercepts process interactions and stores the event, and manages the changes in a protected area.
* a user-mode daemon, called behavioral monitor and notification module, that assigns a malice score to a process, and is used to notify the user about the potential malicious behavior of a process.



### Detecting Ransomware with Honeypot Techniques

###### 蜜罐技术

##### 2-4 Aug 2016



### On Deception-Based Protection Against Cryptographic Ransomware

###### 诱饵策略decoy strategies 存在的问题和解决方案

##### 06 June 2019

#### 1.Introduction

* protection and detection strategies
* deception-based protection
  * decoys: user is supposed not to see nor access
  * 如果有进程试图写decoy文件，即判断发生了恶意攻击，做出反应
  * lack of false positives 
  * real-time detection with minimal overhead
  * 需要防止恶意软件可以检测出decoys并跳过。需要尽可能相似于真实用户。
  * 类似于creating a realistic sandbox environment

#### 9. Conclusion and Furture Work

* Future directions
  * combine different anti-ransomware strategies
  * being effective without confusing the user



### Ransomware protection using the moving target defense perspective

###### 随机改变文件拓展名移动目标

##### 14 July 2019

#### 1.Introduction

* monitoring critical Application Programming Interface (API) 
* [Moving Target Defense](https://www.sciencedirect.com/topics/computer-science/moving-target-defense) (MTD) concept



### The aftermath of a crypto-ransomware attack at a large academic institution

##### 2016

# Cuckoo安装历程

操作系统：Ubuntu18.04 TLS

虚拟机：virtualbox + win7_64镜像

## 主机安装

### 基本依赖包

```shell
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev
sudo apt-get install python-virtualenv python-setuptools
sudo apt-get install libjpeg-dev zlib1g-dev swig
```

### 安装数据库

```shell
sudo apt-get install mongodb
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
sudo mysql_secure_installation
# 配置初始密码
sudo mysql -u root
```

``` mysql
USE mysql;
UPDATE user SET plugin='mysql_native_password' WHERE User='root';
FLUSH PRIVILEGES;
exit;
```

```shell
service mysql restart
```

### 安装Cuckoo

使用虚拟环境安装

```shell
virtualenv venv
. venv/bin/activate
pip install -U pip setuptools
pip install -U cuckoo
```

### 其他工具

```shell
sudo apt-get install tcpdump apparmor-utils
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
sudo aa-disable /usr/sbin/tcpdump

sudo apt-get install libcap2-bin

sudo apt-get install yara
sudo apt-get install python-yara

sudo apt-get install volatility
```

### 社区签名

```shell
cuckoo community
```



## 虚拟机安装

```shell
sudo apt-get install virtualbox
```

#### 网卡配置

在virtualbox设置增加hostonly网卡



## 客户机配置

客户机使用window7

1. 安装增强功能
2. 软件安装：
   1. 安装Python pip pillow
   2. 安装qq 微信等软件
3. 关闭防火墙、自动更新
4. 关闭UAC 启用管理员账号
5. 配置agent.py，添加计划任务用管理员自启动
6. 拍快照



### 安装 M2Crypto 0.24.0

1. Download original Source Package and Patch from Debian repo:

   ``` shell
   wget http://deb.debian.org/debian/pool/main/m/m2crypto/m2crypto_0.24.0.orig.tar.xz
   wget http://deb.debian.org/debian/pool/main/m/m2crypto/m2crypto_0.24.0-1.1.debian.tar.xz
   ```

2. Unpack and copy debian folder from the patch to source folder:

   ```shell
   tar xvf m2crypto_0.24.0.orig.tar.xz
   tar xvf m2crypto_0.24.0-1.1.debian.tar.xz
   cp -r debian/ M2Crypto-0.24.0/
   cd M2Crypto-0.24.0/
   ```

3. Install quilt (tool to apply the patch)

   ```shell
   sudo apt install quilt
   ```

4. Add this line at the end of the .bashrc file:

   ```shell
   gedit ~/.bashrc
   # 将这一行添到末尾export QUILT_PATCHES=debian/patches
   # Reload bashrc:
   . ~/.bashrc
   ```

5. Apply the patch (from within M2Crypto-0.24.0 folder):
   quilt push

6. Prepare the package for pip:
   python setup.py sdist

This will create tar.gz archive in dist folder

```
cuckoo@cuckoo-Virtual-Machine:~/M2Crypto-0.24.0$ ls -l dist/ 
total 800 
-rw-rw-r-- 1 cuckoo cuckoo 624777 Feb 10 20:30 M2Crypto-0.24.0-py2.7-linux-x86_64.egg 
-rw-rw-r-- 1 cuckoo cuckoo 190488 Feb 11 09:04 M2Crypto-0.24.0.tar.gz 
```

7. Install dependencies:

   ```shell
   sudo apt install swig
   sudo apt install libssl1.0-dev
   ```

8. Finally install M2Crypto in your python's virtual environment:

   ```shell
   . venv/bin/activate
   pip install ~/M2Crypto-0.24.0/dist/M2Crypto-0.24.0.tar.gz
   ```

If all goes well you should have these folders created:

```
(venv) cuckoo@cuckoo-Virtual-Machine:~$ ls -l venv/lib/python2.7/site-packages/ | grep M2Crypto 
drwxrwxr-x  4 cuckoo cuckoo    4096 Feb 10 20:54 M2Crypto 
drwxrwxr-x  2 cuckoo cuckoo    4096 Feb 10 20:54 M2Crypto-0.24.0.dist-info 
 
(venv) cuckoo@cuckoo-Virtual-Machine:~$ pip list | grep M2Crypto 
M2Crypto          0.24.0 
```

And import in python shouldn't give you any errors:

```
(venv) cuckoo@cuckoo-Virtual-Machine:~$ python 
Python 2.7.15rc1 (default, Nov 12 2018, 14:31:15) 
[GCC 7.3.0] on linux2 
Type "help", "copyright", "credits" or "license" for more information. 
>>> import M2Crypto 
>>> exit() 
```



### 分析结果

* analysis.log：执行过程中的命令行输出
* dump.pcap： 由tcpdump网络嗅探器生成的网络转储
* dump_sorted.pcap：已排序的dump版本
* memory.dmp：全部内存转储
* files/: 存储所有被程序操作过的文件
* files.json：包括操作该文件的所有进程、其在客户机中的原始文件路径等
* logs/:raw logs
* reports/: 生成的报告
* shots/：客户机截屏
* tlsmaster.txt





