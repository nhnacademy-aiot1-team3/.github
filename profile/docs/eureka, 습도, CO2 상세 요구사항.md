# Eureka
Eureka client를 생성, 관리 하는 Server

1. spring security를 적용
   - euruka 서버에 접속하려면 저장한 id, pw를 입력해야 한다.
   - csrf().disable() - REST API 성격이 더 커서 비활성화
2. 각 api 서버들은 Server client이며 client가 직접 registry에 등록
3. client side discovery

---

# 습도

1. dashboard에서 owner가 일정 이상치를 설정 할 수 있다.
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 비오는 날, humidity 수치가 올라가면 제습기 ON 또는 램프 ON
   - 건조한 날, humidity 수치가 내려가면 가습기 ON 또는 램프 ON
3. 센서의 위치를 실제 사무실 위치와 동일하게 로드맵을 구성할 수 있다.
---

# co2

1. dashboard에서 owner가 일정 이상치 범위를 설정 할 수 있다.
2. 이상치에 따라 각기 다른 제어를 설정 할 수 있다.
   - 현재 co2 수치가 설정한 범위 내에서 각기 다른 색의 램프를 통해 알림
   - 또는 범위 내에서 "경고", "위험" 등의 수준을 담은 메시지를 알림 메시지를 통해 알림
