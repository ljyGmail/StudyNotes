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

![停机不收费](image.png)

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
