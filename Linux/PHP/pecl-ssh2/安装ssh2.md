## PECL 安装 ssh2

#### 1. 执行命令
```shell
    pecl install ssh2
```

#### 3. 当出现如下图输出时，说明安装成功
![ssh2 install success](install_success.png)

#### 4. 修改 php.ini
```
extension=ssh2
```

#### 5. 重启 php-fpm
```shell
    sudo systemctl restart php-fpm
```
