# DATABO3 프로젝트 (TEAM 3)

## :busts_in_silhouette: 팀 구성원

| 강경훈 | 나채현 | 박상진 | 양현성 | 윤인섭 | 이지현 | 정세인 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| ![](https://avatars.githubusercontent.com/u/143977692?v=4) | ![](https://avatars.githubusercontent.com/u/111407310?v=4) | ![](https://avatars.githubusercontent.com/u/140796840?v=4) | ![](https://avatars.githubusercontent.com/u/95899916?v=4) | ![](https://avatars.githubusercontent.com/u/55538952?v=4) | ![](https://avatars.githubusercontent.com/u/141386540?v=4) | ![](https://avatars.githubusercontent.com/u/143979590?v=4) |
|[GitHub](https://github.com/kkh5535)|[GitHub](https://github.com/chaehyonNa)|[GitHub](https://github.com/Mizz1ove)|[GitHub](https://github.com/HyeonSeon9)|[GitHub](https://github.com/insub2004)|[GitHub](https://github.com/badgelatte)|[GitHub](https://github.com/SeinJs)|

## :dart: 목표

사무실의 쾌적한 상태를 유지하기 위한 에너지 효율 관리 서비스를 개발하는 것이 우리 팀의 목표입니다. 이 서비스는 현재 사무실의 환경 상태를 실시간으로 모니터링하고, 사용자가 원하는 설정 값에 따라 자동으로 제어할 수 있는 기능을 제공합니다. 이를 통해 에너지 소비를 최적화하고, 효율적인 사무실 환경을 유지할 수 있도록 합니다.

## :clipboard: 프로젝트 설명

우리 팀은 사무실의 환경을 관리하기 위한 에너지 효율 서비스를 개발하는 프로젝트를 수행하고 있습니다. 이 서비스는 사용자가 사무실의 조명, 온도 등과 같은 환경 요소를 모니터링하고 제어할 수 있도록 합니다. 또한 사용자가 설정한 값에 따라 자동으로 환경을 조절하여 에너지를 절약하고 편의성을 제공합니다.

## :classical_building: 설계

- **기본 아키텍쳐**

![fourth diagram](https://github.com/nhnacademy-aiot1-team3/.github/blob/main/profile/images/architecture-diagram-ver-6.jpg)

## :globe_with_meridians: 기능

### WEB


**Front**

DashBoard View를 보여주는 Server

port는 8080, 8081

1. 인증 여부에 따라 서비스 페이지 접근
     - 인증을 받지 않은 경우 : 로그인 페이지 접근

2. 권한 별로 접근 페이지 분리
     - admin : 모든 페이지 접근 가능
     - user : 대쉬보드, 내정보, 장치 추가 기능 등등
 
3. 실시간 차트 시작화
     - 센서 api의 데이터 정보를 가져와 그래프를 이용한 시각화
 
4. jwt 토큰 만료일 확인
     - 로그인 시 auth server에서 access token과 refresh token의 만료일을 보내준것을 통해 확인
 
5. github, payco, OAuth 로그인

6. L4 스위치를 이용한 front 이중화

---

**Gateway**

port 는 8888

1. 라우팅
    - path 별로 라우팅을 해줌
    - auth path : /auth/**
    - api account : /api/account/**
    - 추가될 api : /api/~/**

2. 로드밸런싱
    - 로드 밸린싱이 필요한 api에 추가
  
3. jwt 토큰 검증
    - 인가가 필요하지 않은 경로를 제외 / 인가없이 path를 확인해 라우팅 해줌
        - 로그인 (/auth/login)
        - oauth 로그인 (/auth/**/oauth)
        - 회원가입 (/api/account/member/register)
        - 추가할 사항
          
    - 인가가 필요한 경로는 토큰의 시그니처를 검증, 토큰의 만료 여부를 확인
    - 검증에 실패하면 UNAUTHORIZED(401) 오류를 요청이 왔던 곳으로 반환

4. blacklist 관리
    - 로그아웃 요청일때 해당 토큰을 redis에 저장
    - 요청이 들어 올때마다 블랙리스트 토큰과 현재 요청에 담긴 토큰을 확인해 로그아웃이 된 토큰인지 비교하고 블랙리스트에 없다면 정상처리 있다면 UNAUTHORIZED(401) 오류를 반환
   
---

**Eureka**
Eureka client를 생성, 관리 하는 Server

1. spring security를 적용
   - euruka 서버에 접속하려면 저장한 id, pw를 입력해야 한다.
   - csrf().disable() - REST API 성격이 더 커서 비활성화
     
2. 각 api 서버들은 Server client이며 client가 직접 registry에 등록
   
4. client side discovery

---

**Account (계정)**

DB에 계정의 정보를 등록, 삭제, 조회, 수정과 관련된 기능을 담당하고 있는 API이다.

1. 로그인 사용자 정보 조회
     - 요청은 사용자의 이메일 주소 또는 아이디로 사용하여 이루어진다.
     - Auth API에서 오는 요청을 따라 사용자의 계정 아이디와 비밀번호로 응답을 보낸다.
     - 요청한 아이디 또는 이메일 가진 데이터가 존재하지 않으면 null 보낸다.

3. 회원가입
     - 사용자의 정보를 받아오면 이를 DB에 저장을 한다.
     - 아이디, 이메일, 비밀번호를 받아온다.

3. 회원탈퇴
     - 사용자의 아이디 또는 이메일을 통해 DB에 존재하는지 판단하고 삭제한다.

4. 회원정보 수정
     - 사용자의 정보를 수정한다.

### IoT

**온도**

1. 실시간 온도 측정
    - 사무실의 평균 온도를 그래프로 시각화
    - 각 구역 별로 온도 시각화
    - 관리자는 자신이 원하는 구역만 선택해 온도를 확인 가능
  
2. 이상치 설정
    - 이상치를 넘어서기 전 3~5도 전에 "경고", "위험" 알림 메시지를 통해 알림
    - 이상치 조절 가능
      
3. AI 이상 감지 기능
    - 훈련 데이터(7일 ~ 30일 전 데이터)를 통해 모델 학습
    - 학습시킨 모델에 실시간 데이터를 입력
    - 입력된 실시간 데이터와 학습된 모델의 패턴을 비교
    - 이상 탐지되면 알림
    - 설정한 제어 실행
  
---

**마그네틱 접점**

1. 직접 설정 가능
    - 접점이 분리되면 안되는 시간 설정
    - 해당 시간에 분리되면 알림
2. AI 이상 감지 기능
    - 마그네틱 접점 데이터(7일 ~ 30일 전 데이터)를 통해 모델 학습
    - 학습시킨 모델에 실시간 데이터를 입력
    - 입력된 실시간 데이터와 학습된 모델의 패턴을 비교
    - 이상 탐지되면 알림
    - 설정한 제어 실행

---

**VS133**

1. 실시간 모니터링 기능
   - 실시간 출입 인원 수 현황, 현재 인원 수 현황

---

**습도**

1. dashboard에서 owner가 일정 이상치를 설정 할 수 있다.
   
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 비오는 날, humidity 수치가 올라가면 제습기 ON(램프 또는 모터 ON)
   - 건조한 날, humidity 수치가 내려가면 가습기 ON(램프 또는 모터 ON)
     
3. 센서의 위치를 실제 사무실 위치와 동일하게 로드맵을 구성할 수 있다.

---

**CO2**

1. dashboard에서 owner가 일정 이상치 범위를 설정 할 수 있다.
   
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 현재 co2 수치가 설정한 범위 내에서 각기 다른 색의 램프를 통해 알림
   - 또는 범위 내에서 "경고", "위험" 등의 수준을 담은 메시지를 알림 메시지를 통해 알림
     
3. 센서의 위치를 실제 사무실 위치와 동일하게 로드맵을 구성할 수 있다.

---

**전력**

1. 전월량 비교
    - 일정시간 동안 사용한 전력의 양을 시간대 별로 나눠서 비교
        - ex) 3월 1일의 6시, 3월 2일의 6시를 비교해서 분석해줌 -> 이걸 1주, 1달 설정 기간별로 보여주며 비교해줌
          
    - 관리자가 원하는 기간의 데이터의 그래프를 메인 화면에 커스텀 할 수 있게 해줌

2. 전력 소비 AI로 예측, 일정 수치에 도달하면 일부 기능 강제 OFF
    - 전력 소비한 것을 전압, 전류, 역률, 전력, 전력량의 데이터를 보며 이번달의 전력소비를 AI를 통해 예측해줌
        - ex) 현재 3월 10일인데 남은 3월 달의 전력을 10일간 소비한 기록과 지금까지 달마다 소비한 기록(2월 1월 작년 12월...)의 데이터를 통해 3월 전력 소비를 예측
          
    - 어느정도에 예산의 전기료를 설정하는 기능을 추가하여 그달의 설정한 요금이 넘어갈것 같으면 AI에서 예측한것을 통하여 전력 아끼기 모드?에 들어가서 온도, 습도에 사용되는 기계들이 조정되고 알림이 발생하여 관리자가 인지할 수 있게 해줌
   
---

**에너지 관련 추가 선택 사항**

도욱 효율적인 에너지 절약을 위해 전력 제어하는데 필요한 추가 기능.

1. 직원 수 식별 기능 (인원 상황에 따라 차별된 환경)
     - 시스템은 사용자가 설정한 직원 수를 기반으로 에너지 소비를 추적하고, 해당 데이터에 따라 차별화된 환경을 설정할 수 있어야 한다.
     - 직원 수는 사용자가 수동으로 입력할 수 있어야 한다. (사용자 지정 옵션)
     - 직원 수에 따라 시스템은 조명, 온도 등의 환경 요소를 조절할 수 있어야 한다.

**제어**

전력 소스를 차단 또는 개방하는 기능.

1. 스위치
     - 사용자는 스위치를 사용하여 전체 램프나 특정 램프를 켜거나 끌 수 있어야 한다. (수동 제어)
     - 스위치 조작은 실시간으로 적용되어야 하며, 상태 변경 시 사용자에게 피드백이 제공되어야 한다.

2. 램프
     - 램프는 켜고 끌 수 있는 기능을 제공해야 한다.
     - 사용자는 개별적으로 램프를 제어할 수 있어야 하며, 필요에 따라 그룹화하여 제어할 수도 있어야 한다.
     - 램프의 상태 변경은 실시간으로 적용되어야 하며, 사용자에게 상태 변화에 대한 피드백이 제공되어야 한다.

## :wrench: 개발 기술 스텍

### Language

- ![Java](https://img.shields.io/badge/Java-E34F26?style=flat&logo=Java&logoColor=white)
- ![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat&logo=Thymeleaf&logoColor=white)
- ![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
- ![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=CSS3&logoColor=white)

### Framework

- ![Spring](https://img.shields.io/badge/spring-6DB33F?style=flat&logo=spring&logoColor=white)
- ![Spring Boot](https://img.shields.io/badge/spring%20boot-6DB33F?style=flat&logo=springboot&logoColor=white)
- ![Spring Security](https://img.shields.io/badge/spring%20security-6DB33F?style=flat&logo=springsecurity&logoColor=white)
- ![Spring Cloud](https://img.shields.io/badge/spring%20cloud-3693F3?style=flat&logo=googlecloud&logoColor=white)
- ![Spring Gateway](https://img.shields.io/badge/spring%20gateway-3693F3?style=flat&logo=googlecloud&logoColor=white)
- ![Spring Eureka](https://img.shields.io/badge/spring%20eureka-3693F3?style=flat&logo=googlecloud&logoColor=white)
- ![SpringDataJPA](https://img.shields.io/badge/Spring%20Data%20JPA-6DB33F?style=flat&logo=Spring&logoColor=white)

### CI/CD

- ![Github Action](https://img.shields.io/badge/Github%20Action-2088FF?style=flat&logo=githubactions&logoColor=white)
- ![NHN Cloud](https://img.shields.io/badge/-NHN%20Cloud-blue?style=flat&logo=iCloud&logoColor=white)
- ![SonarQube](https://img.shields.io/badge/SonarQube-4E98CD?style=flat&logo=SonarQube&logoColor=white)

### DB

- ![MySQL](http://img.shields.io/badge/MySQL-4479A1?style=flat&logo=MySQL&logoColor=white)
- ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=Redis&logoColor=white)

## :memo: 기타 문서
- ### [WIKI 페이지](https://github.com/nhnacademy-aiot1-team3/.github/wiki)
- ### [API 명세서](https://github.com/nhnacademy-aiot1-team3/.github/wiki/API-%EB%AA%85%EC%84%B8%EC%84%9C)
