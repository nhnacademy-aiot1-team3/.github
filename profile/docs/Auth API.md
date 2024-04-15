## Auth API

사용자 인증 및 인가와 관련된 기능하는 API.

1. 요청에 따라 JWT Token을 발급해준다.
  - auth api는 account api에 사용자가 입력한 아이디와 비밀번호를 전달하여 로그인 성공 여부를 요청하고, 성공한다면 토큰을 발급한다.

2. jwt 토큰과 함께 로그아웃 요청이 오면, redis의 blacklist에 jwt 토큰을 등록하고, 보관기간을 jwt 토큰의 유효기간 까지로 둔다.

3. 토큰 재발급 요청이 오면, refresh 토큰을 재발급하고 redis에 보관하고 있던 refresh토큰을 갱신한다.
