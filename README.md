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

    cat sql-inject.tar | sudo docker import - sql-inject:latest

你应该将'sql-inject.tar'替换成实际的文件名<br>
<br>
使用此镜像制作容器

    sudo docker create --name sql-inject -it sql-inject:latest /bin/sh

启动镜像

    sudo docker start sql-inject

确保此容器已启动

    sudo docker ps

如输出的内容存在容器sql-inject的信息，则容器已启动<br>
<br>
进入容器

    sudo docker exec -it sql-inject /bin/bash

在容器里，你需要手动启动Apache和MySQL 。运行`zsh`可以更方便的输入

    service apache2 start
    service mysql start
 
在你的Linux主机里使用`sudo docker inspect sql-inject | IPAdress`查看容器IP，或者在容器中`ifconfig` <br>
这里假设你的容器IP为 172.17.0.2，在主机里用浏览器访问它（记得改为你自己Docker的IP）

    firefox 172.17.0.2
 
如果可以看到Apache的默认网页，则运行成功 <br>
在浏览器地址栏里修改信息，在末尾添加信息，如'/hello.php'( http://172.17.0.2/hello.php )，可以访问靶场网页文件
，访问以上网页文件可以确认php是否可以使用 <br>
靶场网页文件放在 /var/www/html/ 目录下，你应该将自己写的网页文件放在这里，然后可以用以上方式访问它 <br>
<br>
 
关闭靶场：在主机上

    sudo docker stop sql-inject

如你需要，可以下载网页源码以随时查看此文档
