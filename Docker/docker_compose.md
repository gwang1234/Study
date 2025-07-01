# Docker-compose 정리

<br>

### 1. compose 파일 실행
```bash
docker compose up -d
```

### 2. compose 작동 확인
```bash
docker compose ps
docker ps

docker logs
```

### 3. compose로 실행된 컨테이너 삭제
```bash
docker compose down
```

### 4. 이미지 빌드 및 push
```bash
# docker 파일 생성 후 이미지 빌드
docker build -t [image name] .

# 이미지 태그 붙이기
docker tag [image name]:latest [ip]:[port]/[image name:tag]

# 레지스트리로 푸시
docker push [ip]:[port]/[image name:tag]
```

### 5. 새 이미지 업데이트 되었을 때
```bash
# Registry에서 이미지 받기
docker compose pull [ip]:[port]/[image name:tag]

# 새로운 image 설정
vi ~/[경로]/docker-compose.yml

# 설정 적용
docker compose up -d [service name]
```

### 6. compose.yml 파일 작성
```bash
services:
  www:
    image: my-image:latest # my-image라는 도커 이미지를 사용해 컨테이너 생성
    hostname: www # 컨테이너 내부에서 인식하는 호스트네임(컴퓨터 이름)
    restart: always # 도커 데몬이 재시작되면 컨테이너도 자동 재실행
    command: "/sbin/init" # 컨테이너를 시작할 때 기본 실행 명령을 /sbin/init로 한
    privileged: true # 컨테이너에 "호스트 머신과 거의 같은 수준의 권한"을 부여
    user: root # 컨테이너 내에서 root(관리자) 권한으로 실행
    ports:
      - "33:3306"
      - "4430:443"
    volumes:
      - my-vol:/var/log/myapp # 외부 볼륨
      - /home/username/www/db:/var/lib/mysql
      - /etc/localtime:/etc/localtime
    environment: # 타임존을 서울로 지정
      TZ: Asia/Seoul
    networks:
      broker-bridge: 
        ipv4_address: 172.20.0.10 # 외부 네트워크에서 사용할 컨테이너의 고정 IP 주소

volumes:
  my-vol:
    external: true # my-vol라는 이름의 외부 볼륨을 사용

networks:
  broker-bridge:
    name: my-net # 실제 네트워크 이름은 my-net
    external: true # 이미 외부(미리 만들어둔) 네트워크”를 사용
```