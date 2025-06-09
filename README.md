# JIKIM.GG 통합 개발 환경 (Docker Compose 기반)
> Next.js 프론트엔드와 Spring Boot 백엔드 서브모듈 연동 및 Docker Compose 테스트용 환경 구성


---

## 테스트 환경

- Docker 28.1.1
- Docker Desktop 4.41.2
- Git

---

## 실행 방법

### 1. 저장소 클론 및 서브모듈 초기화

```bash
git clone https://github.com/ji1007k/basic-compose.git
cd basic-compose
git submodule update --init --recursive
```


### 2. 프론트엔드 설정 변경
fe/.env.production 파일에서 아래 항목을 수정:
```bash
USE_REMOTE_API=false
```
로컬 백엔드 컨테이너와 연동 시 필요한 설정

### 3. Docker 빌드 및 컨테이너 실행
docker 폴더(또는 compose.yaml이 위치한 폴더)로 이동한 후, 아래 명령어 실행:
```bash
docker compose up -d --build
```

### 4. 컨테이너 실행 확인
```bash
docker ps
```
아래 컨테이너들이 실행 중인지 확인:  
- postgres-db
- docker-be-1
- docker-fe-1

---

## 접속 주소
- 프론트엔드: https://localhost:3000/jikimi  
- 백엔드 API 문서 (Swagger): http://localhost:8080/api/swagger-ui/index.html
### 기본 로그인 계정
- admin / admin
- user / user
---

## 사용 포트 (컨테이너 / 로컬)
로컬 포트가 이미 사용 중인 경우 `docker/compose.yaml` 파일에서 포트 수정
- **PostgreSQL** : 5432 / 15432
- **백엔드 API** : 8080 / 8080
- **프론트엔드** : 3000 / 3000  



