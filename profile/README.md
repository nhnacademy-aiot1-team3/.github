# DATABO3 프로젝트 (TEAM 3)

## :busts_in_silhouette: 팀 구성원

| 강경훈 | 나채현 | 박상진 | 양현성 | 윤인섭 | 이지현 | 정세인 | 박종빈 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| ![](https://avatars.githubusercontent.com/u/143977692?v=4) | ![](https://avatars.githubusercontent.com/u/111407310?v=4) | ![](https://avatars.githubusercontent.com/u/140796840?v=4) | ![](https://avatars.githubusercontent.com/u/95899916?v=4) | ![](https://avatars.githubusercontent.com/u/55538952?v=4) | ![](https://avatars.githubusercontent.com/u/141386540?v=4) | ![](https://avatars.githubusercontent.com/u/143979590?v=4) | ![](https://avatars.githubusercontent.com/u/112842467?v=4) |
|[GitHub](https://github.com/kkh5535)|[GitHub](https://github.com/chaehyonNa)|[GitHub](https://github.com/Mizz1ove)|[GitHub](https://github.com/HyeonSeon9)|[GitHub](https://github.com/insub2004)|[GitHub](https://github.com/badgelatte)|[GitHub](https://github.com/SeinJs)|[GitHub](https://github.com/JongbeenPark)|

## :memo: 중요한 문서
- ### [WIKI 페이지](https://github.com/nhnacademy-aiot1-team3/.github/wiki)
- ### [API 명세서](https://github.com/nhnacademy-aiot1-team3/.github/wiki/API-%EB%AA%85%EC%84%B8%EC%84%9C)
- ### [로드맵](https://docs.google.com/spreadsheets/d/1YqwhYOUfoeq6wLenFOB-whe454M5sj2ZTO-KG4r63vk/edit?usp=sharing)
  
## :dart: 목표

사무실의 쾌적한 상태를 유지하기 위한 에너지 효율 관리 서비스를 개발하는 것이 우리 팀의 목표입니다. 이 서비스는 현재 사무실의 환경 상태를 실시간으로 모니터링하고, 사용자가 원하는 설정 값에 따라 자동으로 제어할 수 있는 기능을 제공합니다. 이를 통해 에너지 소비를 최적화하고, 효율적인 사무실 환경을 유지할 수 있도록 합니다.

## :clipboard: 프로젝트 설명

우리 팀은 사무실의 환경을 관리하기 위한 에너지 효율 서비스를 개발하는 프로젝트를 수행하고 있습니다. 이 서비스는 사용자가 사무실의 조명, 온도 등과 같은 환경 요소를 모니터링하고 제어할 수 있도록 합니다. 또한 사용자가 설정한 값에 따라 자동으로 환경을 조절하여 에너지를 절약하고 편의성을 제공합니다.

## :classical_building: 설계

- **기본 아키텍쳐**

![seventh diagram](https://github.com/nhnacademy-aiot1-team3/.github/blob/main/profile/images/architecture-diagram-ver-7.png)

- **상세 아키텍쳐**

![seventh diagram detailed](https://github.com/nhnacademy-aiot1-team3/.github/blob/main/profile/images/architecture-diagram(detailed)-ver-7.png)

## :globe_with_meridians: 기능


### WEB

**Front**

View를 보여주는 Server. HTTP와 HTTPS 프로토콜를 지원하며 port는 8080, 8081.

1. 인증 여부에 따라 서비스 페이지 접근
     - 인증을 받지 않은 경우 : 로그인 페이지 접근

2. 권한 별로 접근 페이지 분리
     - admin : 모든 페이지 접근 가능
          - Admin 관리 페이지
               - owner 회원 가입 요청 승인, 거절
               - 부서 추가 및 삭제 요청 승인, 거절
               - owner 회원 목록
          - 공지사항 추가 및 수정
     - owner :
          - 마이페이지
               - 개인정보(id,pw, 프로필 이미지)
               - 비밀번호 변경
               - 부서 목록 및 추가/삭제 요청
                    -각 부서별로 viewer 승인대기 목록 확인 가능
          - 센서 배터리 현황
          - Error log
          - Sensor 목록
          - 대시보드 - 원하는 차트 선택 후 정렬
          - 습도, co2, 전력
               - 여러 차트 중 선택 후 정렬 가능
               - sensor 추가 버튼
               - sensor 목록 버튼
          - 온도
               - 여러 차트 중 선택 후 정렬 가능
               - ai 제어와 기본 제어 중 선택 가능
          - 도어 센서 리스트
               - ai 제어와 기본 제어 중 선택 가능
               - 출입 로그 확인
               - 설정된 시간동안의 출입 빈도수 그래프
               - 도어 센서 설정 페이지
                    - 원하는 시간 설정 및 삭제
               - 현재 구역별 인원 그래프
     - user :
          - 대시보드 - 원하는 차트 정렬 가능
               - 여러 차트 중 선택 후 정렬 가능(온도, 습도, co2, 전력, 도어 센서)
          - 내 정보
 
3. 실시간 차트 시작화
     - 센서 api의 데이터 정보를 가져와 그래프를 이용한 시각화
 
4. 원하는 차트 선택 가능

5. jwt 토큰 만료일 확인
     - 로그인 시 auth server에서 access token과 refresh token의 만료일을 보내준것을 통해 확인
 
6. github, payco, OAuth 로그인

7. L4 스위치를 이용한 front 이중화

**Gateway**

모든 요청이 진입하는 접점이다. port 는 8888.

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
   
**Eureka**

Eureka client를 생성, 관리 하는 Server.

1. spring security를 적용
   - euruka 서버에 접속하려면 저장한 id, pw를 입력해야 한다.
   - csrf().disable() - REST API 성격이 더 커서 비활성화
     
2. 각 api 서버들은 Server client이며 client가 직접 registry에 등록
   
4. client side discovery

---

### API

**Account API**

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
     - 가입이후 사용자의 권한 수정이 가능하다. (viewer -> owner)
       
5. 회원 상태
     - 회원의 상태는 총 4가지이다.
     - 회원은 1가지의 상태만 가질 수 있다.
     - 첫 회원 가입시 wait 상태이며 가입승인을 받으면 active 상태가 된다.
     - 로그인을 한 달 이상 하지 않을 시 자동으로 pause(휴면) 상태가 된다.
     - 휴면 상태를 풀 수 있다. (방법은 아직)

6. 회원 권한

     - 회원은 한 개의 권한만 가질 수 있다.
     - 권한 승인을 받아야 해당 권한이 생긴다.
     - 권한 승인은 바로 상위 권한 + 같은 조직에 속한 사용자가 승인해 줄 수 있다.
     - owner는 admin에게 승인을 받는다.
     - viewer는 owner에게 승인을 받는다.
     - 권한 변경은 viewer -> owner로만 가능하다.

**Auth API**

사용자 인증 및 인가와 관련된 기능하는 API.

1. 요청에 따라 JWT Token을 발급해준다.
     - auth api는 account api에 사용자가 입력한 아이디와 비밀번호를 전달하여 로그인 성공 여부를 요청하고, 성공한다면 토큰을 발급한다.

2. jwt 토큰과 함께 로그아웃 요청이 오면, redis의 blacklist에 jwt 토큰을 등록하고, 보관기간을 jwt 토큰의 유효기간 까지로 둔다.

3. 토큰 재발급 요청이 오면, refresh 토큰을 재발급하고 redis에 보관하고 있던 refresh토큰을 갱신한다.

**Sensor API**

DB에 저장되어 있는 데이터를 꺼내오고 이를 반환해주는 API, 차트 API에 연결 지어 데이터를 주고 받는다.

1. 가공된 데이터 조회

     - Rule Engine에서 가공된 센서 데이터를 조회할 수 있어야 한다.
     - 온도, 습도, CO2, 마그네틱 접점의 데이터를 조회할 수 있어야 한다.
     - RDB에서 가져온 데이터를 JSON 형식으로 온다.
     - 가공된 데이터 종류:
       - 7일간의 평균 값
       - 사용자 지정 시간의 평균 값
       - 예상 값

2. 실시간 데이터 조회

     - 센서에서 수집한 데이터를 실시간으로 제공할 수 있어야 한다.
     - 클라이언트가 요청할 때마다 가장 최근의 데이터를 반환해야 한다.

3. 센서 설정 저장

     - 권한 있는 사용자가 선택할 수 있도록 옵션을 저장한다.
       - 센서 기능 타입, target value, 오차 범위, 시작 및 종료 날짜 시간을 지정할 수 있다.
     - 적절한 데이터베이스를 사용하여 데이터를 관리해야 한다.

4. 센서 등록, 삭제, 수정 기능

     - 조직의 오너가 센서의 정보를 등록, 삭제, 수정할 수 있어야 한다.

5. 전체 센서 및 종류별 센서 목록 조회

     - 전체 센서 조회: 조직 내에 해당되는 센서들의 목록 조회 가능해야 한다.
     - 종류별 센서 조회: 조직 내에 존재하는 센서들 중 조회하는 사용자가 선택한 종류에 해당되는 센서 목록을 표시해야 한다. 

6. 센서의 data 로그 관리 기능

     - 센서와 관련된 모든 동작(센서 보내는 데이터, 센서 설정 변경)을 표시하고 이를 데이터베이스에 저장하고 관리한다.

7. 센서의 error 로그 관리 기능

     - 센서가 작동하지 않거나 데이터 수집에 실패한 경우에 대비한 에러 내용을 로그에 표시한다.
     - 사용자가 적절하게 처리할 수 있도록 자세한 오류 메시지를 표시한다.


**Chart API**

차트와 관련된 모든 기능을 보유하고 있다.

1. 다양한 차트 유형 지원

     - 선 그래프, 막대 그래프, 원형 그래프 등 다양한 차트 유형을 지원해야 한다.
     - 사용자가 선택할 수 있도록 선택 목록을 주어야 한다.

2. 다중 데이터 시각화

     - 여러 데이터 세트를 동시에 시각화할 수 있는 기능이 필요한다.
     - 한 차트에 여러 데이터를 표시할 수 있어야 한다.
     - 온도 경우 예측 값을 표시할 수 있도록 한다.
     - 각 센서별 현재 값을 표시하는 차트가 있어야 한다.

3. 인터랙티브 기능

     - 사용자가 차트를 상호 작용할 수 있는 기능이 필요한다.
     - 차트 위에 마우스를 가져가면 해당 값이 표시되어야 한다.

4. 맞춤 설정 가능

     - 차트의 스타일, 색상, 축 레이블 등을 사용자가 맞춤 설정할 수 있는 기능이 필요한다.
     - 사용자가 차트를 둘 수 있는 위치를 표시해주어야 한다.
     - 사용자가 특정 장소의 데이터를 조회할 수 있도록 선택 목록을 줘야 한다.

**Member Management API**

Admin 및 Owner가 조직 및 사용자를 관리하는데 사용되는 API.

1. 조직 생성, 삭제, 수정, 목록 조회 기능

      - 조직 생성, 삭제와 같은 조직 관리에 필요한 기능을 제공할 수 있어야 한다. 
      - 목록 종류:
        - 승인 대기 목록
        - viewer 목록
        - 조직 목록

2. 조직에 멤버 추가, 삭제 기능

      - 사용자에게 조직 추가, 삭제 기능을 제공할 수 있어야 한다.
    
3. 공지 사항 추가, 삭제, 수정 및 조회 기능
   
      - 관리자는 공지사항을 추가, 삭제 및 수정할 수 있어야 한다.
      - 모든 사용자에게 공지사항을 조회할 수 있도록 해야 한다.
      - 공지사항에 파일을 첨부할 수 있도록 해야 한다.
---

### Rule Engine

  - 환경 : 스프링? O X?
  - 역할 : 실시간 센서의 측정 데이터들을 각 센서 타입별 설정 값에 기반해서 제어에 활용한 후 알맞게 가공 후 데이터베이스에 저장
  - 설계 방식 : flow based model

**Flow**

0. 진입 전

  - RabbitMQ Consumer application으로 부터 가공되지 않은 데이터를 받음
    
1. 등록 센서 확인

  - topic에 있는 센서의 아이디 추출
  - 추출한 아이디로 데이터베이스의 센서테이블에 포함되어 있는지 검사
  - 없으면 데이터베이스에 추가
  
2. 결측치 필터링

  - payload의 value값이 NaN 또는 Null과 같이 undefined 인지 확인
  - 결측치에 해당하면 7번으로 이동
  - 그 외에는 3번으로 이동
  - 에러로그 테이블에 에러 타입 추가 할거? <--
    
3. 오탐탐지 필터링

  - payload의 value값이 오류에 해당하는 값인지 판별(ex. temperature value : 500)
  - 오류로 판정 시 7번으로 이동
  - 그 외에는 4번으로 이동
  - 오탐 기준 정하기 <--
  - (오류탐지 하는 방법 아직..) <--
  - 에러로그 테이블에 에러 타입 추가 할거? <--
    
4. 기능별 분류 설정

  - 데이터베이스에서 센서의 설정된 기능(function_id)을 확인
  - function_id별로 흐름 제어
    - function_id : 0 (read_only) -> 7번으로 이동
    - functoin_id : 1 (basic) -> 5번으로 이동
    - function_id : 2 (ai) -> 4-1번으로 이동

4.1 AI 설정

  - 센서 타입별 학습된 AI모델 사용
  - 실시간 데이터를 AI모델에 적용
  - 적용 후 결과를 5번으로 이동

5. 제어 유무 판별

  - paload의 값과 데이터베이스에서 가져온 센서별 설정값을 이용해 제어 여부 판단
  - 제어가 필요할 시 원본 데이터 대신 "어떤 장치가 어떤 제어를 할 것인가"에 대한 데이터를 6번으로 이동 
  - 제어 여부와 상관없이 원본 데이터는 7번으로 이동

6. 제어

  - 데이터베이스의 센서,디바이스,제어행동 테이블을 참고해 5번에서 넘어온 데이터를 실행
  - 제어 로그 7번으로 이동

7. 데이터 가공

  - 들어온 데이터를 데이터 베이스에 저장할 각 테이블 형식에 맞게 가공
  - 8번으로 이동

8. 데이터베이스에 가공 데이터 추가

  - 들어온 데이터를 데이터베이스의 각 테이블에 추가
  - error_log, device_log, sensor_data_log 테이블

---

### 인공지능 요구사항

1. 목적
    - 온도 제어와 전력 사용료 제공을 위한 인공지능
<br>

2. 기능
    - 온도는 그래프를 분석해 희망온도를 예측
    - 전력은 그래프를 분석해 다음달 사용료를 예측
<br>

3. 성능
    - 정확도는 85% 이상
    - 학습데이터는 80%, 테스트데이터는 20% 사용
    - 분류 성능 평가 지표로 나타낼 것
<br>

4. 시스템 요구사항
    - 언어는 python을 사용 할 것
    - 인공지능 모델은 선형 회귀(Linear Regression)과 Random Forest를 사용 할 것
    - 지도학습으로 진행 할 것
    - 학습 할 데이터는 influxdb에서 api token을 이용해서 데이터 수집
    - influxdb에서 query문을 통해서 데이터를 받아온 뒤 데이터 가공한 후에 인공지능에 학습

        1. 온도
            - 모델을 통해 다음주 온도 데이터를 예측
            - 예측한 데이터를 이용해서 선호온도를 예측
              
        2. 전력량
            - 모델을 통해 전력사용략을 예측
            - 예측한 데이터를 이용해서 전력 사용료를 예측

---

### 데이터 수집

고객사 Gateway로 수집한 데이터 RuleEngine까지 전달시 필요한 요구사항.

**Protocol Transfer Gateway**

각 고객사별 Gateway로 수집하는 센서데이터 Protocol을 MQTT message로 변환하여 MessageBroker로 전달. port는 1883.

1. 센서와 게이트웨이는 사측 인원이 직접 설치한다.

2. 설치의 범위는 다음과 같다.

     - 게이트웨이를 이용자의 네트워크망에 등록한다.
     - 설치한 센서를 등록한 게이트웨이에 등록한다.
     - 게이트웨이에서 읽어들이는 센서 데이터의 토픽을 회사 규정에 맞게 설정한다.
     - 관리자 페이지에서 게이트웨이의 주소를 등록한다.

3. Node-red를 이용하여 데이터의 topic을 가공한다.

**Message Broker**

Protocol Transfer Gateway로 받은 MQTT message를 simple telegraf 와 sub/pub application으로 전달.

- MQTT Client와 Serversocket을 이용
- topic : data/#
- simple telegraf & sub/pub app 로 publish 한다.
  
- 브로커가 게이트웨이를 구독하는 방식
     - 1883port로 Mqtt message 구독한다.
     - 서버 구동시 브로커가 db와 통신하여 gateway의 주소들을 읽어와 구독한다.
     - 관리자 페이지에서 db의 gateway목록에 대한 CRUD를 요청할 수 있고, 요청이 성공적으로 수행되었다면, 브로커에 db와 재통신 할 것을 요청한다.
     - 브로커는 요청을 수행하여 db의 gateway의 주소들을 읽어와 구독 대상의 목록을 갱신한다.

**Sub/Pub Application**

- Message broker를 통하여 받은 MQTT message를 RabbitMQ 를 이용하여 Queue에 보관
- MQTT Client sub을 이용하여 MQTT message 수신

**RabbitMQ 설정**

Exchange

- Exchange Type : topic / header  
- Durability : Durable - Sensor data 보존  
- Auto delete : No

Queue

- SensorType에 따라 binding
- Queue Name : 각 Sensor Type 에 따라서 설정 (ex. Temperature)
- Durability : No

---

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

**VS133**

1. 실시간 모니터링 기능
   - 실시간 출입 인원 수 현황, 현재 인원 수 현황

**습도**

1. dashboard에서 owner가 일정 이상치를 설정 할 수 있다.
   
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 비오는 날, humidity 수치가 올라가면 제습기 ON(램프 또는 모터 ON)
   - 건조한 날, humidity 수치가 내려가면 가습기 ON(램프 또는 모터 ON)
     
3. 센서의 위치를 실제 사무실 위치와 동일하게 로드맵을 구성할 수 있다.

**CO2**

1. dashboard에서 owner가 일정 이상치 범위를 설정 할 수 있다.
   
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 현재 co2 수치가 설정한 범위 내에서 각기 다른 색의 램프를 통해 알림
   - 또는 범위 내에서 "경고", "위험" 등의 수준을 담은 메시지를 알림 메시지를 통해 알림
     
3. 센서의 위치를 실제 사무실 위치와 동일하게 로드맵을 구성할 수 있다.

**전력**

1. 전월량 비교
    - 일정시간 동안 사용한 전력의 양을 시간대 별로 나눠서 비교
        - ex) 3월 1일의 6시, 3월 2일의 6시를 비교해서 분석해줌 -> 이걸 1주, 1달 설정 기간별로 보여주며 비교해줌
          
    - 관리자가 원하는 기간의 데이터의 그래프를 메인 화면에 커스텀 할 수 있게 해줌

2. 전력 소비 AI로 예측, 일정 수치에 도달하면 일부 기능 강제 OFF
    - 전력 소비한 것을 전압, 전류, 역률, 전력, 전력량의 데이터를 보며 이번달의 전력소비를 AI를 통해 예측해줌
        - ex) 현재 3월 10일인데 남은 3월 달의 전력을 10일간 소비한 기록과 지금까지 달마다 소비한 기록(2월 1월 작년 12월...)의 데이터를 통해 3월 전력 소비를 예측
          
    - 어느정도에 예산의 전기료를 설정하는 기능을 추가하여 그달의 설정한 요금이 넘어갈것 같으면 AI에서 예측한것을 통하여 전력 아끼기 모드?에 들어가서 온도, 습도에 사용되는 기계들이 조정되고 알림이 발생하여 관리자가 인지할 수 있게 해줌
   
**출입 카운터**

- 모든 사용자는 데이터 값을 조회할 수 있어야 한다.
- 사용자 지정 설정이 없다.

**제어**

전력 소스를 차단 또는 개방하는 기능.

1. 스위치
     - 사용자는 스위치를 사용하여 전체 램프나 특정 램프를 켜거나 끌 수 있어야 한다. (수동 제어)
     - 스위치 조작은 실시간으로 적용되어야 하며, 상태 변경 시 사용자에게 피드백이 제공되어야 한다.

2. 램프
     - 램프는 켜고 끌 수 있는 기능을 제공해야 한다.
     - 사용자는 개별적으로 램프를 제어할 수 있어야 하며, 필요에 따라 그룹화하여 제어할 수도 있어야 한다.
     - 램프의 상태 변경은 실시간으로 적용되어야 하며, 사용자에게 상태 변화에 대한 피드백이 제공되어야 한다.

3. 휀
     - 기본기능 또는 AI기능을 통해 설정된 범위를 벗어난게 탐지되면 작동

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

