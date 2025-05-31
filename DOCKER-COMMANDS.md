# 📘 DOCKER-COMMANDS.md

## 🛠️ 도커 이미지 명령어

### 이미지 빌드
**중간에 이미지/이미지에 대한 스크립트가 변경되어도 변경된 부분만 실행되기 때문에 더 빠르지만 딥러닝하는 도커 이미지는 보통 용량이 수십GB라서 캐시가 과도하게 쌓일 수도 있음(그러면 캐시 지우고 처음부터 해야함.)**

```bash
docker build -t my-image .
```

### 캐시 없이 이미지 빌드
**처음부터 다시 빌드하기 때문에 script가 중간에 바뀌면 오래 걸릴 수도 있지만 캐시가 쌓이면 용량이 부족해서 멈출 수도 있음.<br/>오래 걸려도 안정적으로 설치하고 싶은 경우 이렇게 빌드**

```bash
docker build --no-cache -t my-image .
```

### 이미지 목록 보기

```bash
docker images
```

### 이미지 삭제

```bash
docker rmi my-image
```

### 사용하지 않는 이미지 모두 삭제

```bash
docker image prune -a
```

---

## 🚀 도커 컨테이너 명령어

### 컨테이너 실행

```bash
docker run --rm -it --gpus all my-image
```

### 특정 명령어로 실행

```bash
docker run --rm -it --gpus all my-image bash
```

### 컨테이너 목록 보기

```bash
docker ps              # 실행 중인 컨테이너

docker ps -a           # 모든 컨테이너 (종료 포함)
```

### 컨테이너 중지 및 삭제

```bash
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

### 컨테이너 내부 접속

```bash
docker exec -it <CONTAINER_ID> bash
```

---

## 📦 볼륨 마운트 예시

로컬 코드를 컨테이너에 연결해 개발/실행하는 예시:

```bash
docker run --rm -it --gpus all \
  -v $(pwd)/my_repo:/workspace/my_repo \
  -w /workspace/my_repo \
  my-image
```

---

## 🔄 도커 시스템 정리 명령어

### 중지된 컨테이너 모두 제거

```bash
docker container prune
```

### 네트워크, 볼륨 포함 전체 정리 (주의)

```bash
docker system prune -a
```

> 주의: 위 명령어는 **모든 이미지/컨테이너/네트워크**를 삭제하므로 신중히 사용하세요.

---

## ✅ 유용한 옵션 요약

| 옵션           | 설명                            |
| ------------ | ----------------------------- |
| `--rm`       | 종료 시 자동 삭제                    |
| `-it`        | 인터랙티브 + 터미널 모드                |
| `--gpus all` | GPU 할당 옵션 (nvidia-docker2 필요) |
| `-v`         | 로컬 볼륨 마운트                     |
| `-w`         | 컨테이너 내 작업 디렉토리 지정             |
