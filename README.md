# Laradock精简优化版

 
> 基于[Laradock](https://laradock-docs.linganmin.cn/) 精简，只保留了PHP+MYSQL+MONGODB+REDIS+NGINX+APACHE ，针对PHP添加了redis、mongodb 、 xhprof和xdebug扩展

<a name="Intro"></a>
## 介绍

Laradock包含预包装 Docker 镜像，提供你一个美妙的开发环境而不需要安装 PHP, NGINX, MySQL 和其他任何软件在你本地机器上。

**使用概览：**

1. 安装 docker，[下载MAC版](https://download.docker.com/mac/stable/Docker.dmg)   ； 安装docker-compose ,[下载docker-compose]（https://github.com/docker/compose/releases）

2. 下载 Laradock ：

    ```bash
    git clone https://github.com/laradock/laradock.git  &&  cd  laradock   &&  docker-compose up -d
    ```
    
3. 设置NGINX站点配置

4. 打开你的项目的数据库配置文件，然后设置mysql地址为"mysql" 和 redis地址为"redis"。

5. 打开浏览器，访问 localhost


## 许可证

[MIT License](https://github.com/laradock/laradock/blob/master/LICENSE) (MIT)
