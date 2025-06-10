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
# 저장소 클론
git clone https://github.com/ji1007k/basic-compose.git
cd basic-compose
# 서브모듈 초기화
git submodule update --init
# 최신 서브모듈 커밋으로 갱신
git submodule update --remote --merge
```
--init: .gitmodules 파일에 정의된 서브모듈들을 초기화함 (디렉토리 생성 및 설정)

### 2. 프론트엔드 설정 변경
fe/.env.production 파일에서 아래 항목을 수정:
```bash
USE_REMOTE_API=false
```
로컬 백엔드 컨테이너와 연동을 위한 설정

### 3. Docker 빌드 및 컨테이너 실행
docker 폴더(또는 compose.yaml이 위치한 폴더)로 이동한 후, 아래 명령어 실행:
```bash
cd docker
docker compose up -d --build
# 특정 컨테이너만 띄우려면
docker compose up -d be/fe
```
사용 포트 (컨테이너 / 로컬):
- **PostgreSQL** : 5432 / 15432
- **백엔드 API** : 8080 / 8080
- **프론트엔드** : 3000 / 3000

---

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
### 초기 데이터 안내
초기 DB에는 **일정 관련 데이터가 포함되어 있지 않습니다.**  
[API 문서 페이지](http://localhost:8080/api/swagger-ui/index.html)에서 아래 항목들을 **수동으로 동기화**해 주세요:
- 리그
- 토너먼트
- 팀
- 경기일정

> ⚠️ **Swagger에서 요청 전 반드시 Servers 셀렉트박스에서 `http://localhost:8080 - Local Server`를 선택해 주세요.**

---

### 팀 정보 수동 동기화 시 주의사항

POST 요청 시 **CSRF 검증 우회를 위해 헤더 설정이 필요**합니다.

1. Swagger 상단의 **자물쇠 아이콘 🔒**을 클릭
2. `skip` 입력 후 **Authorize** 클릭
3. 이후 요청 전송

이 과정을 거치지 않으면 요청이 403으로 차단될 수 있습니다.

---



