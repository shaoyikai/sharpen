### 镜像

1) 使用 images 命令列出所有镜像

`docker images`

TAG信息用来标记来自来自同一仓库的不同镜像。例如，ubuntu仓库中有多个镜像，通过TAG信息来区分发行版本，包括10.04，12.04等标签。

镜像大小信息只是表示该镜像的逻辑体积大小，实际上由于相同的镜像层本地只会存储一份，物理上占用的存储空间会小于各镜像的逻辑体积之和。

2) 使用 tag 命令添加镜像标签

`docker tag ubuntu:latest myubuntu:latest`

3) 使用 inspect 命令查看镜像详细信息

`docker inspect ubuntu:14.04`

4) 使用 history 命令查看镜像历史

`docker history ubuntu:14.04`

注意过长的命令被自动截断了，可以使用前面提到的-- no- trunc 选项来输出完整命令。

5) 搜寻镜像

`docker search -s 3 nginx`

6) 删除镜像

`docker rmi myubuntu:latest`

当同一个镜像拥有多个标签的时候，docker rmi命令只是删除该镜像多个标签中的指定标签而已，并不影响镜像文件。因此上述操作相当于只是删除了镜像 2fa927b5cdd3 的一个标签而已。

但当镜像只剩下一个标签的时候就要小心了，此时再使用docker rmi 命令会彻底删除镜像。

当使用 docker rmi 命令， 并且后面跟上镜像的ID（也可以是能进行区分的部分ID串前缀）时，会先尝试删除所有指向该镜像的标签，然后删除该镜像文件本身。

通常并不推荐使用 - f 参数来强制删除一个存在容器依赖的镜像。正确的做法是，先删除依赖该镜像的所有容器，再来删除镜像。

7) 创建镜像

a.基于已有镜像的容器创建：`docker commit`

>
	docker run -it ubuntu: 14. 04 /bin/ bash
	root@ a925cb40b3f0:/# touch test 
	root@ a925cb40b3f0:/# exit
	docker commit -m "Added a new file" -a "Docker Newbee" a925cb40b3f0 test:0.1

b.基于本地模板导入：`docker import`

>
	cat ubuntu- 14. 04- x86_ 64- minimal. tar. gz | docker import - ubuntu: 14. 04

c.基于 Dockerfile 创建

8) 存出和载入镜像

用户可以使用 docker save 和 docker load 命令来存出和载入镜像。

如果要导出镜像到本地文件，可以使用 docker save 命令。例如，导出本地的 ubuntu： 14. 04 镜像为文件 ubuntu_ 14. 04. tar，如下所示：

`docker save -o ubuntu_14.04.tar ubuntu:14.04`

`docker load --input ubuntu_14.04.tar`

9) 上传镜像：`docker push`

>
	docker tag test:latest user/test:latest
	docker push user/test:latest


### 操作Docker容器

容器是Docker的另一个核心概念。简单来说，容器是镜像的一个运行实例。所不同的是，镜像是静态的只读文件，而容器带有运行时需要的可写文件层。如果认为虚拟机是模拟运行的一整套操作系统（包括内核、应用运行态环境和其他系统环境）和跑在上面的应用，那么Docker容器就是独立运行的一个（或一组）应用，以及它们必需的运行环境。

1) 新建容器

`docker create -it ubuntu:latest`

使用 docker create 命令新建的容器处于停止状态，可以使用 docker start 命令来启动它。

2) 启动容器

`docker start af8f4f922daf`

除了创建容器后通过 start 命令来启动，也可以直接新建并启动容器。所需要的命令主要为 docker run ，等价于先执行 docker create 命令，再执行 docker start 命令。

`docker run ubuntu /bin/echo 'Hello world' Hello world`

3) 终止容器

`docker stop ce5`

可以用 docker ps- qa 命令看到所有容器的ID。

4) 重启容器

`docker restart ce5`

5) 进入容器

有多种方法，包括使用官方的 attach 或 exec 命令，以及第三方的 nsenter 工具等。

`docker attach 容器名`

`docker exec -it 243c32535da7 /bin/bash`

6) 删除容器

`docker rm ce5`

如果要直接删除一个运行中的容器，可以添加 -f 参数。Docker 会先发送 SIGKILL 信号给容器，终止其中的应用，之后强行删除。

7) 导入和导出容器

>
	docker export -o test_for_run.tar ce5
	docker import test_for_run.tar - test/ubuntu:v1.0

实际上，既可以使用 docker load 命令来导入镜像存储文件到本地镜像库，也可以使用 docker import 命令来导入一个容器快照到本地镜像库。

8) 容器小结

在生产环境中，因为容器自身的轻量级特性，笔者推荐使用容器时在一组容器前引入HA（High Availability，高可靠性）机制。例如使用HAProxy工具来代理容器访问，这样在容器出现故障时，可以快速切换到功能正常的容器。此外，建议通过指定合适的容器重启策略，来自动重启退出的容器。



### 访问Docker仓库


### PHP环境
docker run -p 172.23.158.57:8080:80 nginx



> 内容来源：杨保华; 戴王剑; 曹亚仑. Docker技术入门与实战（第2版） 