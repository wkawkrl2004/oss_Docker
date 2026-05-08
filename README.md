# 실습 5 - Docker

FastAPI 애플리케이션을 Docker로 컨테이너화하고 AWS EC2에 배포하는 실습입니다.

## 프로젝트 구조

```
.
├── main.py              # FastAPI 애플리케이션
├── model.py             # Pydantic 모델 정의
├── todo.py              # 데이터 로드/저장 유틸리티
├── courses.json         # 초기 데이터
├── requirements.txt     # Python 의존성
├── Dockerfile           # Docker 이미지 빌드 설정
├── docker-compose.yml   # Docker Compose 설정
└── .gitignore
```

## API 엔드포인트

| Method | URL | 설명 |
|--------|-----|------|
| GET | `/courses` | 전체 강좌 목록 조회 |
| POST | `/courses` | 강좌 추가 |

## EC2 배포 방법

### 1. EC2에 Docker 설치

```bash
sudo apt update && sudo apt install -y docker.io docker-compose
sudo systemctl enable docker && sudo systemctl start docker
sudo usermod -aG docker ubuntu
# 재로그인 후 진행
```

### 2. 프로젝트 클론

```bash
git clone <본인의 GitHub 저장소 URL>
cd <저장소 폴더명>
```

### 3-A. docker compose로 실행 (권장)

```bash
docker compose up -d --build
```

### 3-B. docker run으로 실행 (대안)

```bash
docker build -t courses-api .
docker run -d \
  --name courses-api \
  --restart always \
  -p 80:8000 \
  courses-api
```

### 4. 실행 확인

```bash
docker ps
```

### 5. 브라우저에서 확인

```
http://<EC2 퍼블릭 IP>/courses
http://<EC2 퍼블릭 IP>/docs
```

## 포트 설정

- **외부(EC2) 포트**: 80
- **컨테이너 내부 포트**: 8000
