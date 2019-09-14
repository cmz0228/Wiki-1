# 使用 RPM 包管理器（Yum）部署 SSPanel UIM

本教程基於RHEL 7（CentOS 7）寫成

開始之前，請確認系統是否有安裝了別的第三方RPM源，如果有，請先刪除再繼續安裝，這樣可以避免跟本文中所用到的repo衝突。執行安裝指令之前，請切換系統用戶至root，或者在每個命令前加上 `sudo` 指令

## 一些準備工作

更新一下系統（這是一個好習慣）

`yum update -y`

安裝Yum管理工具，這在多個步驟中都會用到

`yum install yum-utils -y`

## 安裝 Nginx

先創建 Nginx repo 文件

`vi /etc/yum.repos.d/nginx.repo`

然後粘貼如下内容進去

```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
```

按Esc然後輸入 `:x` 退出，接下來我們要選擇我們要使用的 Nginx 分支

Nginx Mainline 與 Stable 之間的區別可以用下面一張圖説明

![Mainline_vs_Stable](https://i.stack.imgur.com/etScD.png)

這裏我們使用 Mainline 版本作爲示例，使用 `yum-config-manager` 啓用repo

`yum-config-manager --enable nginx-mainline`

然後安裝 nginx

`yum install nginx -y`

##安裝PHP

安裝EPEL Release

`yum install epel-release -y`

然後安裝 Remi repo

`rpm -ivh wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

從 7.1/7.2/7.3 選擇一個你想用的PHP版本，這裏以 7.3 爲例

`yum-config-manager --enable remi-php73`

然後安裝 PHP 以及一些必須的組件

`yum install php php-mysqlnd php-bcmath php-gd php-fpm `
