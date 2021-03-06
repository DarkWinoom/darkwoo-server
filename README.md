# darkwoo-server
> 使用 Laravel + Mysql 搭建的网页CMS系统后台通用后端（服务端），提供必要的数据处理与 api 通信功能

**该项目目前正在开发中，需要搭配 [darkwoo-client前端](https://github.com/DarkWinoom/darkwoo-client) 一起使用**

## 服务器要求
本项目使用的是 Laravel 5.8，因此服务器需要满足以下要求
* PHP >= 7.1.3
* OpenSSL PHP 拓展
* PDO PHP 拓展
* Mbstring PHP 拓展
* Tokenizer PHP 拓展
* XML PHP 拓展
* Ctype PHP 拓展
* JSON PHP 拓展
* BCMath PHP 拓展

对于 Windows 或者 MacOS 系统，推荐使用 **Laravel Homestead** 虚拟机作为开发环境，本项目开发使用的版本如下：
* homestead v8.5.3
* laravel/homestead v7.2.1
* php-cli v7.1

如果您不清楚如何安装 homestead 或者在安装的过程中遇到问题，可参考本项目附带的 [安装laravel/homestead简易说明](https://github.com/DarkWinoom/darkwoo-server/blob/master/HOMESTEAD-GUIDE.md)

## 项目特点
内容整理中...

## 功能清单
内容整理中...

## Build Setup

```bash
# 克隆项目
git clone https://github.com/DarkWinoom/darkwoo-server.git

# 进入项目目录
cd darkwoo-server

# 安装依赖
composer install

# 复制一份 .env 文件，之后根据服务器实际情况配置此文件
cp .env.example .env

# 生成 key
php artisan key:generate

# 启动内置服务（也可以使用 apache 或 nginx 等服务器）
php artisan serve
```

## License
This project is open-source software licensed under the [MIT license](https://opensource.org/licenses/MIT).