# Docker로 꾸미는 프로젝트 환경
## Docker Compose
여러개 컨테이너 구성된 어플리케이션을 만들고 관리할수 있게 해주는 도구  
- 명령어 기반의 인터페이스
- docker-compose.yml
- 단일서버 기준

## 도커 컴포즈 주요 명령어
- docker-compose up 어플리케이션을 컴포저로 실행한다.
- docker-compose up -d 데몬 모드로 실행한다.
- docker-compose up -d no-create
- docker-compose ps
- docker-compose down
- docker-compose rm

## link의 원리
link를 걸면 도커 엔진이 컨테이너에 환경변수로 필요한 정보를 주입 시킨다.

## 도커 허브(docker Hub)
- 도커 이미지 공개 저장소

```docker build -t docker-whale:lates .
docker images
```
docker images에서 tag name 찾기


```
$ docker login
$ docker tag 9cbc7ae91606 sewoo0920/docker-whale:latest
$ docker push sewoo0920/docker-whale:latest
The push refers to repository [docker.io/sewoo0920/docker-whale]
4ffd14857f23: Pushing [==============================>                    ]  16.84MB/28.05MB
5f70bf18a086: Mounted from docker/whalesay
d061ee1340ec: Mounted from docker/whalesay
d511ed9e12e1: Mounted from docker/whalesay
091abc5148e4: Mounted from docker/whalesay
```
- docker.io - docker

## 레지스트리(Registry)

- 도커 이미지를 저장하고 공유할수 있는 서버
- 오픈소스, Apache 라이센스
- v1과 v2가 호환되지 않는다.
- 클라우드: DockerHub
- 인트라넷: DTR

```
 docker run -d -p 5000:5000 --name myregistry registry:2
 ```

 ### 레지스트리 저장소
 ### 레지스트리 보안
 ### 레지스트리 팁
 - 네이밍 기준: 루트, Repo 명
 - 이미지 데이터 관리: gc 필요
 - 성능 이슈
    - 대용량 레지스트리
    - 분산 레지스트리

## 개발 환경 구성
### GitLab
- 이슈관리, 코드리뷰, CI/CD를 지원하는 통합 개발환경 서버

### GitLab이 업그레이드 되면?
- 멈추고 up


docker run -d --hostname localhost -p 80:80 -p 443:443 -p 10022:22 --name gitlab --restart always --volume /srv/gitlab/config:/etc/gitlab --volume /srv/gitlab/logs:/var/log/gitlab --volume /srv/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest

## Docker CI 환경 구성
### Github & DockerHub
### Jenkins & Docker

```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock ubuntu:latest sh -c "apt-get update ; apt-get install docker.io -y ; bash"
```
 - it - interactive mode console 모드로 실행하는것처럼
 - v 볼륨으로 공유

 # 실습

# 복습  

# 실습