# Docker 볼륨과 네트워킹
지난 주 복습
예제: href-container clone 받아서
```
# Single demo
$ ./build.sh

# Multi demo

$ vi Dockerfile.multi

$ docker build -t href-container . -f Dockerfile.multi

$ docker images

$ docker run -e url=https://www.alexellis.io/ href-container

$ docker run -e url=https://docker.com href-container



cd href-container
docker build -t href-container . -f Dockerfile.multi
docker run -e url=https://docker.com href-container
```
실행

## 컨테이너 볼륨
컨테이너 stateless

### Volume 장점
- 컨테이너와 데이터 분리
- 컨테이너간 데이터 공유
- I/O 성능 향상
- 호스트와 컨테이너간 파일 공유

### Volume 관리 유형
- 케이스1: 컨테이너 내부에 저장한다.
- 케이스2: 도커 AUFS에 저장한다.
- 케이스3: 도커 호스트 파일 시스템 볼륨 마운트
- 케이스4: volume-driver를 이용해 네트워크로 연결된 장치에 저장한다.

### Volume 주요 명령어
- docker volume create
- docker volume ls
- docker volume inspect
- docker volume rm
```
docker run -it -v test1:/www/test1 ubuntu
```

## 컨테이너 네트워킹

### Networking Model
- 도커가 시작될때 호스트 머신에 docker0라고 부르는 가상 인터페이스를 생선한다.
- docker0에 사설 IP가 랜덤하게 배정된다.

### Network 주요 명령어
- docker network create
- docker network ls
- docker network inspect
- docker network rm
- docker network connect
- docker network disconnect

### network 유형
- bridge: 단일 호스트내에서 컨테이너를 연결해주는 네트워크
    -ex. docker0
- overlay: 멀티 호스트간에 연결해주는 네트워크

### bridge network
- 컨테이너는 동일 호스트내에 위치해야 한다.
- 사용자 정의 bridge 네트워크에 포함된 컨테이너는 컨테이너 이름으로 통신 가능

```
docker run -itd --net=red --name ubuntu1 ubuntu
docker run -itd --net=red --name ubuntu2 ubuntu
docker run -it --net=blue --name ubuntu3 ubuntu
docker run -it --net=blue --name ubuntu4 ubuntu

docker network inspect red
docker network inspect blue

// docker container 접속
docker exec -it 42f91babe025 /bin/bash

```

### overlay network
- 오버레이 네트워크 요건
    - key-value 스토어: Consul, Etcd, Zookeeper
    - key-value 스토어에 연결된 호스트 클러스터
    - 커널 버전 3.16 이상
    - 각 호스트 도커 엔진 설정
- 다른 도커 호스트에서 각각 실행중인 컨테이너들이 서로 통신할수 있게 해준다.
- 도커 엔진은 overlay 네트워크 드라이버를 통해 멀티 호스트 네트워크 지원

```
// mhl-consul 머신 추가
docker-machine create -d virtualbox mhl-consul

// consul 컨테이너 추가
docker $(docker-machine config mhl-consul) run -d -p 8500:8500 -h consul progrium/consul -server -bootstrap

// mhl-demo1 머신을 Consul 클러스터에 추가
docker-machine created -d virtualbox --engine-opt="cluster-sotre=consul://$(docker-machine ip mhl-consul):8500" --engine-opt="cluster-advertise=eth1:0" mhl-demo1
```

## Docker Swam
- 도커 호스트 클러스터를 구성하고, 클러스터에 컨테이너를 배치해주는 도구
 - 표준 Docker API
 - 스케쥴링 지원
 - 디스커버리 지원: Consul, Etcd, ZooKeeper

거의 Deprecated 그래서 바로 swarm mode로

## Swarm mode
- 도커 엔진에 클러스터 관리가 통합된 모드
특징
- 멀티 호스트 네트워킹: 분산 환경에서 여러개 노드를 하나의 네트워크로 묶은것
- 서비스 디스커버리: 멀티 호스트 환경에서 실행된 컨테이너 정보를 제공
- 로드밸런싱: 대용량 트래픽을 분산해 주는것
- 롤링 업데이트: 새로운 이미지를 순차적으로 업데이트 해 주는것
- Health Check: 서비스가 정상 상태인지 확인
- 스케일 아웃: 컨테이너를 원하는 상태로 관리, Desired state reconciliation
- 로깅
- 모니터링
- HA

실습 3-17  

```
swarm init 자기 ip
```

## Kubernetes
- 이름의 기원: 그리스어로 '조타수', '조종사'
- aka 'k8s'
- 컨테이너 어플리케이션 배포, 스케일링, 관리를 자동화 해주는 오픈소스 시스템

### 클라이언트
- kubectl: 명령어 방식
- dashboard: 웹 서비스

kubectl run hello-node --image=hello-node:v1 --port=8080
kubectl get deployments
kubectl get pods
kubectl config view

// 다른 서버 fc-docker 든 어떤거든
eval $(docker-machine env fc-docker)

kubectl describe svc kubernetes-dashboard -n kube-system

# 연습

# 연습