# docker-snippets
常用docker脚本集合

### `docker run`相关
- 本地运行docker时指定`env`file。主要用于运行时注入环境变量

    ```shell
    docker run --name local-test \
        --env-file .local.env \
        -d \
        -p 80:80 \
        midewaytest
    ```

    说明： `midewaytest`镜像名称， `local-test`为container名称。`.local.env`为指定的env-file

### `docker compose` 相关

- `docker compose`运行`mysql`和`rabbitmq`
  1. 创建docker-compose.yml，并输入以下内容：
   
    ```dockerfile
    version: '3.6'
    services:
    mysql8:
    image: mysql
    container_name: mysql8-container
    restart: always
    ports: # 映射端口
        - '3306:3306'
    environment:
        - MYSQL_ROOT_PASSWORD=123456
        - MYSQL_DATABASE=video_test
        - TZ=Asia/Shanghai
    volumes:
        - ~/docker-data/mysql:/var/lib/mysql
    # 设置mysql字符串集方式和 collation排序字段方式
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password

    rabbit:
        image: rabbitmq:3.9-management-alpine
        container_name: "rabbitmq-3.9-management-alpine"
        restart: always
        ports:
        - "5672:5672"
        - "15672:15672"
        volumes:
        - ./rabbitmq.conf:/etc/rabbitmq/rabbitmq.conf
        - ~/docker-data/rabbitmq:/var/lib/rabbitmq

    ```
    2. 执行以下`shell`脚本
   
    ```shell
    # 关闭容器
    docker-compose stop || true;
    # 删除容器
    docker-compose down || true;
    # 构建镜像
    docker-compose build;
    # 启动并后台运行
    docker-compose up -d;
    ```