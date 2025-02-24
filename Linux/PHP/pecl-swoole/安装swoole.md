## PECL 安装 swoole

#### 1. 执行命令
```shell
    pecl install swoole
```
#### 2. 按照如图所示配置
![swoole install options](swoole_install_options.png)

#### 3. 当出现如下图输出时，说明安装成功
![swoole install success](install_success.png)

#### 4. 修改 php.ini
```
extension=swoole
```

#### 5. 重启 php-fpm
```shell
    sudo systemctl restart php-fpm
```
