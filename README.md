# 🧑‍💻 프로젝트 환경 세팅 가이드

이 문서는 본 프로젝트를 실행하기 위해 필요한 **GCP 기반 GPU 개발 환경 구성 방법**을 정리한 안내서입니다. 팀원들이 동일한 환경에서 개발을 시작할 수 있도록 GCP 세팅부터 Docker 실행까지 단계별 문서로 나누어 제공됩니다.

> ⚠️ 이 레포는 코드가 아닌 **환경 세팅 문서 전용 레포**입니다.<br/>
> 해당 방법대로 따라하던 중, 뭔가 '불편'하거나 코드편집/과정 등에서 '비효율적인 것 같은데...?'라는 생각이 들면 당신이 뭔가 잘못하고 있는 걸 수도 있습니다.<br/>
> 만약 그렇다면 연락주세요.
---

## 📁 문서 구성

| 문서                                                     | 설명                                                      |
| ------------------------------------------------------ | ------------------------------------------------------- |
| [`GCP_Setup_Guide.md`](./GCP_Setup_Guide.md)           | GCP 가입, 프로젝트 생성, GPU 인스턴스 생성, SSH 키 등록 등 초기 세팅          |
| [`Ubuntu_Command_Guide.md`](./Ubuntu_Command_Guide.md) | Ubuntu 환경에서 자주 사용하는 명령어 및 시스템 설정                        |
| [`Docker_Setup_Guide.md`](./Docker_Setup_Guide.md)     | NVIDIA 드라이버 설치, Docker/nvidia-docker 설정, Dockerfile 작성법 |
| [`DOCKER-COMMANDS.md`](./DOCKER-COMMANDS.md)           | Docker 이미지/컨테이너 관리 명령어 정리                               |
| [`TIPS.md`](./TIPS.md)                                 | PyTorch/CUDA 버전 호환표, Conda, 디버깅 팁 등 실전 활용 노트            |

---

## 🚀 사용 흐름 요약

1. `GCP_Setup_Guide.md` 문서를 따라 GCP VM을 생성하고 SSH로 접속합니다.
2. `Docker_Setup_Guide.md`를 참고하여 NVIDIA 드라이버 및 Docker 환경을 설정합니다.
3. 실행하려는 GitHub 레포에 맞게 Dockerfile을 수정하고 컨테이너를 실행합니다.
4. 작업 도중 명령어가 헷갈릴 경우 `DOCKER-COMMANDS.md` 또는 `Ubuntu_Command_Guide.md`를 참고하세요.
5. CUDA 버전 충돌이나 실행오류 발생 시 `TIPS.md`에 있는 PyTorch 호환표나 디버깅 팁을 활용하세요.

---

## 🧩 환경 통일 목적

이 가이드는 모든 팀원이 **동일한 Docker 기반 개발 환경**에서 일관되게 실험하고 개발할 수 있도록 설계되었습니다.
GCP의 GPU 자원은 유료이므로, 불필요한 인스턴스는 반드시 종료하거나 삭제해주세요.

---

## 📬 문의 및 피드백

문서에 잘못된 부분이 있거나, 개선할 아이디어가 있다면 PR 또는 이슈로 남겨주세요!

# Kubernetes 대신 Docker Compose를 사용하는 게 나을 듯.
