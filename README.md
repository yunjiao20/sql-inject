# sql-inject

用于学习SQL注入的Docker环境(为了学习制作的，花了不少时间，希望留个备份与分享) <br>
此Docker容器快照使用Docker Ubuntu制作，配置安装了Apache2、MySQL、PHP、zsh，可以在Docker in Linux上快速部署，
为了实践《从零到一：CTFer成长之路》上的SQL注入题目制作<br>
<br>

## 安装与使用

在下载本项目之前，请确保你的Linux上有可用的Docker，Docker的安装与配置此处不赘述<br>
<br>
在release下载Docker容器快照文件，进入包含有此文件的目录，这里假设此文件为sql-inject.tar，
将快照文件制成Docker镜像。

    # 本地主机运行
    cat sql-inject.tar | sudo docker import - sql-inject:latest

你应该将'sql-inject.tar'替换成实际的文件名<br>
<br>
使用此镜像制作容器

    # 本地主机运行
    sudo docker create --name sql-inject -it sql-inject:latest /bin/sh

启动镜像

    # 本地主机运行
    sudo docker start sql-inject

确保此容器已启动

    # 本地主机运行
    sudo docker ps

如输出的内容存在容器sql-inject的信息，则容器已启动<br>
<br>
进入容器

    # 本地主机运行
    sudo docker exec -it sql-inject /bin/bash

在容器里，你需要手动启动Apache和MySQL 。运行`zsh`可以更方便的输入

    # Docker容器内运行
    service apache2 start
    service mysql start

`/`目录下提供了 start.sh 脚本，你也可以使用`./start.sh`快速启动 <br>
在你的Linux主机里使用`sudo docker inspect sql-inject | IPAdress`查看容器IP，或者在容器中`ifconfig` <br>
这里假设你的容器IP为 172.17.0.2，在主机里用浏览器访问它（记得改为你自己Docker的IP）

    # 本地主机运行
    firefox 172.17.0.2

如果可以看到Apache的默认网页，则运行成功 <br>
在浏览器地址栏里修改信息，在末尾添加信息，如'/hello.php'( http://172.17.0.2/hello.php )，可以访问靶场网页文件
，访问以上网页文件可以确认php是否可以使用 <br>
靶场网页文件放在 /var/www/html/ 目录下，你应该将自己写的网页文件放在这里，然后可以用以上方式访问它 <br>
<br>

关闭靶场：在主机上

    # 本地主机运行
    sudo docker stop sql-inject

如你需要，可以下载网页源码以随时查看此文档
<br>
<br>
<br>
<br>
你也可以利用此项目来快速搭建本地服务器，只需要使用

    sudo docker run --name sql-inject -p 80:80 -it sql-inject:lateest /bin/sh

将容器80端口映射到本地80端口即可，然后可以使用apache2，php和mysql，免去下载配置的时间。然后你可以在其他设备上访问这个容器


# 其他

附我的docker命令笔记

    docker

    docker search 镜像               --搜索镜像
    docker pull 仓库名称:[标签]      --获取镜像，默认下载lastest

    docker images                    --查看下载到本地的所有镜像
    docker inspect 镜像ID号          --获取镜像详细信息
    docker rmi 镜像ID号              --彻底删除该镜像（应先删除该镜像的所有容器）
    docker save -o 存储文件名 存储的镜像
                                     --将镜像保存成为本地文件(.tar)
    docker load < 存出的文件         --将镜像文件导入到镜像库中

    docker create 选项 镜像          --创建容器
                                       [选项: -i  让容器开启标准输入接受用户输入
                                              -t  让docker分配一个伪终端tty
                                              -it 和起来，运行一个交互式会话shell]
    docker ps          --查看正在运行的容器
    docker ps -a       --显示所有的容器
    docker start 容器ID/名称         --启动容器
    docker run 镜像                  --创建并启动容器，如果此镜像不存在，从公有仓库下载
    docker run --name 容器名称 -it 镜像 容器命令
    docker run --name 容器名称 -p 本机端口:容器端口 -it 镜像 容器命令(如/bin/sh)          --将容器端口映射到本地端口
                                    如: sudo docker run --name sql-try -p 8080:80 -it sql-inject:latest /bin/zsh 
    docker stop 容器ID/名称          --终止容器运行
    docker exec -it 容器ID/名称 /bin/sh      --docker start容器后，进入运行着的容器
    docker rm 容器ID/名称            --删除终止状态的容器
    docker cp 本地文件路径 容器:文件路径
              容器:文件路径 文件路径         --复制文件到容器中与导出容器文件，如 
                                                       docker cp ~/hello.txt 25291d3fad0fd:/opt/
                                                                 25291d3fad0fd:/opt/abc123.txt ~/hi.txt
    docker export 容器ID/名称 > 文件名
                  -o 文件名 容器ID/名称      --导出容器为容器快照文件(.tar)
    cat 文件名 | docker import - 镜像名称:标签       --导入容器快照，生成镜像
                                                       或 docker import 文件名 -- 镜像名称:标签
                                                                                                   
