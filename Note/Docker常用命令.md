# Docker常用命令

* sudo service docker restart : 重启 Docker 服务
* docker image ls : 列出本机的所有 image 文件
* docker image rm [imageName] : 删除 image 文件
* docker container kill [containID] : 手动终止
* docker container ls : 列出本机正在运行的容器
* docker container ls --all : 列出本机所有容器，包括终止运行的容器
* docker container rm [containerID] : 删除容器文件
* docker image build : 创建 image 文件
* docker container run : 从 image 文件生成容器,并具有自动抓取 image 文件的功能,如果发现本地没有指定的 image 文件，就会从仓库自动抓取。
* docker container start : 启动已经生成、已经停止运行的容器文件
* docker container stop : 终止容器运行,并自行进行收尾清理工作
* docker container logs : 查看 docker 容器的输出
* docker container exec : 进入一个正在运行的 docker 容器
* docker container cp : 从正在运行的 Docker 容器里面，将文件拷贝到本机