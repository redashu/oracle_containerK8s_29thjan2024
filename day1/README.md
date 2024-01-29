### Understanding problem with bare-metal server

<img src="bare.png">

### multiple app compatibality problem solved by HYpervisor -- Vm 

<img src="vm.png">

### problem with VM 

<img src="vm1.png">

### Introduction to containers 

<img src="cre.png">

### introduction to container runtimes 

<img src="cre1.png">

### we are going with Docker software out of many options available 

<img src="install.png">

### Installing docker-engine in oracle linux 7

```
[opc@docker-server ~]$ sudo yum install docker-engine 
Failed to set locale, defaulting to C
Loaded plugins: langpacks, ulninfo
ol7_MySQL80                                                                                             | 3.0 kB  00:00:00     
ol7_MySQL80_connectors_community                                                                        | 2.9 kB  00:00:00     
ol7_MySQL80_tools_community                                                                             | 2.9 kB  00:00:00     
ol7_UEKR6                                                
```

### starting docker service

```
[root@docker-server ~]# systemctl start docker
[root@docker-server ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2024-01-29 06:17:11 GMT; 6s ago
     Docs: https://docs.docker.com
 Main PID: 25468 (dockerd)
    Tasks: 9
   Memory: 38.2M
   CGroup: /system.slice/docker.service
           └─25468 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Jan 29 06:17:10 docker-server dockerd[25468]: time="2024-01-29T06:17:10.820897171Z" level=warning msg="Your kernel does not support cgroup blkio weight"
Jan 29 06:17:10 docker-server dockerd[25468]: time="2024-01-29T06:17:10.820925931Z" level=warning msg="Your kernel does not support cgroup blkio ...t_device"
Jan 29 06:17:10 docker-server dockerd[25468]: time="2024-01-29T06:17:10.821525490Z" level=info msg="Loading containers: start."
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.066427106Z" level=info msg="Default bridge (docker0) is assigned with an ... address"
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.198903738Z" level=info msg="Loading containers: done."
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.235504041Z" level=warning msg="Not using native diff for overlay2, this m...=overlay2
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.235760361Z" level=info msg="Docker daemon" commit=9bb540d graphdriver(s)=....03.11-ol
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.235916200Z" level=info msg="Daemon has completed initialization"
Jan 29 06:17:11 docker-server systemd[1]: Started Docker Application Container Engine.
Jan 29 06:17:11 docker-server dockerd[25468]: time="2024-01-29T06:17:11.274943219Z" level=info msg="API listen on /var/run/docker.sock"
Hint: Some lines were ellipsized, use -l to show in full.

### auto-start after reboot 
[root@docker-server ~]# systemctl enable  docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@docker-server ~]# 

```

### by default root user in linux can check 

```
[root@docker-server ~]# docker  version 
Client: Docker Engine - Community
 Version:           19.03.11-ol
 API version:       1.40
 Go version:        go1.16.4
 Git commit:        9bb540d
 Built:             Fri Jul 23 01:32:32 2021
 OS/Arch:           linux/arm64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11-ol
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.16.4
  Git commit:       9bb540d
  Built:            Fri Jul 23 01:31:44 2021
  OS/Arch:          linux/arm64
  Experimental:     false
  Default Registry: docker.io
 containerd:
  Version:          v1.4.8
  GitCommit:        7eba5930496d9bbe375fdf71603e610ad737d2b2
 runc:
  Version:          1.1.7
  GitCommit:        860f061
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```

### lets try with non root user 

```
[ashu@docker-server ~]$ whoami
ashu
[ashu@docker-server ~]$ docker  version 
Client: Docker Engine - Community
 Version:           19.03.11-ol
 API version:       1.40
 Go version:        go1.16.4
 Git commit:        9bb540d
 Built:             Fri Jul 23 01:32:32 2021
 OS/Arch:           linux/arm64
 Experimental:      false
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.40/version": dial unix /var/run/docker.sock: connect: permission denied
[ashu@docker-server ~]$ 


```

### Fixing this issues or allow users to connect docker server

```
usermod -aG docker ashu
```

### lets logout that user and login again 

```
[ashu@docker-server ~]$ whoami
ashu
[ashu@docker-server ~]$ docker  version 
Client: Docker Engine - Community
 Version:           19.03.11-ol
 API version:       1.40
 Go version:        go1.16.4
 Git commit:        9bb540d
 Built:             Fri Jul 23 01:32:32 2021
 OS/Arch:           linux/arm64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11-ol
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.16.4
  Git commit:       9bb540d
  Built:            Fri Jul 23 01:31:44 2021
  OS/Arch:          linux/arm64
  Experimental:     false
  Default Registry: docker.io
 containerd:
  Version:          v1.4.8
  GitCommit:        7eba5930496d9bbe375fdf71603e610ad737d2b2
 runc:
  Version:          1.1.7
  GitCommit:        860f061
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```

### Docker images and containers 

<img src="cnt1.png">

### pulling images from docker hub 

```
[ashu@docker-server ~]$ whoami
ashu
[ashu@docker-server ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ docker  pull  alpine 
Using default tag: latest
Trying to pull repository docker.io/library/alpine ... 
latest: Pulling from docker.io/library/alpine
bca4290a9639: Pull complete 
Digest: sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
Status: Downloaded newer image for alpine:latest
alpine:latest
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              ace17d5d883e        2 days ago          7.73MB
[ashu@docker-server ~]$ 


```

### pulling python latest image

```
[ashu@docker-server ~]$ docker  pull python
Using default tag: latest
Trying to pull repository docker.io/library/python ... 
latest: Pulling from docker.io/library/python
5665c1f9a9e1: Pull complete 
f419b1a62fc8: Pull complete 
76b4f1810f99: Pull complete 
1c176cbf6497: Pull complete 
ba0d9396537e: Pull complete 
f93cd5cfd8e8: Pull complete 
cac9244624e2: Pull complete 
84ab309da70c: Pull complete 
Digest: sha256:a09f71f4af992ddf9a620330fed343c850c371251be45c3f9bb46ebeca49c9c6
Status: Downloaded newer image for python:latest
python:latest
[ashu@docker-server ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              ace17d5d883e        2 days ago          7.73MB
python              latest              33039c2f184f        5 weeks ago         1.02GB

```

### Creating first container with default process

```
[ashu@docker-server ~]$ docker  run  --name  ashuc1  -it  -d   alpine:latest 
ee8b471b6af84e007980650286461776d7007375e423c8113d861d17cf6ace4c


[ashu@docker-server ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ee8b471b6af8        alpine:latest       "/bin/sh"           3 seconds ago       Up 2 seconds                            ashuc1
[ashu@docker-server ~]$ 


```


### checking resouce consumption of container 

```
docker  stats ashuc1
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
ee8b471b6af8        ashuc1              0.00%               2.625MiB / 14.85GiB   0.02%               1.46kB / 0B         1.38MB / 0B         1
^C

```

### to exit from above state -- use contr + c 

### stop container and verify 

```
[ashu@docker-server ~]$ docker  stop  ashuc1
ashuc1
[ashu@docker-server ~]$ docker  ps
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS              PORTS               NAMES
99657f196402        alpine:latest        "/bin/sh"           2 minutes ago       Up 2 minutes                            sandhya1
b66d6b47ca0e        oraclelinux:9-slim   "/bin/bash"         3 minutes ago       Up 3 minutes                            chinta_sunil
30d31faf24f8        alpine:latest        "/bin/sh"           3 minutes ago       Up 3 minutes                            anant1
13ee51ce341a        alpine               "/bin/sh"           4 minutes ago       Up 4 minutes                            sunil
58d3dd491ed3        alpine:latest        "watch -n 5 ls"     5 minutes ago       Up 5 minutes                            dhara2
baf9bd6f7224        alpine               "/bin/sh"           5 minutes ago       Up 5 minutes                            happy_greider
3c262db87829        alpine:latest        "/bin/sh"           5 minutes ago       Up 5 minutes                            vishalc1
8f1d9d4763d6        alpine:latest        "/bin/sh"           5 minutes ago       Up 5 minutes                            kash1
6875c97197db        alpine               "/bin/sh"           5 minutes ago       Up 5 minutes                            rachana1
2208a6d4a41e        alpine:latest        "/bin/sh"           7 minutes ago       Up 7 minutes                            prashanth1
89aec282a3f8        alpine:latest        "/bin/sh"           7 minutes ago       Up 7 minutes                            dhara1
[ashu@docker-server ~]$ 

```

### starting an exited container 

```
[ashu@docker-server ~]$ docker  ps  -a
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS                            PORTS               NAMES
99657f196402        alpine:latest        "/bin/sh"           3 minutes ago       Exited (137) 40 seconds ago                           sandhya1
b66d6b47ca0e        oraclelinux:9-slim   "/bin/bash"         4 minutes ago       Exited (137) 10 seconds ago                           chinta_sunil
30d31faf24f8        alpine:latest        "/bin/sh"           4 minutes ago       Exited (137) 34 seconds ago                           anant1
af7d8abe2f45        oraclelinux:9-slim   "/bin/bash"         5 minutes ago       Exited (0) 5 minutes ago                              chinta
13ee51ce341a        alpine               "/bin/sh"           5 minutes ago       Exited (137) 10 seconds ago                           sunil
58d3dd491ed3        alpine:latest        "watch -n 5 ls"     5 minutes ago       Exited (137) 9 seconds ago                            dhara2
baf9bd6f7224        alpine               "/bin/sh"           6 minutes ago       Up 6 minutes                                          happy_greider
3c262db87829        alpine:latest        "/bin/sh"           6 minutes ago       Exited (137) 31 seconds ago                           vishalc1
8f1d9d4763d6        alpine:latest        "/bin/sh"           6 minutes ago       Up 6 minutes                                          kash1
6875c97197db        alpine               "/bin/sh"           6 minutes ago       Up 6 minutes                                          rachana1
7d8e2aef532a        oraclelinux:9-slim   "/bin/bash"         7 minutes ago       Exited (0) 7 minutes ago                              charming_dubinsky
17d1f2ea14c7        oraclelinux:7.9      "/bin/bash"         7 minutes ago       Exited (137) 59 seconds ago                           anant
2208a6d4a41e        alpine:latest        "/bin/sh"           8 minutes ago       Exited (137) 47 seconds ago                           prashanth1
89aec282a3f8        alpine:latest        "/bin/sh"           8 minutes ago       Exited (137) 22 seconds ago                           dhara1
ee8b471b6af8        alpine:latest        "/bin/sh"           9 minutes ago       Exited (137) About a minute ago                       ashuc1
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ docker  start  ashuc1
ashuc1
[ashu@docker-server ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
baf9bd6f7224        alpine              "/bin/sh"           6 minutes ago       Up 6 minutes                            happy_greider
8f1d9d4763d6        alpine:latest       "/bin/sh"           7 minutes ago       Up 7 minutes                            kash1
6875c97197db        alpine              "/bin/sh"           7 minutes ago       Up 7 minutes                            rachana1
ee8b471b6af8        alpine:latest       "/bin/sh"           9 minutes ago       Up 2 seconds                            ashuc1
[ashu@docker-server ~]$ 

```

### we can get shell access of a running container using exec 

```
[ashu@docker-server ~]$ docker   exec  -it  ashuc1  sh 
/ # 
/ # 
/ # whoami
root
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # mkdir  helloashutoshh
/ # ls
bin             etc             home            media           opt             root            sbin            sys             usr
dev             helloashutoshh  lib             mnt             proc            run             srv             tmp             var
/ # exit
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ 

```

### removing a contaienr 

```
[ashu@docker-server ~]$ docker  stop ashuc1
ashuc1
[ashu@docker-server ~]$ docker  rm  ashuc1
ashuc1
[ashu@docker-server ~]
```

### creating container with custom process

```
[ashu@docker-server ~]$ docker  run -it -d --name ashuc2  alpine  date 
6855f51f7ef4ff759a027dd6895bbdcbadafd2d9980b6b6950ed80ce2c05b23f


[ashu@docker-server ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$


[ashu@docker-server ~]$ docker  ps -a
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS                      PORTS               NAMES
6855f51f7ef4        alpine               "date"              16 seconds ago      Exited (0) 15 seconds ago                       ashuc2
7d8e2aef532a        oraclelinux:9-slim   "/bin/bash"         15 minutes ago      Exited (0) 15 minutes ago                       charming_dubinsky
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$

[ashu@docker-server ~]$ docker  logs  ashuc2
Mon Jan 29 07:15:04 UTC 2024
[ashu@docker-server ~]$


[ashu@docker-server ~]$ docker rm  ashuc2
ashuc2
[ashu@docker-server ~]$ 

```

### application containerization -- using dockerfile 

<img src="appc.png">

## python script containerization 

### hello.py code 

```
import time

while True:
    print("Hello all , welcome to COntainer by Docker..!!")
    time.sleep(3)
    print("Welcome to Oracle India ..")
    time.sleep(2)
    print("this is ashutoshh singh ..!!")
    print("______________________")
    time.sleep(3)
```

### Dockerfile 

```
FROM python
# we are trying to use python image from Docker hub 
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com 
# label is optional keyword but used for sharing image creator info
RUN mkdir /ashucode 
# to run any command during image build time 
COPY hello.py /ashucode/hello.py 
# during build time my code is getting copied into image 
CMD ["python","/ashucode/hello.py"]
# to define default process of this image 

```
### building docker image 

```
[ashu@docker-server ashu-app-containerization]$ ls
ashu-java  ashu-python
[ashu@docker-server ashu-app-containerization]$ ls  ashu-python/
Dockerfile  hello.py
[ashu@docker-server ashu-app-containerization]$ ls
ashu-java  ashu-python
[ashu@docker-server ashu-app-containerization]$ docker  build  -t  ashupython:v1   ashu-python/ 
Sending build context to Docker daemon  3.072kB
Step 1/6 : FROM python
 ---> 33039c2f184f
Step 2/6 : LABEL name=ashutoshh
 ---> Running in ef4fbd7075b7
Removing intermediate container ef4fbd7075b7
 ---> 2825f7fa6d36
Step 3/6 : LABEL email=ashutoshh@linux.com
 ---> Running in dcb1a6d68f53
Removing intermediate container dcb1a6d68f53
 ---> afd999596f52
Step 4/6 : RUN mkdir /ashucode
 ---> Running in ff58e46bb4d1
Removing intermediate container ff58e46bb4d1
 ---> ce18e4a4edb5
Step 5/6 : COPY hello.py /ashucode/hello.py
 ---> bc911bac571c
Step 6/6 : CMD ["python","/ashucode/hello.py"]
 ---> Running in d3c8c919bb7d
Removing intermediate container d3c8c919bb7d
 ---> 90fefe2036f0
Successfully built 90fefe2036f0
Successfully tagged ashupython:v1
```

### creating container and checking python code output 

```
[ashu@docker-server ashu-app-containerization]$ docker  run -itd  --name ashupyc1  ashupython:v1 
110a0c008e86eeb878d6bb94ceb91667271db454cab2121d149bed4d5e58aaa6
[ashu@docker-server ashu-app-containerization]$ docker  ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
110a0c008e86        ashupython:v1       "python /ashucode/he…"   2 seconds ago       Up 1 second                             ashupyc1
[ashu@docker-server ashu-app-containerization]$ docker logs  ashupyc1
Hello all , welcome to COntainer by Docker..!!
Welcome to Oracle India ..
this is ashutoshh singh ..!!
______________________
Hello all , welcome to COntainer by Docker..!!
[ashu@docker-server ashu-app-containerization]$ docker logs -f  ashupyc1
Hello all , welcome to COntainer by Docker..!!
Welcome to Oracle India ..
this is ashutoshh singh ..!!
______________________
Hello all , welcome to COntainer by Docker..!!
Welcome to Oracle India ..
this is ashutoshh singh ..!!
______________________
Hello all , welcome to COntainer by Docker..!!
Welcome to Oracle India ..
this is ashutoshh singh ..!!
______________________
```


