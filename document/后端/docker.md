# Docker

## 介绍

https://info.support.huawei.com/info-finder/encyclopedia/zh/Docker%E5%AE%B9%E5%99%A8.html

Docker 是一种轻量级的虚拟化技术，也是一个开源的应用容器平台，允许开发者将应用程序及其依赖打包进一个可移植的容器中，并可在任何支持 Linux 或 Windows 的服务器（本质是虚拟了一个linux子系统）上运行。相比传统虚拟机（VM），Docker 容器更轻量、启动更快、资源占用更少。

传统虚拟机通过在物理机上虚拟出多个逻辑设备来运行不同操作系统，虽然提升了硬件利用率，但因需完整安装操作系统而资源开销大、迁移不便。Docker 容器则采用进程级隔离而非操作系统级隔离，在共享主机内核的基础上实现应用环境的隔离，显著降低了系统负担。

Docker 的三大核心特点为：

1. **轻量化**：共享主机内核，启动快，资源消耗低；
2. **标准开放**：基于开放标准，兼容各类主流操作系统和基础设施；
3. **安全可靠**：应用彼此隔离且独立于底层设施，默认提供强隔离性。

其显著优势包括：秒级启停、高密度部署（可同时运行数千容器）、类似 Git 的镜像管理方式、通过 Dockerfile 实现自动化构建与部署，以及几乎无额外系统开销。

Docker 的三大组成要素是：

- **镜像（Image）**：只读模板，包含运行应用所需的所有文件和配置；
- **容器（Container）**：镜像的运行实例，相互隔离、安全独立；
- **镜像仓库（Registry）**：用于存储和分发镜像，支持公有或私有部署。

Docker 采用客户端/服务器（C/S）架构：Docker 客户端发送指令，Docker 守护进程（daemon）在后台接收请求并负责容器的创建、运行与分发。用户通过客户端与守护进程交互，实现对容器的管理。

Docker的运行逻辑如下图所示，Docker使用客户端/服务器 (C/S) 架构模式，Docker守护进程（Docker daemon）作为Server端接收Docker客户端的请求，并负责创建、运行和分发Docker容器。Docker守护进程一般在Docker主机后台运行，用户使用Docker客户端直接跟Docker守护进程进行信息交互。

![Docker的运行逻辑](https://download.huawei.com/mdl/image/download?uuid=7eae7af14c8444849558625ced5038b4)

## 安装

https://www.runoob.com/docker/windows-docker-install.html

https://blog.csdn.net/swadian2008/article/details/137105221

- 安装 Hyper-V，Hyper-V 是微软开发的虚拟机，类似于 VMWare 或 VirtualBox，仅适用于 Windows 10。这是 Docker Desktop for Windows 所使用的虚拟机。

  但是，这个虚拟机一旦启用，QEMU、VirtualBox 或 VMWare Workstation 15 及以下版本将无法使用！如果你必须在电脑上使用其他虚拟机（例如开发 Android 应用必须使用的模拟器），请不要使用 Hyper-V！

## 外网访问

https://blog.csdn.net/qq_29752857/article/details/129683240

https://blog.51cto.com/u_16175516/8743495

在 Docker 中运行 MySQL 并使其能通过外网访问，主要步骤总结如下：

1. **运行 MySQL 容器并映射端口**：
   使用 `docker run` 命令启动 MySQL 容器，将主机的 3307 端口映射到容器的 3306 端口（MySQL 默认端口），同时设置 root 用户密码。例如：

   ```
   docker run --name mysql-container -p 3307:3306 -e MYSQL_ROOT_PASSWORD=your_password -d mysql
   ```

2. **开放服务器防火墙端口**：
   确保服务器防火墙允许外部访问映射的端口（如 3307）。

3. **修改 MySQL 配置以允许远程连接**：
   进入容器，编辑 MySQL 配置文件（如 `/etc/mysql/my.cnf` 或 `/etc/my.cnf`），将 `bind-address` 设置为 `0.0.0.0` 或注释掉该行，使 MySQL 监听所有网络接口。

4. **重启 MySQL 服务**：
    在容器内执行 `service mysql restart` 使配置生效。

5. **授权 root 用户远程访问权限**：

   - MySQL 5.x：

     语句同时授予权限和设置密码：

     ```
     GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'your_password' WITH GRANT OPTION;
     ```

   - MySQL 8.x

     ```
     GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
     
     ALTER USER 'root'@'%' IDENTIFIED BY 'your_password';
     ```

   最后执行 `FLUSH PRIVILEGES;` 刷新权限。

6. **安全提醒**：

    开放数据库端口存在安全风险，应仅在必要时启用，并加强安全配置（如使用强密码、限制访问 IP、使用非 root 用户等）。

完成上述步骤后，即可从外网通过主机 IP 和映射端口（如 `IP:3307`）访问 MySQL 数据库。

##  **Images（镜像）**

> 镜像就像是一个安装程序或者模板，它定义了应用运行所需的一切，但本身不能直接运行。

### Docker 安装

https://hub.docker.com/mysql?tab=tags

```
docker pull mysql:8
```

### 使用

- 列出镜像列表

  ```
  docker images
  ```

- 删除镜像

  镜像删除使用 **docker rmi** 命令，比如我们删除 hello-world 镜像：

  ```
  $ docker rmi hello-world
  ```




## **Containers（容器）**

> 容器是镜像的运行实例，是一个轻量级、可移植的执行环境。
>
> 如果镜像是类，那么容器就是对象实例。一个镜像可以创建多个容器，就像一个类可以创建多个对象。

### 命令

#### 查看

- 查看所有的容器命令如下：

  ```
  docker ps -a
  ```

- 查看我们**正在运行**的容器

  ```
  docker ps 
  ```

- docker inspect 容器

#### 创建容器

```
docker run -d --name <自定义名称>
```

- -d 分离

- --name **容器名称**

- -p 80:80 端口映射，每个容器都运行在独立的虚拟环境中，容器的网络和宿主机的网络是隔离的

  <img src="../img/docker/image-20260325112410871.png" alt="image-20260325112410871" style="zoom:33%;" /><img src="../img/docker/image-20260325112634780.png" alt="image-20260325112634780" style="zoom:33%;" />

- -v 挂载卷  则是把宿主机与容器的文件目录进行绑定，容器内对这个文件夹的修改会影响宿主机的文件夹，反之同理。**挂载前确保宿主机有相应的挂载项**

  <img src="../img/docker/image-20260325113000814.png" alt="image-20260325113000814" style="zoom:33%;" />

  - 如果你在容器里创建了一个文件（例如数据库数据、日志、代码），当你删除这个容器时，**这些文件也会随之永久消失**。
  - 挂载卷的作用就是打破这种隔离，把数据存在宿主机上。
    - **持久化**：容器删了，数据还在宿主机的文件夹里。
    - **共享**：多个容器可以挂载同一个宿主机文件夹，实现数据共享。
    - **开发便利**：你在电脑上修改代码文件，容器里运行的程序能立刻看到变化（无需重新构建镜像）。


#### 进入容器

- 进入交互式命令行 docker exec -it <容器名或ID> <Shell程序>

  docker exec -it nginx bash

#### 启动容器

使用 docker start 启动一个已停止的容器：

```
docker start id
```

#### 删除容器

```
$ docker rm -f id
```



## **Volumes（数据卷）**

Docker 数据卷（Volume）是一种用于持久化容器数据的机制，它由 Docker 管理，独立于容器的生命周期。数据卷可以存储在宿主机的特定位置（通常是 `/var/lib/docker/volumes/`），并且可以在多个容器之间共享。

| 概念                 | 面向对象类比            | 传统软件类比                      | 特性                                                         |
| :------------------- | :---------------------- | :-------------------------------- | ------------------------------------------------------------ |
| **Image (镜像)**     | **类 (Class)**          | 安装光盘 / ISO 文件 / 安装包      | **只读**。它是构建容器的蓝图，包含代码、库、环境变量和配置文件。 |
| **Container (容器)** | **对象 (Object)**       | 运行中的程序进程                  | **可读写的运行实例**。由镜像启动，拥有自己的文件系统、网络空间和进程空间。 |
| **Volume (数据卷)**  | **外部数据库/文件共享** | 独立的硬盘分区 / "我的文档"文件夹 | **持久化存储**。专门用于保存数据，即使容器被删除，数据也不会丢失。 |

### 命令

#### 1. **创建数据卷**

你可以通过 `docker volume create` 命令手动创建一个数据卷：

```
docker volume create my_volume
```

- `my_volume` 是数据卷的名称，可以根据需要自定义。

------

#### 2. **查看数据卷**

创建完成后，可以使用以下命令查看所有数据卷：

```
docker volume ls
```

输出示例：

```
DRIVER    VOLUME NAME
local     my_volume
```

------

docker inspect mysql8

```
    "Mounts": [
            {
                "Type": "volume",
                "Name": "eb2b5771251a603193a32b9e5267678c58d7403f7cb1a9a31c3132c6e2c7ee2a",
                "Source": "/var/lib/docker/volumes/eb2b5771251a603193a32b9e5267678c58d7403f7cb1a9a31c3132c6e2c7ee2a/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```



#### 3. **使用数据卷启动容器**

在启动容器时，可以通过 `-v` 参数将数据卷挂载到容器中。例如：

```
docker run -d --name my_container -v my_volume:/data nginx:latest
docker run -d --name nginx_new -v nginx_volume:/data nginx:trixie-perl
```

- `my_volume`：数据卷的名称。
- `/data`：容器内的挂载点路径。
- `nginx:latest`：使用的镜像名称。

这样，容器内的 `/data` 目录就会与数据卷 `my_volume` 关联。即使容器被删除，数据卷中的数据仍然保留。

------

#### 4. **查看数据卷内容**

数据卷的内容存储在宿主机的 `/var/lib/docker/volumes/` 目录下。你可以通过以下路径访问数据卷的实际内容：

```
/var/lib/docker/volumes/my_volume/_data
```

- `_data` 是数据卷的实际存储目录。

------

#### 5. **删除数据卷**

如果不再需要某个数据卷，可以使用以下命令删除它：

```
docker volume rm my_volume
```

注意：删除数据卷之前，确保没有容器正在使用该数据卷，否则会报错。

------

#### 6. **跨容器共享数据卷**

数据卷可以在多个容器之间共享。例如，启动两个容器并共享同一个数据卷：

```
docker run -d --name container1 -v my_volume:/data nginx:latest

docker run -d --name container2 -v my_volume:/data nginx:latest
```

这样，`container1` 和 `container2` 都会共享 `my_volume` 数据卷，它们对 `/data` 目录的修改会同步到同一个数据卷中。

### 挂载卷

#### 复制到本地

- **前提：挂载前确保宿主机有相应的挂载项**

  ```
  # 1. 确保目标文件夹存在
  mkdir -p //d/appData/docker/nginx/conf.d
  
  # 2. 从临时容器复制配置文件到你的 D 盘
  docker cp temp-nginx:/etc/nginx/conf.d/default.conf //d/appData/docker/nginx/conf.d/default.conf
  
  # 3. (可选) 也把主配置文件复制出来，这很重要！
  docker cp temp-nginx:/etc/nginx/nginx.conf //d/appData/docker/nginx/nginx.conf
  ```



### 挂载卷区别

| 特性             | 数据卷（Volume）               | 挂载卷（Bind Mount）           |
| :--------------- | :----------------------------- | :----------------------------- |
| **管理方式**     | 由 Docker 自动管理             | 由用户手动管理                 |
| **性能**         | 更高（Docker 优化）            | 取决于宿主机文件系统           |
| **跨主机迁移**   | 更容易（支持导出和导入）       | 需要手动复制文件或目录         |
| **权限和安全性** | 更安全（Docker 管理权限）      | 需要用户手动管理权限           |
| **使用场景**     | 长期存储、跨主机迁移、数据库等 | 开发环境、日志记录、代码同步等 |

#### 1. **定义**

- **数据卷（Volume）**：
  - 数据卷是由 Docker 管理的存储区域，通常位于宿主机的 `/var/lib/docker/volumes/` 目录下。
  - 数据卷是独立于容器生命周期的，即使容器被删除，数据卷中的数据仍然存在。
- **挂载卷（Bind Mount）**：
  - 挂载卷是将宿主机上的任意目录或文件直接挂载到容器中。
  - 挂载卷的数据存储在宿主机的指定路径上，完全由用户控制。

#### 2. **管理方式**

- **数据卷**：

  - 由 Docker 自动管理，用户不需要关心数据卷的具体存储位置。

  - 可以通过 `docker volume` 命令进行创建、查看、删除等操作。

  - 示例命令：

    ```
    
    docker volume create my_volume
    
    docker volume ls
    
    docker volume rm my_volume
    ```

- **挂载卷**：

  - 完全由用户控制，数据存储在宿主机的指定路径上。

  - 用户需要手动管理挂载点的权限和内容。

  - 示例命令：

    ```
    
    docker run -d -v /host/path:/container/path <image_name>
    ```

#### 3. **性能**

- **数据卷**：
  - 数据卷的性能通常优于挂载卷，因为 Docker 对数据卷进行了优化，减少了文件系统的开销。
  - 数据卷适合频繁读写的场景。
- **挂载卷**：
  - 挂载卷的性能取决于宿主机的文件系统，可能会受到文件系统性能的影响。
  - 挂载卷适合需要快速访问宿主机文件的场景。

#### 4. **跨主机迁移**

- **数据卷**：

  - 数据卷可以轻松地迁移到其他主机，因为 Docker 提供了 `docker volume export` 和 `docker volume import` 命令。

  - 示例命令：

    ```
    docker volume export my_volume > volume_backup.tar
    
    docker volume import volume_backup.tar my_volume
    ```

- **挂载卷**：

  - 挂载卷的迁移需要手动复制宿主机上的文件或目录。
  - 如果挂载的是宿主机的某个特定路径，迁移时需要确保目标主机上有相同的路径结构。

#### 5. **权限和安全性**

- **数据卷**：
  - 数据卷的权限由 Docker 自动管理，用户不需要担心权限问题。
  - 数据卷的安全性较高，因为数据存储在 Docker 的专用目录中。
- **挂载卷**：
  - 挂载卷的权限完全由用户控制，可能会导致权限问题。
  - 如果挂载的是宿主机的敏感目录，可能会带来安全风险。

------

#### 6. **使用场景**

- **数据卷**：
  - 适合需要长期存储和共享数据的场景，例如数据库数据、缓存数据等。
  - 适合跨主机迁移和备份的场景。
- **挂载卷**：
  - 适合需要快速访问宿主机文件的场景，例如开发环境中的代码同步、日志文件记录等。
  - 适合需要手动控制数据存储位置的场景。

## 迁移

### 命令

#### 容器

##### **导出容器**

```
docker export -o my_container.tar nginx_new
```

这会将容器的内容导出为一个 tar 文件。

##### **导入容器**

```
docker import my_container.tar my_nginx_image:latest
```

这会将 tar 文件导入为一个新的镜像。

#### **数据卷**

如果你需要将数据卷迁移到其他主机，可以使用 `docker volume export` 和 `docker volume import` 命令。

##### 导出数据卷

```
docker export mysql_container > D:\mysql\mysql_backup1.tar.gz
```

```
docker run --rm --volumes-from mysql_volume -v C:\path\to\backup\:/backup alpine tar czf /backup/mysql_backup.tar.gz /var/lib/mysql
```

使用 `docker run` 命令运行一个临时容器，将数据卷挂载到容器中，然后使用 `tar` 命令将数据卷的内容导出到主机上的一个文件中。

##### 导入数据卷

```
docker volume import volume_backup.tar my_volume
```

##### 绑定数据卷

docker run -d --name <容器名称> -v <宿主机路径>:<容器内路径> <镜像名称>

```
docker run -d --name my_container -v my_volume:/path/in/container nginx
```

绑定数据卷：-v my_volume:/container/path

#### 挂载卷

##### 绑定

-v /host/path:/container/path

##### 导出信息

```
docker inspect --format='{{json .Mounts}}' nginx | awk '
BEGIN { RS=","; FS=":" }
{
  if ($0 ~ /"Source"/) {
    gsub(/"/, "", $2)
    gsub(/\[/, "", $2)
    gsub(/\]/, "", $2)
    source = $2
  }
  if ($0 ~ /"Destination"/) {
    gsub(/"/, "", $2)
    destination = $2
  }
  if (source && destination) {
    print "-v " source ":" destination
    source = ""
    destination = ""
  }
}'
```



### 已有容器绑定数据卷和挂载卷

**同时挂载数据卷和绑定主机目录，确保它们指向不同的容器路径，而不是同一个路径**

1. 停止并重新创建容器

   缺点：如果容器中有正在运行的服务或状态，需要手动备份和恢复。

2. 将容器提交为一个新的镜像

   ```
   # 提交容器为新的镜像
   docker commit my_container my_image_with_volume
   # 删除原容器
   docker rm my_container
   # 创建新容器并绑定数据卷
   # 容器中的所有修改（包括文件系统的更改）都会被包含在新镜像中。但容器的数据卷（Volume）不会被包含在新镜像中。
   docker run -p 80:80 -d --name my_container \
     -v my_volume:/app/data \
     my_image_with_volume
   ```

3. 使用 docker cp 手动迁移数据

   如果你不想重新创建容器，但希望将数据迁移到数据卷中，可以使用 `docker cp` 命令手动迁移数据。

   ```
   # 将容器中的数据复制到本地目录
   docker cp my_container:/app/data /home/liming/data_backup
   
   # 删除原容器
   docker rm my_container
   
   # 创建新容器并绑定数据卷
   docker run -d --name my_container \
     -v /home/liming/data_backup:/app/data \
     my_image:latest
   ```

   

### TCR

#### 标记镜像为远程仓库地址

为了将镜像推送到腾讯云容器镜像服务（Tencent Container Registry, TCR），需要先标记镜像：

```
docker tag myapp:latest <registry-url>/myapp:latest
```

其中 `<registry-url>` 是腾讯云容器镜像服务的地址，例如：

```
ccr.ccs.tencentyun.com/myapp:latest
```

#### 登录腾讯云容器镜像服务

使用以下命令登录腾讯云容器镜像服务：

```
docker login ccr.ccs.tencentyun.com
```

输入你的用户名和密码（可以在腾讯云容器镜像服务中创建）。

#### 推送镜像到腾讯云

推送本地镜像到腾讯云容器镜像服务：

```
docker push <registry-url>/myapp:latest
```

#### 拉取 Docker 镜像

从腾讯云容器镜像服务拉取镜像：

```
docker pull <registry-url>/myapp:latest
```

#### 运行 Docker 容器并挂载

##### nginx

```
docker run -d \
  --name web-test \
  --restart unless-stopped \
  -p 80:80 \
  -v /root/docker-data/nginx/config/nginx.conf:/etc/nginx/nginx.conf:ro \
  -v /root/docker-data/nginx/config/mime.types:/etc/nginx/mime.types:ro \
  -v /root/docker-data/nginx/config/conf.d:/etc/nginx/conf.d:rw \
  -v /root/docker-data/nginx/html:/usr/share/nginx/html:rw \
  -v /root/docker-data/nginx/html/logs:/var/log/nginx \
  nginx:latest
```

##### mysql

- 导出

  ```
  docker run --rm --volumes-from mysql_volume -v C:\path\to\backup\:/backup alpine tar czf /backup/mysql_backup.tar.gz /var/lib/mysql
  ```

- 导入数据卷

  - 本地备份放在/root/docker-data/backup/mysql_backup.tar.gz

  - 远端

    - docker volume create mysql_volume_new
    - 

    

## mysql

### 绑定数据卷和挂载卷

```
docker run -d -p 2206:2206 --name mysql_container \
  -v //d/appData/docker/mysql/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=your_password \
  -e MYSQL_DATABASE=your_database \
  -e MYSQL_USER=your_user \
  -e MYSQL_PASSWORD=your_password \
  mysql:latest
  
docker run -d -p 3306:3306 --name mysql_container \
  -v mysql_volume:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=lm_database \
  -e MYSQL_USER=lm \
  -e MYSQL_PASSWORD=123456 \
  mysql:latest
```

参数说明：

1. -p：端口映射，此处映射主机3306端口到容器pwc-mysql的3306端口
2. **`-v //d/appData/docker/mysql/data:/var/lib/mysql`**：
   - 将主机上的 `//d/appData/docker/mysql/data` 目录绑定到容器内的 `/var/lib/mysql` 路径。
   - 这个路径是 MySQL 存储数据的地方。
3. **`-e MYSQL_ROOT_PASSWORD=your_password`**：
   - 设置 MySQL 的 root 用户密码。
4. **`-e MYSQL_DATABASE=your_database`**：
   - 创建一个名为 `your_database` 的数据库。
5. **`-e MYSQL_USER=your_user`**：
   - 创建一个名为 `your_user` 的用户。
6. **`-e MYSQL_PASSWORD=your_password`**：
   - 设置 `your_user` 的密码。
7. **`mysql:latest`**：
   - 使用最新版本的 MySQL 镜像。

### 进入 Mysql 容器

```
docker exec -it mysql_container bash
mysql -u root -p
```

### tip

#### 容器默认存在mysql数据库

这个 `mysql` 数据库**不是你的业务数据**，它是 **MySQL 自己的系统核心数据库**，负责存储和管理 MySQL 运行所需的各种元数据和配置信息。简单来说，它就像是 MySQL 的“控制面板”或“系统目录”。

MySQL 服务器进程（mysqld）在启动和运行时，需要知道：

- 谁来连接？（查 `user` 表验证身份）
- 他能做什么？（查 `db`, `tables_priv` 等表进行权限判断）
- 有哪些配置？（部分配置也存储在这里）

所以，`mysql` 这个数据库是 **MySQL 正常运行的前置条件和基础**。没有它，MySQL 服务器甚至无法启动，因为它不知道任何用户信息（包括 `root`）。

## nginx

### 绑定数据卷和挂载卷

```
docker run -p 80:80 -d --name nginx_container \
  -v nginx_volume:/etc/nginx/conf.d \
  -v //d/appData/docker/nginx/nginx:/etc/nginx \
  -v //d/appData/docker/nginx/html:/usr/share/nginx/html \
  nginx_image:latest
```

**在后台启动一个 Nginx 容器，将容器的网页目录映射到 Windows D 盘的指定文件夹，并开放 80 端口供访问。**

- **`/d/...`**：Git Bash 有时会尝试把这种 Unix 风格的路径自动转换成 Windows 风格（例如变成 `D:\...`），但在传递给 Docker 引擎时，这种转换偶尔会出错（比如变成奇怪的分号路径 `html;D`）。
- **`//d/...`**：双斜杠是告诉 Git Bash **“不要转换这个路径，原样传给 Docker”**。Docker Desktop 对 `//d/` 这种格式兼容性最好，能准确识别为 `D:` 盘。

![image-20260325132706681](../img/docker/image-20260325132706681.png)

## node

```
docker run -p 3600:3600 -d --name node-app -v /root/docker-data/node/:/app node:18 tail -f /dev/null
```

