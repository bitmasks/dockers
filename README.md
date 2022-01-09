# devops  docker all for one

### 怎么使用
 下载并启动(请确保docker已经启动，另外docker-compose已经安装！):
 ```
 https://github.com/bitmasks/dockers.git  && cd dockers && docker-compose up -d
 ```
 
### 特点

- 容器都固定IP
- 容器都固定IP
- 容器都固定IP
- 容器之间可直接使用IP通讯（你用hosts别名也行的） ， 宿主机访问容器使用localhost， 端口对宿主机暴露

### 组件列表
- redis
  - 端口6379， 没有密码
- nginx
  - 站点配置在 nginx/etc/conf.d 目录下
- php7
  - php-fpm端口：9001
  - ngnx站点配置实例，参考 nginx/etc/conf.d/site2.conf
- php56
  - php-fpm端口：9000
  - ngnx站点配置实例，参考 nginx/etc/conf.d/site1.conf
- mysql
  - pwd： root/123456
- elasticsearch
- mongo
- prometheus-node-exporter-demo
   浏览器：http://localhost:9100/metrics
- prometheus
  - 浏览器：http://localhost:9090/
- grafana
  - 浏览器：http://localhost:3000/  ,  账号： admin   , 密码：admin
- jaeger + web ui
  - 浏览器：http://localhost:16686/
- kibana
- logstash
- finereport
  - 报表  ，demo：http://demo.finereport.com/
- openldap     ldap
- phpldapadmin ldap管理页面
  - 浏览器：http://localhost:9081/
- openldap_self-service-password ldap自助密码服务
  - 浏览器：http://localhost:9082/


### 相关命令

- 构建服务
    - `docker-compose build `
- 启动服务,启动过程中可以直接查看终端日志，观察启动是否成功
    - `docker-compose up`
    - `docker-compose  up redis `
    - `docker-compose restart`
    - `docker-compose restart redis`
- 启动服务在后台，如果确认部署成功，则可以使用此命令，将应用跑在后台，作用类似 nohup python waller.py &
    - `docker-compose up -d`
    - `docker-compose up -d redis`
- 查看日志,效果类似 tail -f waller.log
    - ` docker-compose logs -f `
    - ` docker-compose logs redis `
- 停止服务,会停止服务的运行，但是不会删除服务所所依附的网络，以及存储等
    - `docker-compose stop`
    - `docker-compose stop redis`
    - `docker-compose kill`
- 删除服务，并删除服务产生的网络，存储等，并且会关闭服务的守护
    - `docker-compose down`
    - `docker-compose down redis`

