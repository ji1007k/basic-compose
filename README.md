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
2-1. fe/.env.production 파일에서 아래 항목을 수정:
```bash
USE_REMOTE_API=false
#로컬 백엔드 컨테이너와 연동을 위한 설정
```

2-2. fe/.env.local 파일에서 아래 항목을 수정:
```bash
NODE_ENV=production
```

### 3. Docker 빌드 및 컨테이너 실행
docker 폴더(또는 compose.yaml이 위치한 폴더)로 이동한 후, 아래 명령어 실행:
```bash
# 폴더 이동
cd docker
# 백그라운드에서 컨테이너 빌드 및 실행
docker compose up --build -d
# 특정 컨테이너만 빌드하고 띄우려면
docker compose up --build -d [서비스명]
```
사용 포트:

| | 컨테이터 포트 | 로컬 포트 |
|-|-|-|
| PostgreSQL | 5432 | 15432 |
| 백엔드 API | 8080 | 8080 |
| 프론트엔드 | 3000 | 3000 |

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
- 리그 (/lol/leagues/sync)
- 토너먼트 (/lol/tournaments/sync)
- 팀 (/lol/teams/sync)
- 경기일정
  - 병렬 멀티 스레드 (/lol/batch/run-match-job)
  - 싱글 스레드 (/lol/matches/sync)

> ⚠️ **Swagger에서 요청 전 반드시 Servers 셀렉트박스에서 `http://localhost:8080 - Local Server`를 선택해 주세요.**

---

### 팀 정보 수동 동기화 시 인증 방법

팀 정보 수동 동기화 API는 **CSRF 보호가 적용되어 있어**, 아래 중 **하나의 방식으로 인증을 선행해야 합니다.

#### 방법 1. Basic 인증 + CSRF 우회 헤더 사용

1. Swagger 상단의 🔒 **Authorize** 버튼 클릭
2. `BasicAuth` 입력란에 `admin / admin` 입력 후 **Authorize**
3. **Auth API > `/auth/login`** 실행 (로그인 처리)
4. 다시 Swagger 상단의 🔒 **Authorize** 버튼 클릭
5. `IgnoreCSRF` 입력란에 `skip` 입력 후 **Authorize**
6. 이후 팀 정보 수동 동기화 API 요청 실행

#### 방법 2. Basic 인증 + 실제 CSRF 토큰 검증

1. Swagger 상단의 🔒 **Authorize** 버튼 클릭
2. `BasicAuth` 입력란에 `admin / admin` 입력 후 **Authorize**
3. **Auth API > `/auth/login`** 실행
4. **CSRF API > `/csrf`** 실행
    - 응답으로 받은 CSRF 토큰 값을 복사
5. Swagger 상단의 🔒 **Authorize** 버튼 클릭
6. `CSRF` 입력란에 복사한 토큰 값 입력 후 **Authorize**
7. 이후 팀 정보 수동 동기화 API 요청 실행


> 위 절차를 거치지 않으면 요청이 **403 Forbidden**으로 차단될 수 있습니다.
> 
> **백엔드 애플리케이션에서 직접 Swagger UI를 띄운 경우**에는 로그인 후 별도 설정 없이 바로 요청 가능




