---
title: 设置_VSCode配置PHP
date: 2024-06-11T22:52:12Z
lastmod: 2024-06-11T22:52:12Z
---

### PHP官网地址

[https://www.php.net/downloads.php](https://www.php.net/downloads.php)

### Xdebug环境

[https://xdebug.org/download](https://xdebug.org/download)

将下载完成的文件保存至php的ext目录下，并修改php.ini,添加如下内容

```bash
[xdebug]
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_port=9000

zend_extension="" //xdebug具体路径
xdebug.remote_enable=1
xdebug.remote_autostart=1
```

### 配置Windows系统变量

[`Win`​ + `R`​] 输入 sysdm.cpl
点击`高级`​ -` 环境变量`​

```bash
php -v
```

### VSCode配置

下载扩展插件：
```bash
PHP Debug  
PHP IntelliSense  
PHP Intelephense  
PHP DocBlocker  
PHP Snippets from PHPStrom  
Auto Close Tag  
Auto Rename Tag  
```

在setting.json中添加

```
"php.validate.executablePath": "C:/Users/iroay/OneDrive/SoftBox/php-8.1.10-nts-Win32-vs16-x64/php.exe"
"php.executablePath": "C:/Users/iroay/OneDrive/SoftBox/php-8.1.10-nts-Win32-vs16-x64/php.exe"
```
