# RK3288 Docker Env

## 参考文档

* [使用Docker编译Android系统源码](https://blog.csdn.net/godiors_163/article/details/59069108)
* [【Docker】保存镜像、 容器到本地， 从本地加载镜像、 容器](https://blog.csdn.net/renhanchi/article/details/75898857)
* [Use bind mounts](https://docs.docker.com/storage/bind-mounts/)
* [docker commit](https://docs.docker.com/engine/reference/commandline/commit/)

## Docker Pull Ubuntu 14.04

```Shell
zengjf@zengjf:~/zengjf/gpg$ sudo docker pull ubuntu:14.04
14.04: Pulling from library/ubuntu
324d088ce065: Pull complete
2ab951b6c615: Pull complete
9b01635313e2: Pull complete
04510b914a6c: Pull complete
83ab617df7b4: Pull complete
Digest: sha256:b8855dc848e2622653ab557d1ce2f4c34218a9380cceaa51ced85c5f3c8eb201
Status: Downloaded newer image for ubuntu:14.04
zengjf@zengjf:~/zengjf/gpg$ sudo docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
ubuntu                    14.04               8cef1fa16c77        6 days ago          223MB
[...省略]
zengjf@zengjf:~/zengjf/gpg$
```

## 安装依赖包

* `sudo apt-get update`
* `sudo apt-get install git-core gnupg flex bison gperf libsdl-dev libesd0-dev build-essential zip curl libncurses5-dev zlib1g-dev libxml2-utils genromfs lsb-core libc6-dev-i386 g++-multilib lib32z1-dev lib32ncurses5-dev u-boot-tools android-tools-fastboot Texinfo python software-properties-common bc lzop`
* `sudo apt-get install openjdk-7-jdk`
* `java -version`
* `gcc -version`
* `sudo apt-get install gcc-4.4 g++-4.4 g++-4.4-multilib`
* `sudo mv /usr/bin/gcc /usr/bin/gcc.bk`
* `sudo ln -s /usr/bin/gcc-4.4 /usr/bin/gcc`
* `sudo mv /usr/bin/g++ /usr/bin/g++.bk`
* `sudo ln -s /usr/bin/g++-4.4 /usr/bin/g++`
* `gcc --version`
* `sudo apt-get install build-essential make libc6-dev texinfo libncurses-dev git-core gnupg flex bison zip curl ncurses-dev libsdl-dev zlib1g-dev lib32ncurses5 libxml2-utils lzma gperf python software-properties-common bc lzop`

## Docker save and remove
* `sudo docker run -v `pwd`:/root/android -it ubuntu:14.04 /bin/bash`: 在这种情况下安装好需要的依赖软件；
* 另外开一个终端，保存当前的docker环境：
  * `sudo docker ps`
    ```
    aplex@aplex:~$ sudo docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    451458d9596c        ubuntu:14.04        "/bin/bash"         About an hour ago   Up About an hour                        eloquent_turing
    aplex@aplex:~$
    ```
  * `sudo docker commit -m "android-build" 451458d9596c android-build:v1.0`
  * `sudo docker images`
    ```
    aplex@aplex:~$ sudo docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    android-build             v1.0                016729213337        3 minutes ago       779MB
    ubuntu                    14.04               8cef1fa16c77        9 days ago          223MB
    opensourcebikeshare_web   latest              02d06b104888        6 weeks ago         344MB
    mysql                     latest              5195076672a7        7 weeks ago         371MB
    ubuntu                    latest              f975c5035748        2 months ago        112MB
    aplex@aplex:~$
    ```
  * `sudo docker rmi 8e61b759ab52` 
    ```
    aplex@aplex:~$ sudo docker rmi 8e61b759ab52
    Untagged: local:v1.0
    Deleted: sha256:8e61b759ab52066cbfc40ec4871dc357c3903fc5893ec39826823e3f7994b4d7
    aplex@aplex:~$
    ```

## 合成源代码包

* 当前源代码：
  ```
  zengjg@zengjg:~/zengjf/android$ ls -alh
  total 13G
  drwxrwxr-x  3 zengjg zengjg 4.0K 5月   7 09:23 .
  drwxrwxr-x  8 zengjg zengjg 4.0K 5月   4 09:10 ..
  -rw-r--r--  1 zengjg zengjg  97M 5月   4 17:14 g3288-kernel-20180404.tar.bz2
  drwxrwxr-x 30 zengjg zengjg 4.0K 5月  16  2017 g3288-v51-v2-20170512
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:17 g3288-v51-v2-20170512_aa
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:17 g3288-v51-v2-20170512_ab
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:19 g3288-v51-v2-20170512_ac
  -rw-r--r--  1 zengjg zengjg 1.4G 5月   4 17:19 g3288-v51-v2-20170512_ad
  -rw-r--r--  1 zengjg zengjg   80 5月   4 17:19 readme.txt
  ```
* `cat g3288-v51-v2-20170512_* > g3288-v51-v2-20170512.tar.bz2`
* 合成后源代码：
  ```
  zengjg@zengjg:~/zengjf/android$ ls -alh
  total 13G
  drwxrwxr-x  3 zengjg zengjg 4.0K 5月   7 09:23 .
  drwxrwxr-x  8 zengjg zengjg 4.0K 5月   4 09:10 ..
  -rw-r--r--  1 zengjg zengjg  97M 5月   4 17:14 g3288-kernel-20180404.tar.bz2
  drwxrwxr-x 30 zengjg zengjg 4.0K 5月  16  2017 g3288-v51-v2-20170512
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:17 g3288-v51-v2-20170512_aa
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:17 g3288-v51-v2-20170512_ab
  -rw-r--r--  1 zengjg zengjg 1.6G 5月   4 17:19 g3288-v51-v2-20170512_ac
  -rw-r--r--  1 zengjg zengjg 1.4G 5月   4 17:19 g3288-v51-v2-20170512_ad
  -rw-rw-r--  1 zengjg zengjg 6.0G 5月   7 09:19 g3288-v51-v2-20170512.tar.bz2
  -rw-r--r--  1 zengjg zengjg   80 5月   4 17:19 readme.txt
  ```
* `tar xvf g3288-v51-v2-20170512.tar.bz2`

## Docker Mount Android Source

* ``sudo docker run -v `pwd`:/root/android -it ubuntu:14.04 /bin/bash``
  ```
  zengjf@zengjf:~/zengjf/android$ ls
  g3288-kernel-20180404.tar.bz2  g3288-v51-v2-20170512  g3288-v51-v2-20170512.tar.bz2  readme.txt
  zengjg@zengjg:~/zengjf/android$ pwd
  /home/zengjg/zengjf/android
  zengjg@zengjg:~/zengjf/android$ sudo docker run -v `pwd`:/root/android -it ubuntu:14.04 /bin/bash
  root@43db6b44e4a4:/# ls root/android/
  g3288-kernel-20180404.tar.bz2  g3288-v51-v2-20170512  g3288-v51-v2-20170512.tar.bz2  readme.txt
  root@43db6b44e4a4:/#
  ```

## Compile

```
root@0473cbdfa57e:~/android/g3288-v51-v2-20170512# ./mk.sh -h
Usage: build.sh [OPTION]
Build script for compile the source of telechips project.

  -j=n                 using n threads when building source project (example: -j=16)
  -u, --uboot          build bootloader uboot from source
  -k, --kernel         build kernel from source
  -s, --system         build android file system from source
  -h, --help           display this help and exit
```

### U-Boot Compile

```
root@0473cbdfa57e:~/android/g3288-v51-v2-20170512# ./mk.sh -u -j=4
[...省略]
  LD      u-boot
  OBJCOPY u-boot.bin
  OBJCOPY u-boot.srec
./tools/boot_merger --subfix ".10.bin" ./tools/rk_tools/RKBOOT/RK3288.ini
out:RK3288UbootLoader.bin
fix opt:RK3288UbootLoader_V2.30.10.bin
merge success(RK3288UbootLoader_V2.30.10.bin)
'/root/android/g3288-v51-v2-20170512/uboot/RK3288UbootLoader_V2.30.10.bin' -> '/root/android/g3288-v51-v2-20170512/out/release/RK3288UbootLoader_V2.30.10.bin'
^_^ uboot path: /root/android/g3288-v51-v2-20170512/out/release/RK3288UbootLoader_V2.30.10.bin
```

### Kernel Compile

```
root@0473cbdfa57e:~/android/g3288-v51-v2-20170512# ./mk.sh -k -j=4
[...省略]
  LD      arch/arm/boot/compressed/vmlinux
  OBJCOPY arch/arm/boot/zImage
  Kernel: arch/arm/boot/zImage is ready
  Image:  kernel.img is ready
Pack to resource.img successed!
  Image:  resource.img (with g3288.dtb logo.bmp ) is ready
'/root/android/g3288-v51-v2-20170512/kernel/kernel.img' -> '/root/android/g3288-v51-v2-20170512/out/release/kernel.img'
'/root/android/g3288-v51-v2-20170512/kernel/resource.img' -> '/root/android/g3288-v51-v2-20170512/out/release/resource.img'
```

### Filesystem Compile

```
root@0473cbdfa57e:~/android/g3288-v51-v2-20170512# ./mk.sh -k -j=4
[...省略]
```
