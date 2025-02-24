
#### 1. 下载 Redis 源码
```shell
    wget https://download.redis.io/releases/redis-7.0.0.tar.gz
```
#### 2. 解压源码包
```shell
    tar -xzf redis-7.0.0.tar.gz
    cd redis-7.0.0
```

#### 3. 编译和安装
```shell
    make
    sudo make install
```

#### 4. 配置 Redis
```shell
    sudo mkdir /etc/redis
    sudo cp redis.conf /etc/redis/redis.conf
```
编辑配置文件以自定义 Redis 设置：
```shell
    sudo vi /etc/redis/redis.conf
```
+ daemonize yes --- 让 Redis 以守护进程方式运行
+ bind 0.0.0.0 --- 所有IP均可以访问
+ requirepass yourpassword --- 设置 Redis 访问密码

#### 5. 配置 service 文件
```shell
    sudo tee /etc/systemd/system/redis.service <<-'EOF'
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
EOF
```

#### 6. 修改 service 文件权限并重载 systemd 配置
```shell
    sudo chmod 644 /etc/systemd/system/redis.service
    sudo systemctl daemon-reload
```


#### 7. 启动 Redis 服务 并设置开机启动
```shell
    sudo systemctl start redis
    sudo systemctl enable redis
    sudo systemctl status redis
```

