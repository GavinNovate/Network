# 使用 Nexus 搭建 Maven 私服

Maven 是一个软件（特别是 Java 软件）项目管理及自动构建工具。

Maven 私服是架设在局域网内的一种特殊的远程仓库，目的是代理远程仓库及部署第三方构件（依赖库）。有了私服后，当Maven 需要下载构件时，直接请求私服，私服上存在则下载到本地；否则，私服请求外部的远程仓库，将构件下载到私服，再提供给本地下载。

Nexus 是一款免费的 Maven 私服应用。

#### 搭建 Maven 私服的好处

1. 保证软件所使用的依赖库文件都在私服中有备份，即使远程服务器不能提供服务，也不会影响软件的构建。

2. 管理公司内部依赖库。

   根据公司 B2B 的业务模式，我们的软件需要针对不同客户提供不同的版本，但是软件的核心业务是不变的，将核心业务抽出变成依赖库，让不同版本的软件依赖于这些依赖库，从而只需维护依赖库和软件的特性部分即可，极大地降低维护成本。

   Q：为何不通过 Gitlab 来管理依赖库和不同版本的软件？

   A：Gitlab 无法有效便捷地同时维护不同版本的软件（此处的版本指的是对不同用户需要维护的版本，这些版本中的每个版本还需要升级维护），可能会造成修改一个核心业务的bug而需要手动修改所有版本的软件，维护困难且易于发生版本冲突。

3. 加速自动化构建

   在服务器使用 Gradle（Gradle 是基于 Maven 开发的构建工具）构件软件时，服务器需要去远程中央仓库下载对应依赖库，而下载速度可能不稳定，下载成功与否也不能保证，从而可能影响版本发布。而使用 Maven 私服则不会产生这个问题，在软件开发过程中，需要的依赖库已经被下载到私服，在服务器自动化构建版本时，所有的依赖已经在本地私服，而不需要去远程仓库下载，从而保证版本发布，也加速了自动化构建。



#### 使用 Docker 搭建 Nexus 服务器

1. 安装Docker 及 Docker Compose。（参见附件，如已安装请跳过）

2. 将 docker-compose.yml 文件复制到一个合适的目录，如：maven。（docker-compose.yml 文件见附件）

   docker-compose.yml 是 docker-compose 的配置文件，如下配置包含

   - 使用 hub.docker.com 提供的 sonatype/nexus3 镜像
   - 创建一个 nexus-data 卷，映射到容器的 /nexus-data 目录，该目录是 nexus 数据储存目录，可修改
   - 将宿主机的 8081 端口映射到容器的 8081 端口，8081 端口为 Nexus 服务器监听的端口，如果主机 8081 端口已被占用，可以修改宿主机端口，使用其他端口映射到容器。

   ```yaml
   version: "2"
   
   services:
     nexus:
       image: sonatype/nexus3
       volumes:
         - "nexus-data:/nexus-data"
       ports:
         - "8081:8081"
     
   volumes:
     nexus-data: {}
   ```

   

3. 在 docker-compose.yml 文件所在的目录下，执行以下命令：

   ```shell
   # 后台启动 Nexus 服务
   docker-compose up -d
   
   # 停止 Nexus 服务
   docker-compose down
   ```

   

4. 修改 Nginx 配置，给 Nexus 服务器分配公司域名，建议：maven.autoio.org 或 nexus.autoio.org。