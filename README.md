# FitConnect

<div align="center">

![image](https://github.com/user-attachments/assets/4cf74131-7934-468a-8579-503a444b8f44)


> **운동 파트너 매칭, 맞춤형 식단 추천, 실시간 채팅 등 종합 헬스 케어 웹 서비스** <br/> **개발기간: 2024.09 ~ 2024.10**
</div>

## 개발팀 소개

|      김지환       |          김건웅         |       오상준         |       오제록         |       오현석         |                                                                                                               
| :------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------: | :------------------------------------------------------------------------------: |
|   [@jihwan](https://github.com/Czerny40)   |    [@Koungcon](https://github.com/Koungcon)  | [@Ohsangjun1412](https://github.com/Ohsangjun1412)  | [@jeork](https://github.com/jeork)  | [@Musugol](https://github.com/Musugol)  |

## 프로젝트 소개

본 프로젝트는 사용자들이 효과적으로 운동 목표를 관리하고, 개인의 능력에 맞는 파트너를 쉽게 찾으며, 맞춤형 식단도 추천받을 수 있도록 돕는 통합 헬스 케어 웹 서비스입니다. Spring Boot 기반으로 구성되었으며, 일부 서비스를 위한 Flask와 FastAPI 서버와 연동이 필요합니다.

## 기술 스택
- 백엔드 (Main):

  - Java : JDK 21
    
  - Spring Boot 3.2.8 (Gradle 빌드 관리)
  
  - Spring Data JPA (DB: MySQL)
  
  - Spring Security + OAuth2.0


- 백엔드 (추가 서버):

  - FastAPI (Python) : 운동 파트너 매칭 알고리즘

  - Flask (Python) : 식단 추천 알고리즘

- 프론트엔드:

  - HTML/CSS/JavaScript


- 기타:

  - MySQL

  - Thymeleaf
    
  - WebSocket 기반 실시간 채팅

  - BCryptPasswordEncoder(비밀번호 해시)

  - AES(메시지 암호화)


## 주요 기능
- 운동 파트너 매칭(FastAPI 연동):

  - https://github.com/Czerny40/userMatching

- 식단 추천(Flask 연동):
  
  - [식품의약품안전처 데이터](https://various.foodsafetykorea.go.kr/nutrient/general/down/list.do?)를 활용한 음식 영양소 DB 구축

  - Flask 서버에서 유전 알고리즘 + 협업 필터링(사용자 평점) + 콘텐츠 기반 필터링(초기 선호)
  
  - **유전 알고리즘(Genetic Algorithm)**과 협업 필터링 + 콘텐츠 기반 필터링을 결합해 사용자의 목표에 부합하는 맞춤형 식단 추천
  
  - 콜드 스타트 문제 해결을 위해 콘텐츠 기반을 먼저 적용 후 협업 필터링 병행

  - https://github.com/Czerny40/Foodrecommend 

- 캘린더 기반 운동 루틴 관리:
  - 부위 별 주요 운동 가이드 제공 

  - 사용자가 직접 운동 루틴을 생성, 날짜별로 기록
  
  - 캘린더 UI로 루틴 진행 상황 한눈에 확인

- 커뮤니티 & 실시간 소통:

  - Q&A 게시판: 질문/답변 CRUD, 검색
  
  - 메시지: AES로 암호화된 1:1 쪽지 시스템

  - 실시간 채팅(WebSocket): STOMP 프로토콜로 개인/그룹 대화 가능
  
  - Spring Security & OAuth2.0: 기본 로그인 및 카카오/구글 등 소셜 로그인 통합

- 헬스장 검색(카카오 맵 API):

  - 사용자 위치 기반으로 인근 헬스장을 지도에 표시
    
- 관리자 대시보드:

  - 회원, 게시글, 운동 루틴, 식단 추천 데이터 등을 모니터링·관리
  
  - ROLE_ADMIN 권한으로 접근 가능 

## 아키텍처 (Architecture)

```
┌──────────────────────────────────────┐
│            Client (Web)              │
└──────────────────────────────────────┘
              ▲            │
              │            ▼
     (UI/UX, API 요청)   [HTTP/REST]
┌──────────────────────────────────────┐
│      Spring Boot (Main Backend)      │
│   Gradle, JPA, Security, WebSocket   │
│            ┌──────────┐              │
│            │  MySQL   │              │
│            └──────────┘              │
└──────────────────────────────────────┘
           │               │
     [HTTP/REST]     [HTTP/REST]
           │               │
  ┌─────────────────┐  ┌─────────────────┐
  │   FastAPI       │  │    Flask        │
  │ (Partner Match) │  │ (Diet Recomm.)  │
  └─────────────────┘  └─────────────────┘
           │               │
         [DB]             [DB]    

```
## 디렉토리 구조
```
PROJECT
├─ .gradle/                # Gradle 설정/캐시 폴더
├─ .idea/, .settings/, ... # IDE 관련 설정 폴더
├─ bin/, build/            # 컴파일 결과물 폴더(Gradle 빌드 산출물 등)
├─ gradle/                 # Gradle Wrapper 관련 스크립트
├─ src
│  └─ main
│     ├─ java
│     │  └─ com.mysite.sbb
│     │     ├─ answer/          # Q&A(질문)게시판 댓글 관련 MVC 구성 (Controller, Service, Model/Entity)
│     │     ├─ chat/            # 실시간 채팅(WebSocket) 기능 MVC
│     │     ├─ fitnessSearch/   # 인근 헬스장 검색 관련
│     │     ├─ food/            # 식단 추천 서버 관련
│     │     ├─ message/         # 쪽지/메시지 기능(Controller, Service, Repository)
│     │     ├─ question/        # Q&A(질문)게시판 게시글 관련 MVC
│     │     ├─ service/         # RestClient 서비스 로직
│     │     ├─ user/            # 사용자(User) 관리(Controller, Service, Repository)
│     │     │  ├─ AdminController.java
│     │     │  ├─ CustomOAuth2UserService.java
│     │     │  ├─ SiteUser.java           # User 엔티티(Model)
│     │     │  ├─ UserController.java     # 사용자 요청 처리(Controller)
│     │     │  ├─ UserCreateForm.java
│     │     │  ├─ UserModifyForm.java
│     │     │  ├─ UserRepository.java     # User 엔티티를 다루는 Repository
│     │     │  ├─ UserRole.java
│     │     │  ├─ UserSecurityService.java
│     │     │  └─ UserService.java        # 사용자 관련 비즈니스 로직(Service)
│     │     ├─ UserMatching/     # 파트너 매칭 서버 관련
│     │     ├─ util/            # AES 암호화 등 공통 유틸 클래스
│     │     ├─ workout_tab/     # 운동 루틴, 캘린더 기반 CRUD 관련 MVC
│     │     ├─ DataNotFoundException.java # 공통 예외 처리
│     │     ├─ MainController.java        # 메인 페이지 컨트롤러
│     │     ├─ SbbApplication.java        # Spring Boot 실행(Main) 클래스
│     │     └─ SecurityConfig.java        # Spring Security/OAuth2 설정
│     └─ resources/             # 템플릿(Thymeleaf), 정적 자원(css, js) 등
└─ test/                        
├─ .classpath, .project, ...
├─ build.gradle
├─ gradlew, gradlew.bat
├─ README.md
└─ settings.gradle

```
## 설치 및 실행 방법 (Gradle 기준)

### Installation
``` bash
$ git clone https://github.com/Czerny40/Fitness-Web-App.git
$ cd Fitness-Web-App
```
#### Spring Boot (메인 서버) 실행 (Gradle)
```
# Gradle 초기 설정
./gradlew clean build     # 프로젝트 빌드
./gradlew bootRun         # Spring Boot 서버 실행
```
- http://localhost:8080에서 메인 서버 확인 가능
#### FastAPI 서버 (운동 파트너 매칭)
```
git clone https://github.com/Czerny40/userMatching.git
cd userMatching
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```
- http://localhost:8000에서 FastAPI 서버 동작 확인
- Spring Boot에서 해당 엔드포인트로 매칭 요청을 보내고, 결과를 수신
#### Flask 서버 (식단 추천)
```
git clone https://github.com/Czerny40/Foodrecommend.git
cd Foodrecommend
python app.py
```
- http://localhost:5000에서 Flask 서버 동작 확인
- Spring Boot에서 REST 호출로 식단 추천 결과를 받아옴
  
---
## 기능 확인
| 나의 루틴 기록  |  식단 추천   |
| :-------------------------------------------: | :------------: |
|  <img width="329" src="https://github.com/user-attachments/assets/47239aca-9a70-4d21-b1ca-535557ad0fad"/> |  <img width="329" src="https://github.com/user-attachments/assets/c84802e9-cb68-4aae-91b1-5f2a5d86e64d"/>|  
| 헬스 파트너 찾기   |  Q&A 게시판   |  
| <img width="329" src="https://github.com/user-attachments/assets/8db6d88a-5712-48c6-8d3e-f069d0e2f1a4"/>   |  <img width="329" src="https://github.com/user-attachments/assets/55ce53cc-58e7-49ba-ad60-e410904959ea"/>     |
| 메세지   |  채팅   |  
| <img width="329" src="https://github.com/user-attachments/assets/f95d022e-9b7c-4034-82c4-d102860e359c"/>   |  <img width="329" src="https://github.com/user-attachments/assets/f805f101-5748-42c9-bc53-a5b1d8c3f4c4"/>     |

---
## 참고자료
- [식품의약품안전처(영양소 DB)](https://various.foodsafetykorea.go.kr/nutrient/general/down/list.do?)

- [IPF GL (파워리프팅 공식)](https://www.powerlifting.sport/rules/codes/info/ipf-formula)

- [FastAPI 공식문서](https://fastapi.tiangolo.com/ko/)

- [Flask 공식문서](https://flask-docs-kr.readthedocs.io/ko/latest/)

## 문의
- GitHub Issues 활용 혹은 이메일(castellina12@gmail.com) 문의
