## 02. 基础 - Docker 架构与容器化

![Docker架构](./images/02_01_docker_architecture.png)

![理解容器](./images/02_02_docker_container.png)

## 03. 基础 - 购买云服务器

进入腾讯云官网`https://cloud.tencent.com/`， 点击`控制台`。

![控制台菜单](./images/03_01_console_menu.png)

![服务器基本信息](./images/03_02_server_basic_info.png)

![服务器配置和操作系统](./images/03_03_server_config_os.png)

- 下一步: 设置网络和主机

![网络和主机](./images/03_04_ip_config.png)

![设置密码](./images/03_05_root_password.png)

- 下一步: 确认配置信息

![确认配置信息](./images/03_06_confirm_finish.png)

- 使用终端访问

![远程登录](./images/03_07_terminal_access.png)

## 04. 基础 - 停机不收费

![停机不收费](./images/04_01_shutdown_no_charge.png)

## 05. 基础 - 安装 Docker

```bash
# 卸载现有的 Docker
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine

# 安装需要的包并配置下载源
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 安装Docker
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 启动并确认Docker是否正常安装
sudo systemctl start docker
docker ps

# 每次重启服务器时自动启动Docker
sudo systemctl enable docker
```

## 06. 命令 - 镜像操作

- 实验要求: 启动一个 nginx，并将它的首页改为自己的页面，发布出去，让所有人都能使用。

![镜像相关命令](./images/06_01_docker_image_commands.png)

- `docker search`

  ![docker search](./images/06_02_docker_search.png)

- `docker pull`

  ![docker pull](./images/06_03_docker_pull.png)

- `docker pull [specified tag]`: 在 docker hub 上查找需要的版本

  ![docker pull specified tag](./images/06_04_docker_pull_specified_tag.png)

- `docker rmi`

  ![docker rmi](./images/06_05_docker_rmi.png)

## 07. 命令 - 容器操作

![容器相关命令](./images/07_01_docker_run_commands.png)

- 直接执行`docker run [image]`命令， 如果本地没有此镜像，会自动下载。容器启动后，会阻塞控制台:

![运行容器](./images/07_02_container_run.png)

- 再打开一个新的控制台，执行`docker ps`，查看容器的运行状态。
- 回到之前的控制台，按下`Ctr+C`让容器停止运行后，再执行`docker ps`， 可以看到没有运行中的容器了。
- 想要看到包括停止运行的容器，需要执行`docker ps -a`。
- 打开，停止，重启容器可以使用`docker start [容器ID]`，`docker stop [容器ID]`，`docker restart [容器ID]`命令

![运行停止重启容器](./images/07_03_container_start_stop.png)

- 执行`docker stats [容器ID]`，查看容器的运行状态。

![查看容器状态](./images/07_04_container_status.png)

- 执行`docker logs [容器ID]`，查看容器的运行日志。

![查看容器日志](./images/07_05_container_log.png)

- 执行`docker rm [容器ID]`，可以删除指定的容器。但删除容器之前，要先停掉该容器，或者进行强制删除(`-f`)。

![删除容器](./images/07_06_container_remove.png)

- 目前启动容器的方式会阻塞控制台，而且启动后无法通过浏览器访问该`nginx`的页面。
- 下面需要学习如何让容器在后台默默启动，而且如何可以通过浏览器访问`nginx`的页面。

## 08. 命令 - run 细节

- 在后台启动容器，并为容器指定名称: `docker run -d --name mynginx nginx`

![后台启动容器](./images/08_01_container_run_daemon.png)

- 此时依然无法从外界访问容器中的内容。原因在与容器只是运行在主机的内部，需要做端口映射才能向外部提供访问容器内容的接口。

![端口映射](./images/08_02_port_mapping.png)

![指定端口启动容器](./images/08_03_docker_run_port.png)

- 此时就可以使用浏览器访问 nginx 中的页面了。

![默认页面](./images/08_04_default_page.png)

- 思考: 上面的端口映射示意图中的 88 能不能重复？ 80 能不能重复？
- 回答: 88 不可以，80 可以。

- 如果需要修改 ngnix 默认提供的页面，需要进入到 nginx 容器中，修改里面的页面内容。
- nginx 的默认页面保存在`/usr/share/nginx/html`路径中。
- 想要知道这个路径不是靠猜的，而是需要参考`Docker Hub`的文档。

![nginx镜像说明](./images/08_05_index_file_path.png)

![修改容器内文件](./images/08_06_modify_page.png)

- 进入容器内部: `docker exec -it mynginx /bin/bash`

![进入容器](./images/08_07_docker_exec.png)

- 可以看到即使是`vi`这种普遍的软件，为了最小化，都是没有的。
- 修改`index.html`内容后，再次访问页面，可以看到已经发生改变。

![修改后页面](./images/08_08_modified_page.png)

- 目前修改页面的方式是进入容器后修改文件，这种方式比较麻烦。
- 以后会学习将内部的文件夹映射到外部的方法，就比较方便了。
- 下面学习如何将修改后的容器发布到应用市场。

## 09. 命令 - 保存镜像

![保存镜像](./images/09_01_save_image.png)

- 将当前运行到容器提交成一个镜像: `docker commit -m "message" [容器ID] [镜像名]:[TAG]`

![提交容器为镜像](./images/09_02_docker_commit.png)

- 保存镜像: `docker save -o [文件名] [镜像名]:[TAG]`

![保存镜像文件](./images/09_03_docker_save.png)

- 加载镜像: `docker load -i [文件名]`
- 使用加载到镜像运行容器

![加载镜像文件并运行容器](./images/09_04_docker_load.png)

## 10. 命令 - 分享镜像

![分享社区](./images/10_01_share_image.png)

- 登录到 Docker Hub: `docker login`

![登录](./images/10_02_docker_login.png)

- 为了让推送到 Docker Hub 到镜像保证唯一，需要将镜像改成`用户名/镜像名:TAG`

![修改镜像名](./images/10_03_docker_tag.png)

- 推送镜像到 Docker Hub:`docker push 用户名/镜像名:TAG`

![推送镜像](./images/10_04_docker_push.png)

- 可以为推送到镜像写一些说明:

![写镜像说明](./images/10_05_write_description.png)

- 为例能够让使用镜像的人不指定版本也可以正常下载使用镜像，标准到做法是再推送一个版本为`latest`到镜像:

![推送最新版本](./images/10_06_push_latest.png)

## 11. 命令 - 实验小结

![小结](./images/11_01_container_summary.png)

- 另外需要在云服务器上配置的是防火墙的配置，比如，现在启动一个`88`端口的 myngnix 容器的话，在外界是无法访问的。

![防火墙配置](./images/11_02_fw_config.png)

![防火墙配置](./images/11_03_fw_config.png)

![防火墙配置](./images/11_04_fw_config.png)

## 12. 存储 - 目录挂载

- 目前启动的 nginx 容器有几个问题。

1. 想要修改页面的内容，需要进入到容器内容进行修改，非常麻烦。
1. 每个容器运行在自己到进程内，有自己到文件系统，数据也就是独立存储到。如果容器被删除了，容器中到数据也就丢失了。

- 此时可以使用文件挂载命令，解决上面到问题:

![目录挂载](./images/12_01_dir_volume.png)

- 运行以下命令，可以看到挂载目录后，即使容器被删除了，数据还是被保留的。
- 通过实验表明，一旦挂载，目录中的数据是以容器外的宿主机的目录内容为准的。如果挂载一个宿主机的空目录，容器内的目录也就空了。

![挂载命令](./images/12_02_volume_commands.png)

- 除了可以挂载`目录`以外，还可以挂载`文件`。

## 13. 存储 - 卷映射

- 对于挂载目录的方式，在有些情况是不适合使用的。例如想将一个目录挂载到 nginx 到配置文件所在到目录。

![映射配置文件目录](./images/13_01_mapping_config_dir.png)

- 由于目录挂载时，目录中到数据会以容器外部的宿主机的数据为准，此时如果宿主机的目录是空的，那么容器中对应的目录也就变空了。
- 如果该目录中包含了容器启动时必需的文件，这时可能会导致容器无法正常启动。

![目录挂载错误示例](./images/13_02_dir_mount_error.png)

- 下面看一下`卷映射`的方式:

![卷映射](./images/13_03_volume_mapping.png)

- 使用卷映射的方式，可以保证容器运行时，宿主机目录的内容会以容器中目录的数据为准。
- 在指定卷映射时，不以路径的形式指定就会被识别为卷映射。
- 卷映射的目录路径在`/var/lib/docker/volumes/`下。

![卷映射示例](./images/13_04_volume_mapping_example.png)

- 关于`docker volume`的一些命令:

![volumn命令](./images/13_05_volume_commands.png)

## 14. 网络 - 自定义网络

- 想要实现多个容器之间的网络通信:

![实现容器间通信](./images/14_01_container_communicating.png)

- 下面这种方式效率很低，明明在一个 Docker 环境中，还要通过外部的 IP 和端口访问其他容器:

![低效率方式](./images/14_02_external_access.png)

- Docker 每启动一个容器，都相当于加入了`docker0`这个网络环境，Docker 会为每一个容器分配唯一的 IP。

![docker0](./images/14_03_docker0.png)

- 但是这种使用 IP 访问的方式也会有一些弊端，比如，IP 可能会因为某种原因发生变化。
- 可以使用一种稳定的访问方式，那就是采用域名访问。
- 但`docker0`默认不支持主机域名，需要我们创建一个自定义网络。
- 创建自定义网络后，容器名就可以当作域名来使用。

![自定义网络](./images/14_04_network.png)

- 运行容器时指定自定义网络:

![指定自定义网络](./images/14_05_applying_network.png)

- 直接使用容器名访问:

![使用容器名作为域名](./images/14_06_container_name_as_domain.png)

## 15. 网络 - Redis 主从集群

![redis主从集群](./images/15_01_redis_cluster.png)

- 为了方便配置，这里使用`bitnami`提供的`redis`镜像，里面的环境变量名和数据存储路径需要参考 docker hub 上的文档。

![运行主机容器](./images/15_02_run_master.png)

- 但是执行`主机`的运行容器的命令后，发现没有正常启动。查看日志，可以看到是因为映射目录的权限不够，需要修改目录的权限。

![修改数据目录权限](./images/15_03_modify_mode.png)

- 运行`从机`容器:

![运行主机容器](./images/15_04_run_slave.png)

- 使用客户端进行验证:

- 主机连接配置

![主机连接配置](./images/15_05_master_db_config.png)

- 从机连接配置

![从机连接配置](./images/15_06_slave_db_config.png)

- 验证效果

![验证效果](./images/15_07_verify_result.png)

## 16. 最佳实践

![alt text](./images/16_01_best_practice.png)

- 根据最佳实践，查找文档后，运行`mysql`的容器:

![alt text](./images/16_02_run_mysql_following_best.png)

- 测试效果:

![alt text](./images/16_03_verify_connection.png)

## 17. Docker Compose - 安装 wordpress

- `Docker Compose`是 Docker 用来批量管理容器的工具。

![Docker Compose](./images/17_01_docker_compose.png)

- 使用目前学过的命令来分别运行`mysql`和`wordpress`的容器:

![wordpress创建流程](./images/17_02_wordpress_process.png)

- 创建`网络`，并创建`mysql`容器:

![创建网络和mysql](./images/17_03_create_network_mysql.png)

- 创建`wordpress`容器:

![创建wordpress](./images/17_04_create_wordpress.png)

- 访问`wordpress`进行测试:

![wordpress界面](./images/17_05_wordpress_page.png)

![wordpress界面](./images/17_06_wordpress_page.png)

![wordpress界面](./images/17_07_wordpress_page.png)

- 当前运行容器的方法，显得比较繁琐。将来如果在其他的机器上安装，还要记住每个单独的`docker`命令。
- 下面学习如何使用`docker compose`工具一次性安装需要的容器。

## 18. Docker Compose - 语法

![Docker Compose语法](./images/18_01_docker_compose.png)

- 编写`compose.yaml`文件:

```yaml
name: myblog

services:
  mysql:
    container_name: mysql
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_DATABASE=wordpress
    volumes:
      - mysql-data:/var/lib/mysql
      - /app/myconf:/etc/mysql/conf.d
    restart: always
    networks:
      - blog

  wordpress:
    image: wordpress
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: 123456
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress:/var/www/html
    restart: always
    networks:
      - blog
    depends_on:
      - mysql

volumes:
  mysql-data:
  wordpress:

networks:
  blog:
```

- 删除相关的资源

![删除卷](./images/18_02_remove_volume.png)

![删除网络](./images/18_03_remove_network.png)

- 运行`docker compose`:

![运行Docker Compose](./images/18_04_run_compose.png)

- 验证安装效果

## 19. Docker Compose - 其他

- 使用`docker compose down`命令来关掉容器组时，容器和网路资源会被删除，但处于安全的考虑，数据卷不会被删除。
- 要想在关掉容器组时，删除掉镜像和数据卷，则需要在`down`命令后面指定参数 `--rmi all  -v`:

![docker compose down](./images/19_01_docker_compose_down.png)
