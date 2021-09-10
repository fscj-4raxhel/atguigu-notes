# Docker 核心基础



### Docker是什么？

容器虚拟化技术

核心三要素：

1. 镜像：模版，相当于类。
2. 容器：由镜像生成的，能运行应用程序的运行时环境。相当于对象/实例。简易版的Linux，仅包含业务运行所有需要的runtime环境
3. 仓库：存镜像的地方，中心仓库是外网的`Docker Hub`。国内用的话阿里云加速是必备的，因为`Docker Hub`访问外网太慢了

### Docker优势是什么？

对比传统虚拟机：

* 更少的抽象层，docker没有Hypervisor，直接给干掉了；所以说docker容器上运行的程序都是直接用的真实物理机的硬件资源
* docker利用宿主机的内核，因此建立一个新容器时，docker不需要和虚拟机一样重新加载一个操作系统内核。

轻便：虚拟机镜像轻轻松几个G，Docker容器只搞一份硬件虚拟，每个应用只包含自己运行所需要的类库
快捷：启动从虚拟机的分钟级下降成秒级
独立：集装箱么，互相不影响

### Docker hello-world

`docker run hello-world`注意实际运行的指令后面还有个冒号`:`冒号后面是标签

![Docker run hello-world](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/Hello_World.png)

### Docker 常用命令

#### 帮助命令

1. `docker version`
   Docker 装好了就有
2. `docker info`
   比上一个全面一些，简单了解就好了
3. `docker --help`
   相当于`Linux`的`man`

#### 镜像命令

鲸鱼背上有集装箱

蓝色的大鲸鱼 --- 宿主机上面

鲸鱼 --- docker

集装箱 --- 容器实例 来自于 镜像（模版）

1. `docker images`：列出**本地**能够运行的镜像的模版
   `docker images -a`：显示所有镜像（包含中间层）：镜像是分层的一层又一层
   `docker images -q`：仅返回ID
   `docker images --digests`：显示镜像的摘要信息

   `docker images --no-trunc`：显示完整的镜像信息

2. `docker search tomcat`：就是去`Docker Hub`上去搜索
   `docker search -s 30 tomcat`：超过30个start的才显示
   `docker search --no-trunc --automated`：显示完整摘要信息，只列出`automated build`镜像

3. `docker pull`：下载镜像`docker pull tomcat` 等价于`docker pull tomcat:latest`

4. `docker rmi -f IMAGE_ID`：删除镜像，用镜像名也可以，如果用镜像名冒号tag同上。`-f `保证能删有正在运行容器的镜像
   `docker rmi -f IMAGENAME1:tag IMAGENAME2:tag`：删除多个
   `docker rmi -f $(docker images -qa)`

#### 容器命令

1. `docker run [OPTIONS] IMAGE [COMMAND][ARG...]`
   常用启动命令`[OPTIONS]`说明

   * `-i`以交互模式运行容器，通常与`-t`同时使用
   * `-t`为容器重新分配一个伪输入终端，`-i`同时使用
   * ![docker run -it IMAGEID](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/docker_run_it.png)

2. `docker ps [OPTIONS]`：docker有哪几个容器在跑
   `docker ps -l`：上一次运行的
   `docker ps -a`：全部运行过的
   `docker ps -n 3`：上3次运行过的
   `docker ps -q`：静默模式，只显示镜像ID

3. 退出容器的两种方式：
   容器停止退出：`exit`

   容器不停止退出：`control + p + q`,是个sequence来的

4. `docker start CONTAINERID`：启动容器
   ![docker start](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/docker_start.png)

5. `docker restart CONTAINERID`：重启容器
6. `docker stop CONTAINERID/NAME`：（温柔）停止容器
7. `docker kill CONTAINERID/NAME`：强制停止容器
8. `docker rm CONTAINERID/NAME`：删除已停止的容器/未停止的加`-f`
   `docker rm $(docker ps -aq)`：删除全部已停止的容器
   `docker ps -a -q| xargs docker rm`：删除全部已停止的容器

---

下半场，**很重要**

1. `docker -d IMAGENAME/IMAGEID`：守护模式/后台运行模式
   如果此时直接`docker ps -a`会发现容器已经退出。这是很重要的一点，`Docker`容器后台运行就必须有一个前台进程，容器运行的指令如果不是哪些一直挂起的命令，就是会自动退出的。
   如果不想退出可以想办法让它一直做事情，比如一直打印

2. `docker logs -f -t --tail CONTAINERID`：查看容器日志

3. `docker top CONTAINERID`：查看容器内运行的进程，注意容器就是一个精简的Linux环境，所以Linux能用的指令包上`docker` 和`CONTAINERID`基本上都能用

4. `docker inspect CONTAINERID`：描述容器内部细节

5. `docker attache CONTAINERID`：重新进入之前退出没停止的容器`control + p + q`出来的那个，直接进入容器启动Terminal，不会启动新的进程

   `docker exec -t CONTAINERID ls -l /tmp`：在容器中打开新的终端，并且可以启动新的进程，可以直接返回结果(隔山打牛)

6. `docker cp CONTAINERID:PATH_INSIDE_CONTAINER path_in_host_machine`：从容器拷贝内容到宿主机上

### 镜像原理

镜像是一种轻量级、可执行的独立软件包。<span style="color:red;">用来打包软件运行环境和基于运行环境开发的软件</span>，它包含运行某个软件所需的所有内容，包括代码，运行时，库，环境变量和配置文件。

#### UnionFS（联合文件系统）

* 是一种分层、轻量级并且高性能的文件系统，它支持<span style="color:red;">对文件系统的修改作为一次提交来一层一层的叠加</span>，同时可以将不同目录挂在到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像
* 特点：一次同时加载多个文件系统，但从外面看来，只能看大一个文件系统，联合加载会把隔层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录



#### Docker镜像加载原理

* 镜像是又一层层的文件系统组成，UnionFS。
* bootfs(boot file system)主要包含
  * bootloader：主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统。<span style="color:red;">Docker景象那个最底层是bootfs</span>，在这一层上与我们经典的Linux系统是一样的，包含boot加载器和内核。
  * kernel：boot加载完以后，整个内核就都在内存中了，此时内存的使用权已由bootfs交给内核，此时系统也会卸载bootfs
* rootfs（root file system）：在bootfs之上，包含的就是经典Linux系统中的`/dev`，`/proc`，`/bin`，`/etc`，等标准目录和文件。rootfs就是各种不同的操作系统发型版`Ubunt`、`CentOS`等

**为什么Docker CentOS容器才200MB？**
因为，一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具，就可以了；因为底层直接用的宿主机的Kernel，自己只需要提供rootfs就可以了。

**为什么Docker镜像要采用这种分层结构呢？**

最大的好处就是资源共享，比如，有多个镜像都从相同的base镜像构建而来，那么组书记只需要在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，就可以为所有容器提供服务了。而且镜像的每一层都可以被共享。

**镜像的特点：都是只读的**
当容器启动时，一个新的科协层被加载到镜像的顶部。这一层通常被称作容器层，容器层之下的都叫镜像层。

### Docker commit

1. `docker commit -m="commit messages" -a="Author" CONTAINERID IMAGENAME:[TAG]`

   提交容器副本使之成为一个新的镜像

   容器 == `docker commit`==> 镜像

### 容器数据卷

做持久化的，还希望容器之间共享数据

#### 是什么

插到笔记本（容器）上拷数据用的U盘（容器数据卷）
Redis里面的rdb和aof（目前还没学过）

#### 能干什么

卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过UnionFS提供一些持久化和共享数据的特性：

卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂在的数据卷

* 数据卷可以在容器之间共享或重用数据
* 卷中的更改可以直接生效
* 数据卷中的更改不会包含在镜像的更新中
* 数据卷的生命周期一直持续到没有容器使用它为止

两件事：1. 容器的持久化； 2. 容器间继承+共享数据

#### 数据卷

两种方式：1.直接命令添加；2. DockerFile添加

**直接命令添加**

1. `docker run -it -v /ABSPATH_IN_HOST:/PATH_IN_CONTAINER IMAGENAME`

![docker run -it -v /ABSPATH_IN_HOST:/PATH_IN_CONTAINER IMAGENAME](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/docker-it-v.png)

docker 会自己新建文件夹



2. `docker run -it -v /ABSPATH_IN_HOST:/PATH_IN_CONTAINER:ro IMAGENAME`

容器内只读（写保护）
例如说，我运行了如下指令：

`docker run -it -v /Users/chris/myDataVolume:/dataVolumeContainer:ro a0477e85b8ae`

然后在本机上`/Users/chris/myDataVolume`加了个host.txt文件，容器内可以看到，但是不能写

![](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/resources/read-only-volume.png)

**DockerFile添加**

DockerFile是镜像的源码级描述，也类似于Linux下的Shell Script

`docker build -f ~/mydocker/DockFile -t fscj/centos .`

#### 数据卷容器

父子容器之间用`--volume-from`之间形成数据共享，即便删了父容器也不影响子容器中的数据卷

容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止

### DockerFile解析

1. 手动编写一个dockerfile文件，必须要符合file的规范
2. 有这个文件以后，直接`docker build`命令执行，获得一个自定义的镜像
3. `docker run`

DockerFile是用来构建镜像的构建文件，是由一系列命令和参数构成的脚本

#### DockerFile构建过程解析

**DockerFile内容基础知识**

1. 每条保留字指令都必须为大写字母且后面要跟随至少一个参数
   `FROM scratch`从祖宗镜像开始
2. 指令按照从上到下，顺序执行
3. `#`表示注释
4. 每条指令都会创建一个新的镜像层，并对镜像进行提交

**Docker执行DockerFile**的大致流程

1. docker 从基础惊喜那个运行一个容器
2. 执行一条指令并对容器做出修改
3. 执行类似于docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dockerfile中的下一条指令知道所有指令都执行完成

**保留字指令**

1. `FROM` ：基础镜像，当前新镜像是基于哪个镜像的
2. `MAINTAINER` ：镜像维护者的姓名和邮箱
3. `RUN` ：容器构建事需要运行的命令
4. `EXPOSE` ：当前容器队外暴露出的端口，tomcat镜像默认对外暴露`8080`
5. `WORKDIR` ：制定创建容器后，终端默认登陆的进来工作目录，一个落脚，默认是`/`
6. `ENV` ：用来在构建镜像过程中设置环境变量
   `ENV MY_PATH /usr/mytest`这个环境变量可以在后续的任何`RUN`指令中使用，就如同在命令前面指定了环境变量前缀一样；也可以在其他指令中直接使用这些环境变量
7. `ADD` ：拷贝加解压缩，将宿主机目录下的文件拷贝进镜像且`ADD`命令会自动处理`URL`和解压`tar`压缩包
8. `COPY` ：只是拷贝，两种写法：
   8.1 `COPY src dest`
   8.2`COPY ["src","dest"]`
9. `VOLUME ` ：容器数据卷，用于数据持久化
10. `CMD` ：制定一个容器启动时要运行的命令
    格式和`RUN`类似，也是有两种
    10.1 `shell`格式：`CMD <COMMAND>`
    10.2 `exec`格式：`CMD ["EXECUTABLE","ARG1","ARG2",...]`
    DockerFile中可以有多个`CMD`，但只有最后一个生效，被传参覆盖
11. `ENTRYPOINT` ：也是制定启动容器时运行的指令和参数传参会追加
12. `ONBUILD` ：父镜像在被子镜像继承后，父镜像的`ONBUILD`会被触发

**案例解析**

1. Docker Hub上绝大多数镜像都是通过在base镜像中安装和配置需要的软件构建出来的

2. 自己写`centos`，`ifcongif`没有，`vim`也没有，自己搞一下
   ![](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/first_DockerFile.png)

3. `docker build -f /Users/chris/mydocker/DockerFile2 -t mycentos:1.3 .`

   ![Building DockerFile](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/build_first_dockerfile_1.png)   

![DockerFile build complete](/Users/chris/Coding Bootcamp/Notes/atguigu-notes/DevOps/Docker/resources/build_first_dockerfile_2.png)