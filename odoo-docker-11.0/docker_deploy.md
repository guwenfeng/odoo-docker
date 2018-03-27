# 介绍
使用Odoo11.0开源框架搭建.使用docker进行部署
`#`代表root, `$`代表普通用户

# Docker 安装
```sh
$ curl https://get.docker.com | sh  # 使用文件安装
$ sudo usermod -aG docker $USER     # 将当前用户加入docker组, 以后就无需通过sudo调用docker命令
$ curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://ae251d83.m.daocloud.io  # 使用daocloud加速
```

# 拉取官方postgres镜像
```sh
$ docker pull postgres:9.5
```

##下载 dockerfile 

# 创建odoo镜像
```sh 
$ docker build -t odoo11 . # 时间较长
```


# 启动容器
```sh
$ mkdir -p /etc/odoo11/  #存放odoo模块
$ mkdir -p /var/lib/odoo #存放数据文件
$ cp -r ~/myaddons/addons /etc/odoo11/
$ mkdir -p /etc/odoo/   # 存放 config 文件
$ sudo chmod 777 /etc/odoo /var/lib/odoo
$ cp /home/odoo11/deploy/odoo.conf /etc/odoo/
$ docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name db postgres:9.5
$ docker run  -v /etc/odoo:/etc/odoo  -v /etc/odoo11/addons/:/mnt/extra-addons  -v /var/lib/odoo:/var/lib/odoo -p 8021:8069 --name odoo11 --link db:db -d odoo11
```
访问 `http://127.0.0.1:8021/`


