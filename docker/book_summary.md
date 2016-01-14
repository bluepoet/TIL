# 1장 Docker

Docker는 하드웨어를 가상화하는 계층이 없기 때문에 메모리 접근, 파일시스템, 네트워크 속도가 가상 머신에 비해 월등히 빠르다.

호스트와 거의 동일한 속도를 낸다.

Docker는 리눅스 커널의 cggroups와 namespaces를 기반으로 하여 이미지, 컨테이너 생성 및 관리 기능과 다양한 부가 기능을 제공합니다.

# 3장 Docker 사용해보기

Docker의 명령은 docker run, docker push와 같이 docker \<명령\> 형식이며, 항상 root 권한으로 실행해야 합니다

### pull 명령으로 이미지 받기

```
sudo docker pull ubuntu:latest
```

### images 명령으로 이미지 목록 출력하기

```
sudo docker images
```

### run 명령으로 컨테이너 생성하기

```
sudo docker run -i -t --name hello ubuntu /bin/bash
```

### ps 명령으로 컨테이너 목록 확인하기

```
sudo docker ps -a
```

### start 명령으로 컨테이너 시작하기

``` 
sudo docker start hello
```

### restart 명령으로 컨테이너 재시작하기

``` 
sudo docker restart hello
```

### attach 명령으로 컨테이너 접속하기

``` 
sudo docker attach hello
```

### exec 명령으로 외부에서 컨테이너 안의 명령 실행하기

``` 
sudo docker exec hello echo "hello world"
```

### stop 명령으로 컨테이너 정지하기

``` 
sudo docker stop hello
```

### rm 명령으로 컨테이너 삭제하기

``` 
sudo docker rm hello
sudo docker ps -a
```

### rmi 명령으로 이미지 삭제하기

``` 
sudo docker rmi ubuntu:latest
```

# 4장 Docker 이미지 생성하기

### build 명령으로 이미지 생성하기

```
sudo docker build --tag hello:0.1
sudo docker run --name hello-nginx -d -p 80:80 -v /root/data:/data hello:0.1
sudo docker ps
```

## 의문

```
로컬 맥에서 docker-machine ssh default로 도커를 띄운다음 nginx이미지를 빌드해 만들어서 run하면 

왜 ifconfig의 docker0, eth0, eth1 ip중 eth1이 적용될까?
```

5장 Docker 살펴보기

history 명령으로 이미지 히스토리 살펴보기

```
docker history hello:0.1
```

cp 명령으로 파일 꺼내기

```
sudo docker cp hello-nginx:/etc/nginx/nginx.conf ./
```

commit 명령으로 컨테이너의 변경사항을 이미지로 생성하기

```
sudo docker commit -a "Foo Bar <foo@bar.com>" -m "add hello.txt" hello-nginx hello:0.2
```

diff 명령으로 컨테이너에서 변경된 파일 확인하기

```
sudo docker diff hello-nginx
```

inspect 명령으로 세부 정보 확인하기

```
sudo dokcer inspect hello-nginx
```


