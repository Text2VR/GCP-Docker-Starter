# 🌍 WorldGen 예제: 텍스트로부터 3D Mesh (.ply) 생성하기

이 프로젝트는 [WorldGen](https://github.com/ZiYang-xie/WorldGen)과 [FLUX.1-dev](https://huggingface.co/black-forest-labs/FLUX.1-dev) 모델을 활용하여 텍스트 프롬프트로부터 `.ply` 포맷의 3D 메쉬를 생성하는 전체 파이프라인을 **Docker 컨테이너**에서 자동 실행하는 예제입니다.

---

## 📌 주요 기능

- ✅ CUDA 12.1 기반 환경 자동 구성
- ✅ `WorldGen` 및 `UniK3D` submodule 자동 설치
- ✅ CUDA KNN 커널 컴파일 자동 수행
- ✅ Hugging Face Gated 모델 (`FLUX.1-dev`) 사용 지원
- ✅ `.ply` 포맷 3D 메쉬 자동 생성

---

## 📁 프로젝트 구조
```
WorldGen_example/
├── Dockerfile # 환경 설정
├── generate_both.py # 메인 실행 스크립트
└── output/ # 생성된 .ply 파일 저장 폴더
```
---

## 🐳 실행 방법 (Docker 기반)

### 1. 레포지토리 클론

```bash
git clone --recurse-submodules https://github.com/<your-repo>.git
cd WorldGen_example
```

### 2. Docker 이미지 빌드
```bash
docker build --no-cache -t worldgen_autorun .
```

### 3. Docker 컨테이너 실행
```bash
docker run --gpus all --rm -v $(pwd)/output:/app/WorldGen/output -it worldgen_autorun bash
```
> 실행 시 다음이 자동으로 수행됩니다:
> * WorldGen 및 submodules 설치
> * CUDA KNN 커널 컴파일
> * /app/WorldGen/generate_both.py 실행

---
## 🧠 컨테이너 내에서 수행할 작업
### 1. CUDA KNN 커널 컴파일
```bash
cd /app/WorldGen/submodules/UniK3D/unik3d/ops/knn
bash compile.sh
```

### 2. Hugging Face 로그인 및 토큰 설정
```bash
pip install huggingface_hub
huggingface-cli login
```
* https://huggingface.co/settings/tokens 에서 Fine-grained Access Token 생성
  * 이름: WorldGen
  * 권한: Read 이상
* FLUX.1-dev 모델 페이지 에서 Request Access 버튼 클릭 후 승인
* 승인 완료 후 huggingface-cli login 으로 로그인

### 3. 메쉬 생성 실행
```bash
cd /app/WorldGen
python generate_both.py
```

---

## 🟨 생성 결과
```
WorldGen_example/
├── Dockerfile # 환경 설정
├── generate_both.py # 메인 실행 스크립트
└── output/ # 생성된 .ply 파일 저장 폴더
    ├── output_mesh.ply
    └── output_gaussian.ply
```
* /app/WorldGen/output 내부에 .ply 파일이 생성되며, 로컬의 output/ 경로에 매핑됩니다.

---
## 🛠️ 문제 해결 요약
| 문제 상황                                   | 원인                  | 해결 방법                             |
| --------------------------------------- | ------------------- | --------------------------------- |
| `libGL.so.1` 오류                         | OpenCV 의존성 누락       | Dockerfile에 `libgl1` 추가           |
| `huggingface_hub.errors.GatedRepoError` | Gated 모델 접근 불가      | 모델 페이지에서 "Request Access" 후 승인 대기 |
| `huggingface-cli` 없음                    | huggingface CLI 미설치 | `pip install huggingface_hub` 실행  |
| `$PYTHONPATH` 오류                        | ENV 변수 참조 시점 문제     | `${PYTHONPATH:-}` 기본값으로 설정(or 컨테이너 진입 후 수동 실행)        |
| Docker `CMD` 실패                         | 복잡한 명령어 처리 불안정      | 컨테이너 진입 후 수동 실행 권장                |

### `$PYTHONPATH` 오류?
```bash
cd /app/WorldGen/submodules/UniK3D/unik3d/ops/knn
bash compile.sh
```
이런식으로 환경변수 상관없이 아예 접속해서 명시적으로 실행해버리기~

---
## 📚 참고 링크
* [WorldGen GitHub](https://github.com/ZiYang-xie/WorldGen)
* [FLUX.1-dev 모델 (Hugging Face)](https://huggingface.co/black-forest-labs/FLUX.1-dev)
* [PyTorch3D](https://github.com/facebookresearch/pytorch3d)
* [Hugging Face CLI 문서](https://huggingface.co/docs/huggingface_hub/quick-start)
