# Eureka
Eureka client를 생성, 관리 하는 Server

1. spring security를 적용
   - euruka 서버에 접속하려면 저장한 id, pw를 입력해야 한다.
   - csrf().disable() - REST API 성격이 더 커서 비활성화
2. 각 api 서버들은 Server client이며 client가 직접 registry에 등록
3. client side discovery

---

# 습도
