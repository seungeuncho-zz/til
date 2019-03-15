# Docker 이해
## Docker는 무엇인가?
리눅스컨테이너 기술을 이용해 어플리케이션 패키징, 배포를 지원하는 경량의 가상화 플랫폼

## 왜 도커를 써야 할까?
다양한 환경에서 많은 어플리케이션과 서버를 사용할때 그만큼의 config를 설정해야함  
규격화된 상자에 넣어서 가지고 다니자.  
x*y만큼 해야하는 일을 1번으로 줄이자.  
어느 환경에서도 실행 가능.  

## 하이퍼바이저 (hypervisor)
호스트 컴퓨터에서 다수의 운영체제를 동시에 실행하기 위한 논리적 플랫폼  

aka 가상화 머신 모니터, 가상화 머신 매니저  

Hypervisor  
Infrastructure -> Hypervisor -> vm (guestOs-> Bins/Libs-> app B)  

Contatiner
Infrastructure -> HostOS-> docker -> contatiner
Hypervisor와는 다르게 GuestOS가 없음  
vm이 주택 contatiner 아파트  

## 도커는 가볍다
VM에 비해 이미지 파일 크기가 작아서 빠르게 이미지를 만들고 실행할 수 있다.

## Immutable Infrastructure
이미지 기반의 어플리케이션 배포 패러다임  
많은 서버를 동적으로 관리하는 클라우드 환경에서 효과적이고 유연한 배포 방식  
ec2-> ami 이미지를 찍어놓는거  

### 특징
- 관리 가능하다
- stateless 하다. (언제 사라져도 괜찮다)
- scalable (줄였다 늘렸다 가능)
- 이미지 기반
- 어플리케이션 배포

## 리눅스 컨테이너(LXC, Linux Container)
호스트 서버 (리눅스) 안에 독립된 리눅스 시스템을 만들어서 하나의 리눅스 처럼 쓰고 싶은 요구 -> 만들어 놓은게 LXC  
단일 리눅스 호스트상에서 여러개의 격리된(isolated 리눅스 시스템을 실행하기 위한 OS 수준의 가상화  
도커가 lxc 를 쓰는것 (도커가 기반으로 개발된 오픈소스 시스템)

## 도커는 왜 성공했을까?
lxc는 설정 복잡하고 사용하기 어려움  
도커는 사용하기 쉽다.  

Swam -> docker에서 만들었다.
Kubernetes -> google 에서 만들었다.

## Docker Client
- 도커 엔진과 통신하는 프로그램
- 맥 - 하이퍼바이저로 사용한다. (가상 머신을 깔고 그 위에서 실행 바로 사용 못함)
- Kitemetic - 템플릿 기반으로 도커 컨테이너를 관리할 수 있게 해주는 GUI 도커 클라이언트
- Rest API를 호출하는 것이 docker client

## Docker Engine
- 어플리케이션을 컨테이너로 만들고 실행하게 해주는 데몬
- Swarm, Kubernetes 와 통합

## Docker Machine
- 로컬, 리모트 서버에 도커 엔진을 설치하고, 설정을 자동으로 해주는 프로비저닝 클라이언트

## Docker Hub
- 도커 이미지를 관리하는 저장소
- 오픈소스 공식 이미지 관리

## Docker Data Center
- DUCP + DTR

## Docker Cloud
- 도커 이미지, 컨테이너 관리를 지원

## 도커 실행하기
``` docker version ```
실행시 Client , Server가 둘 다 띄어져 있는지 확인

``` docker info ```
docker server 의 정보

``` docker run hello-world```
hello-world 라는 컨테이너를 실행해라
run 컨테이너 실행
hello-world 컨테이너가 없으면 컨테이너를 다운 받아 온다.

한개의 client 가 한개의 host에 통신
Namespace: 프로세스별 리소스 격리
Cgroups: 리소스 관리

도커는 client랑 통신할때 tcp를 사용 안함.
unix소켓과 데몬이 바인딩 함.
docker Rest Api - 외부에서 rest api 를 쓰려면 열어야 함.


## 도커 이미지
- 컨테이너를 만들기 위한 템플릿 
- 붕어빵 틀 같은 것
- 읽기 전용 파일 시스템으로 도커가 애플리케이션을 배포하는 단위.
- 레이어 파일 시스템으로 각 파일시스템이 곧 이미지
    - 여러개 레이어 혹은 단일 레이어로 구축 가능
    - 아래의 레이어는 read-only로. 아래의 레이어가 변경 되면 안됨
- 파일 시스템 모드
    - read-only
    - read-write
- bootfs
    - 도커 엔진이 사용하는 파일시스템
    - root 파일 시스템 레이어 (OS 설치 영역)
    - read-only 모드

# 베이스 이미지
- 이미지의 부모가 되는 이미지

어플리케이션 컨테이너 띄우기위해 만든 이미지
수정해서 이미지를 다시 만들면 일부만 바뀐것이기 때문에
일부만 이미지를 만든다. 그것의 id를 만든다. 그것만 따로 저장소에 올린다.
다시 pull 하면 다른 부분만 가져온다.

## Image Layers
- 도커 이미지 계층구조를 확인할 수 있다.

## Dockerfile
- 도커 이미지를 만들기 위해 설치할 SW, 필요한 설정을 정의하는 파일
- 이미지 제작 과정을 투명하게 한다.

```docker search whalesay
docker pull docker/whalesay
docker images
```  
```docker pull docker/whalesay```
레이어가 여러개면 여러개를 다운로드 받음
맨 앞에 있는 것은 id
``` docker ps ```
현재 실행중인 것
``` docker ps -a ```
실행 되었다가 종료 된 것

``` RUN apt-get -y update && apt-get install -y fortune ```
chaning으로 쓰는 것 과 chaning으로 쓰지 않는게 RUN 갯수 정도로 이미지가 만들어지기 때문에 간단한 것은 RUN 을 여러개 안하고 한번에 하는게 나음  
때에 따라 다름  
업데이트 하는것은 별개로 하는 것이 좋음
복수의 명렁어 실행 시에 RUN 명령어 순서는 상관있음

## 도커 이미지 크기를 줄이는 방법
- 가벼운 베이스 이미지를 사용한다.
- Dockerfile 명령을 체인으로 사용한다.
- 빌드 도구를 설치하지 않는다.
- 패키지 관리자를 정리한다.
    - apt clean

## 도커 컨테이너
- 이미지를 실행한 상태
    - 읽기/쓰기가 가능한 파일 시스템(read-write layer)
- 실행된 독립 애플리케이션
    - 컨테이너는 가상서버가 아니다.
        - 실제는 프로세스 pid 를 가지고 있음 죽일수 있음
        - 가상서버라면 ssh로 들어가서 바꿀수 있지만 컨테이너는 그게 안됨
- 이미지는 read-only 컨테이너는 read-write. writable 하면 컨테이너

# 실습 
``` docker run -e MYSQL_ROOT_PASSWORD='Passw0rd' --name mydb -d mysql:5.6 ```
환경변수 정의는 -e
한번 뜨고 죽으면 안되고 데몬모드여야 하기 때문에 -d

!(실습)[/Users/joseongeun/Desktop/docker1/mysql-1.png]
docker rm $(docker ps -al)
도커 컨테이너 다 지울수 있음 

``` docker run --name myweb -d -p 80:80 nginx ```
80:80
왼쪽 외부로 서비스할때 포트(host) 오른쪽이 컨테이너 내부 포트(container)

외부 80 으로 오면 내부 80으로 바인딩 하겠다.
도커 컨테이너 이름은 유일해야함.
같은 포트로 또 런 하려고 하면 에러.
이미 80이 있기 때문에 안됨.
!(실습)[/Users/joseongeun/Desktop/docker1/nginx-1.png]
!(실습)[/Users/joseongeun/Desktop/docker1/nginx-error.png]

``` docker run --name myweb -d -P nginx ```
도커가 자동으로 열러있는 포트와 바인딩 함.
!(실습)[/Users/joseongeun/Desktop/docker1/nginx-2.png]

## VM 활용
### Docker Machine
- 가상서버에 도커엔진을 설치할 수 있게 도와주는 도구

## Vagrant
- 다양한 가상머신을 동일한 방식으로 만들고 관리할 수 있게 해주는 도구
- 도커머신과의 장단점이 있음

Q. docker build 하면 어디에 올라가나?
Q. eb와 docker

## 컨테이너 활용

## 실습

## 복습

## 실습
