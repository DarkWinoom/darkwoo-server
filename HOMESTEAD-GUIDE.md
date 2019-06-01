# 安装laravel/homestead简易说明
> 关于如何安装 laravel/homestead，[官方文档](https://laravel.com/docs/5.8/homestead)（[中文版文档](https://learnku.com/docs/laravel/5.8/homestead/)）已经有很详细的流程了，本文档旨在简化必要流程，同时解决一些在安装过程中常见的问题。建议搭配官方文档进行阅读

**本文档主要针对的是Windows系统，对于MacOS和Linux系统的安装也基本适用，相对于来说Windows下安装的坑要多一些...**

## 步骤
### 准备工作
* 安装 git [官网下载地址](https://git-scm.com/download/win)，安装时建议选择 **Git from the command line and also from 3rd-party software** 这样可以在命令行或第三方软件的终端（如VSCode）中使用git命令
* 安装 VirtualBox [官网下载地址](https://www.virtualbox.org/wiki/Downloads)，安装与使用过程中无需打开
* 安装 Vagrant [官网下载地址](https://www.vagrantup.com/downloads.html)，安装完成后需要重启系统

选择最新版本安装即可
### 安装 homestead vagrant box
windows 系统打开 ``` cmd ``` 或者 ``` git bash ```，执行以下命令
```bash
vagrant box add laravel/homestead
```
如果使用的是国内网络，那么可能会遇到服务器未响应或下载速度非常慢的问题，可以使用以下方法离线安装：

* 下载 [virtualbox.box](https://vagrantcloud.com/laravel/boxes/homestead/versions/7.2.1/providers/virtualbox.box) 文件。地址中的7.2.1可以修改成任意[已发布的版本](https://app.vagrantup.com/laravel/boxes/homestead)
* 将下载的文件存放到任意目录，并在同目录中创建一个 ```metadata.json``` 文件，内容如下：

```bash
{
    "name": "laravel/homestead",
    "versions": 
    [
        {
            "version": "7.2.1",
            "providers": [
                {
                  "name": "virtualbox",
                  "url": "virtualbox.box"
                }
            ]
        }
    ]
}
```
* 将两个文件一并放入同一个目录中（请使用非中文目录），然后进入该目录并运行以下命令：
```bash
vagrant box add metadata.json
```

安装完成后输入 ``` vagrant box list ``` 命令将会看到已安装的 box

### 安装 homestead 管理工具
使用 git 命令直接克隆代码
```bash
git clone https://github.com/laravel/homestead.git ~/Homestead
```
对于windows用户，```~/Homestead``` 表示将文件安装到```C:\Users\你的用户名\Homestead```目录中（windows系统直接使用~路径可能会报找错，直接手动进入即可）。接下来运行以下命令：
```bash
cd ~/Homestead

# 克隆期望的发行版本，可以通过github地址查看版本
git checkout v8.5.4

# MacOS 和 Linux 系统执行的命令
bash init.sh

# Windows 系统执行的命令
init.bat
```

### 配置虚拟服务器
运行 init 命令后将会在文件夹内生成 ```Homestead.yaml```、```after.sh```、```aliases``` 文件，使用任意编辑器打开 ```Homestead.yaml``` 修改配置。配置完成后大概是这样的：
```bash
---
ip: "192.168.10.10"
memory: 2048
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa
    - ~/.ssh/id_rsa.pub

folders:
    - map: E:/HOMESTEAD/darkwoo-server
      to: /home/vagrant/code/darkwoo-server

sites:
    - map: darkwoo-server.test
      to: /home/vagrant/code/darkwoo-server/public
      php: "7.1"

databases:
    - darkwoo-server
```

具体配置可以参考上文中提到的官方文档来进行

### 启动虚拟服务器
在 ```Homestead``` 目录中执行 ```vagrant up``` 挂载虚拟机并启动
> 若在启动时提示 ```Check your Homestead.yaml (or Homestead.json) file, the path to your private key does not exist.```，则需要执行 ```ssh-keygen -t rsa``` 命令创建对应的 key 文件，一般开发环境中直接使用默认参数创建即可，如有特殊需要可查阅该命令相关资料自行配置

### 修改hosts文件
我们需要对虚拟服务器的ip与映射的域名写入 hosts 文件中：
```bash
192.168.10.10  darkwoo-server.test
```

至此，一台 ```laravel/homestead``` 虚拟机已经成功运行了
* 根目录对应地址：```E:/HOMESTEAD/darkwoo-server/public``` 
* 访问域名：```http://darkwoo-server.test```

### vagrant常用命令
* **vagrant init** 初始化 vagrant（本项目用不到）
* **vagrant up** 启动 vagrant（每次开机都需要进入目录执行，当然也可以写脚本开机自动启动）
* **vagrant halt** 关闭 vagrant（建议在每次关机前执行）
* **vagrant ssh** 通过 SSH 登录 vagrant（需要先启动 vagrant）
* **vagrant reload --provision** 重新应用更改 vagrant 配置并重启
* **vagrant destroy** 删除 vagrant（所有配置与数据会被删除，谨慎操作）

## 常见问题
### 如何切换默认php-cli版本
```Homestead.yaml``` 文件可以设置站点运行版本，如果需要修改默认的php-cli版本需要使用以下命令：
```bash
update-alternatives --config php
```
之后键入对应版本的数字并按“回车”即可完成切换

### vagrant ssh进入后界面异常，无法正确执行命令
如果是windows系统，则可能是 CMD 或 powershell 版本过低，可以尝试进行更新，或者使用其他工具（如git bash）。windows 10 系统可以右键菜单属性，取消“使用旧版控制台”选项

## 延伸问题
### 共享目录权限
homestead 默认会将共享目录的权限设置为**777**，即允许所有人对文件进行写操作，这样对于权限较为复杂的linux系统来说可以有效的避免一些权限问题。同时在 vagrant 中 ```chomd``` 命令是没有任何效果的，如果有需要修改文件夹权限的需求，可以参考 [官方文档synced-folders](https://www.vagrantup.com/docs/synced-folders/basic_usage.html) 和 [vagrant-synced-folders-permissions](https://jeremykendall.net/2013/08/09/vagrant-synced-folders-permissions/)，通过修改 vagrant 的配置文件赋予文件夹与文件对应的权限

### guest additions 与 virtualbox 版本不匹配
在执行 ```vagrant up``` 命令时可能会看到这样的提示。一般来说版本不匹配不会产生太大影响，但有时候可能会导致共享目录无法访问的问题。这时候推荐一个插件 ```vagrant-vbguest```，该插件会在每次执行 ```vagrant up``` 的时候检测版本，如果不匹配就会自动更新：
```bash
vagrant plugin install vagrant-vbguest
```