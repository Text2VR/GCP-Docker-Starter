# 🐳 Docker 설정 가이드 (GCP Ubuntu + NVIDIA GPU)

이 문서는 GCP 인스턴스(Ubuntu)에 NVIDIA GPU가 있는 경우 Docker + nvidia-docker2 환경을 설치하고 GitHub 레포를 실행하는 전체 절차를 설명합니다.

---

## 1️⃣ NVIDIA 드라이버 설치
#### 여기서는 535로 해놓긴 했는데 난 L4 기준 550 깔리긴 했음.<br/>보통 터미널에서 드라이버 목록 확인 후 권장 드라이버 자동 설치하는데 Gemini나 GPT한테 물어볼 것을 강력추천함

```bash
sudo apt update
sudo apt install -y nvidia-driver-535
sudo reboot
```

재부팅 후:

```bash
nvidia-smi
```

---

## 2️⃣ Docker 설치

```bash
sudo apt install -y docker.io
sudo usermod -aG docker $USER
```

재로그인 후:

```bash
docker --version
```

---

## 3️⃣ NVIDIA Container Toolkit 설치 (nvidia-docker2)

```bash
# 키 등록
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -

# 리포지토리 추가
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

# 설치 및 재시작
sudo apt update
sudo apt install -y nvidia-docker2
sudo systemctl restart docker
```

### ✅ 테스트

```bash
docker run --rm --gpus all nvidia/cuda:12.2.0-base-ubuntu20.04 nvidia-smi
```

---

## 4️⃣ Dockerfile 예시 (PyTorch + CUDA)

```dockerfile
FROM nvcr.io/nvidia/cuda:12.1.0-cudnn8-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/opt/conda/bin:$PATH"

RUN apt-get update && apt-get install -y \
    git wget curl build-essential && rm -rf /var/lib/apt/lists/*

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-py311_23.11.0-2-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/conda && rm miniconda.sh
RUN /opt/conda/bin/conda install -y -c conda-forge mamba=1.4.2

RUN pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121

WORKDIR /workspace
RUN git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
WORKDIR /workspace/YOUR_REPO

COPY requirements.txt .
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
```

---

## 5️⃣ 실행 스크립트 예시 (run.sh)

```bash
#!/bin/bash
IMAGE="your-repo-image"
REPO="YOUR_REPO"

docker build -t $IMAGE .
docker run --rm -it --gpus all \
  -v $(pwd)/$REPO:/workspace/$REPO \
  -w /workspace/$REPO \
  $IMAGE
```

---

## 6️⃣ 실행 시 자주 쓰는 명령어

* 컨테이너 내부 진입: `docker run --rm -it --gpus all image bash`
* 실시간 nvidia-smi 확인: `watch -n 1 nvidia-smi`
* 마운트 경로 주의: GCP 내 경로는 대부분 `/home/ubuntu/...`

> 더 많은 명령어는 `DOCKER-COMMANDS.md`를 참조하세요.
