# 服务器部署文件

##### 思路
- nginx放在ECS宿主机内，端口(80)，方便缓存静态资源
- mysql (3306)、redis (6379)、jenkins (8080) 集成到docker-compose 
- node应用独立镜像

##### Step 1 购买ECS

阿里云ECS，系统选择center os，一路初始化到底

##### Step 2 安装nginx

```Bash
yum -y install nginx
sudo systemctl enable nginx # 设置开机启动 
sudo service nginx start # 启动 nginx 服务

sudo service nginx stop # 停止 nginx 服务
sudo service nginx restart # 重启 nginx 服务
sudo service nginx reload # 重新加载配置，一般是在修改过 nginx 配置文件时使用
```

##### Step 3 安装Docker & Docker-compose

参考官网 [docker](https://docs.docker.com/engine/install/centos/#install-using-the-repository)

##### Step 4 下载配置文件，docker安装mysql redis jenkins
