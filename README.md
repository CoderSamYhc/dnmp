# DNMP集成环境
- Docker
- Nginx
- Mysql(5.7)
- Redis
- Php(7.3)
- ES(后期加入)
- Kafka(后期加入)
# 安装
1. 拉取仓库代码
    ```git
    git clone https://github.com/CoderSamYhc/dnmp.git
   git clone https://gitee.com/chengpro_admin/dnmp.git
    ```
2. 创建本地文件夹
    ```bash
    mkdir data log www
    ```
3. 修改env配置
    ```$xslt
    cp .env.sample .env
    ```
# 使用
```bash
# 启动
docker-compose up -d
# 关闭
docker-compose down
# 管理服务
docker-compose start|stop|restart nginx|php|mysql|redis
```