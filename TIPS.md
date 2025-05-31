# 📘 TIPS.md

## 🔁 PyTorch + CUDA 버전 호환

GitHub 레포 마다 PyTorch와 CUDA 버전이 다르므로 아래 표를 확인하고 정확한 조합으로 설치하세요.

| PyTorch 버전 | CUDA 버전 | pip 설치 명령어                                                                                                        |
| ---------- | ------- | ----------------------------------------------------------------------------------------------------------------- |
| 2.2.0      | 12.1    | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121`                                |
| 2.1.0      | 11.8    | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118`                                |
| 1.13.1     | 11.7    | `pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 -f https://download.pytorch.org/whl/torch_stable.html` |

🔗 참고: [https://pytorch.org/get-started/previous-versions](https://pytorch.org/get-started/previous-versions)

---

## 🔍 Dockerfile 디버깅 팁

| 문제 상황      | 해결 방법                                                   |
| ---------- | ------------------------------------------------------- |
| `nvcc` 없음  | CUDA Base 이미지가 `devel`인지 확인 (e.g., `devel-ubuntu20.04`) |
| 버전 충돌      | `pip freeze` 확인 후 명시 버전 지정                              |
| Python 미실행 | `CMD` 또는 `ENTRYPOINT` 누락 여부 확인                          |
| 캐시 문제      | `--no-cache`로 재빌드 시도                                    |

---

## 📦 Docker 컨테이너 실전 실행 패턴

```bash
# bash 진입 (디버깅용)
docker run --rm -it --gpus all my-image bash

# 실행 자동화
chmod +x run_repo.sh
./run_repo.sh
```

---

## 🧰 Ubuntu 자주 쓰는 명령어

| 기능        | 명령어                                   |
| --------- | ------------------------------------- |
| 현재 위치 보기  | `pwd`                                 |
| 디렉토리 보기   | `ls -al`                              |
| 디렉토리 이동   | `cd 디렉토리명`                            |
| 파일 다운로드   | `wget URL`                            |
| 편집기 열기    | `nano 파일명.py` 또는 `vim 파일명.py`         |
| 새 디렉토리 생성 | `mkdir 폴더명`                           |
| zip 압축 해제 | `unzip 파일.zip`                        |
| tar.gz 해제 | `tar -xzvf 파일.tar.gz`                 |
| 패키지 설치    | `sudo apt install 패키지명`               |
| 시스템 업데이트  | `sudo apt update && sudo apt upgrade` |

---

## 📄 Conda 환경 팁

```bash
# 환경 생성
conda create -n myenv python=3.10

# 환경 활성화
conda activate myenv

# 패키지 설치
mamba install numpy pytorch -c pytorch
```

### 도커에서는 Conda 대신 `base`에 pip로 설치하는 경우가 많습니다.
즉, 따로 conda 환경 없이 애초에 container 자체를 해당 레포를 위한 container로 만들기 때문에 굳이 conda 환경으로 나눌 필요가 없을 수도 있습니다.


---

## ✅ GitHub 레포 체크리스트

* [ ] `requirements.txt` 또는 `environment.yml` 존재 여부 확인
* [ ] `cuda` 또는 `nvcc` 요구사항 문서화 여부
* [ ] PyTorch 버전 확인 (`torch==x.x.x`)
* [ ] `main.py`, `train.py`, `demo.py` 등의 실행 스크립트 위치 확인
