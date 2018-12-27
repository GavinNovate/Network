# Docker 安装



docker 安装

https://docs.docker.com/install/linux/docker-ce/ubuntu/

```shell
# 移除旧版本
sudo apt-get remove docker docker-engine docker.io

# 更新仓库源
sudo apt-get update

# 安装
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# 下载 Key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# 验证
sudo apt-key fingerprint 0EBFCD88
# 结果
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22

# 添加到仓库
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
# 更新仓库
sudo apt-get update

# 安装
sudo apt-get install docker-ce

# 测试
sudo docker run hello-world
```





docker compose 安装

https://docs.docker.com/compose/install/

```shell
# 下载
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

# 赋权
sudo chmod +x /usr/local/bin/docker-compose

# 查看版本
docker-compose --version
```
