## 安装

```shell
#	卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                
#	安装软件包，设置阿里云镜像库
yum install -y yum-utils
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 


#	安装docker引擎和containerd容器
yum install docker-ce docker-ce-cli containerd.io
# 指定版本号
yum install docker-ce-<版本号> docker-ce-cli-<版本号> containerd.io

# 启动docker
systemctl start docker
```

```shell
# 判断docker是否安装成功，运行hello-world
docker version
docker run hello-world
```

![image-20220101161842095](E:\Typora\笔记\typora图片复制存储\image-20220101161842095.png)

![image-20220101161800503](E:\Typora\笔记\typora图片复制存储\image-20220101161800503.png)

```shell
# 卸载Docker
# 1.卸载docker引擎，cli和容器包
yum remove docker-ce docker-ce-cli containerd.io
# 2.主机上的映像、容器、卷或自定义配置文件不会自动删除。删除所有映像、容器和卷
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```



## 常用命令

### 帮助命令

```shell
docker version			#显示docker版本信息
docker info         #显示docker系统信息，包括镜像和容器的数量
docker --help       #帮助命令，显示docker的所有命令
docker xx --help		#显示xx命令的具体参数
```



```shell
[root@akie ~]# docker images
REPOSITORY       TAG          IMAGE ID       CREATED        SIZE
blog-0.0.1.jar   latest       5b603e1bf066   2 days ago     729MB
nginx            latest       605c77e624dd   2 days ago     141MB
redis            latest       7614ae9453d1   10 days ago    113MB
mysql            latest       3218b38490ce   11 days ago    516MB
rabbitmq         management   6c3c2a225947   2 weeks ago    253MB
hello-world      latest       feb5d9fea6a5   3 months ago   13.3kB
java             8            d23bdf5b1b1b   4 years ago    643MB

#解释
REPOSITORY 	镜像的名称
TAG				 	镜像的标签
IMAGE ID   	镜像的id
CREATED			镜像的创建时间
SIZE				镜像的大小
```

```shell
attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

```

