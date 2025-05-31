# 🐧 Ubuntu 설정 및 명령어 가이드

이 문서는 GCP 인스턴스 또는 로컬 Ubuntu 환경에서 자주 사용하는 명령어와 시스템 설정을 정리한 가이드입니다.

---

## 📦 기본 시스템 명령어

| 기능             | 명령어                        |
| -------------- | -------------------------- |
| 현재 디렉토리 확인     | `pwd`                      |
| 파일 목록 보기       | `ls` 또는 `ls -al`           |
| 디렉토리 이동        | `cd 디렉토리명`                 |
| 디렉토리 생성        | `mkdir 폴더명`                |
| 파일 복사          | `cp source.txt target.txt` |
| 파일 이동          | `mv source target`         |
| 파일 삭제          | `rm 파일명` 또는 `rm -r 폴더명`    |
| 압축 해제 (zip)    | `unzip 파일.zip`             |
| 압축 해제 (tar.gz) | `tar -xzvf 파일.tar.gz`      |

---

## 🌐 네트워크 및 시스템

| 기능       | 명령어                                   |
| -------- | ------------------------------------- |
| 시스템 업데이트 | `sudo apt update && sudo apt upgrade` |
| 패키지 설치   | `sudo apt install 패키지명`               |
| 프로세스 보기  | `top` 또는 `htop` (설치 필요)               |
| 포트 확인    | `netstat -tuln` 또는 `ss -tuln`         |
| IP 주소 확인 | `ip a` 또는 `hostname -I`               |

---

## 📝 텍스트 편집기

| 편집기  | 명령어                 |
| ---- | ------------------- |
| Nano | `nano 파일명.py` |
| Vim  | `vim 파일명.py`  |
> 보통 Nano가 쉽다고들 하는 것 같은데 개인적으로 Vim을 선호함.

---

## 🐍 Python/Conda 환경 설정

### Conda 환경 생성 및 활성화

```bash
conda create -n myenv python=3.10
conda activate myenv
```

### 패키지 설치

```bash
pip install 패키지명
# 또는
mamba install 패키지명 -c conda-forge
```

---

## 🔍 시스템 정보 확인

```bash
nvidia-smi             # GPU 정보 확인
uname -a               # 커널/OS 정보
cat /etc/os-release    # Ubuntu 버전
```

---

## 📁 파일 다운로드

```bash
wget URL
curl -O URL
```

---

## 🧪 자주 쓰는 유틸리티

```bash
htop                   # 실시간 시스템 사용률 보기
watch -n 1 nvidia-smi  # GPU 사용률 1초 단위로 모니터링
```

> 위 유틸리티가 없다면: `sudo apt install htop`

### *주로 htop으로 cpu 및 ram 사용량 확인, watch nvidia-smi로 gpu 사용량 확인!!*
