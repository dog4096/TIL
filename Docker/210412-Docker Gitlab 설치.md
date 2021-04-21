# Docker Gitlab 설치
### 1. Image pull
```
docker pull gitlab/gitlab-ce:latest
```
### 2. 설치
```linux
docker run --detach \
    --hostname {HostName} \
    --publish {외부HttpsPort}:443 --publish {외부HttpPort}:80 --publish {외부Port}:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:{CustomDir} \
    --volume /srv/gitlab/logs:{CustomDir} \
    --volume /srv/gitlab/data:{CustomDir} \
    gitlab/gitlab-ce:latest
```

etc. 기본 계정 ID는 root이고 최초 접속시에 비밀번호 변경 안내창이 뜬다.

