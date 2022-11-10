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