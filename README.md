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

```Bash
docker-compose up -d 以守护进程启动服务，如果首次运行会自动build

docker-compose stop 关闭服务，这个服务下的所有容器都会stop
docker-compose down 关闭并且移除容器，内建网络，镜像和挂载的内容卷
docker-compose restart 重启你的服务
docker-compose help 查看其他命令
```

潜在的问题： 
jenkins一直无法访问，docker ps 查看之后发现jenkins一直在重启，
查看docker日志 ：docker logs jenkins 发现是jenkins数据文件夹权限有问题
解决：chown -R 1000:1000 /root/jenkins_home //用户组改变
变更之后需要重新容器

因为本操作是在阿里云ECS上进行安装的，需要在安全组里开放用到的端口。

##### Step 5 解锁Jenkins

```Bash
# 在安装完成后，默认生成了一个登录密码，首次登录需要这个密码。
# 密码路径：var/jenkins_home/secrets/initialAdminPassword //容器内部
# 查找密码：
docker exec -it xxx bash //进入jenkins容器
cat /var/jenkins_home/secrets/initialAdminPassword //查看密码
```
