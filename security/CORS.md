# CORS

태그: security

### Cross-Origin Resource Sharing

- 보통 프론트와 백엔드 개발 시 자주 발생하는 문제임
- 웹 보안상의 이유로 다른 도메인에서 오는 요청을 차단하는 정책
- 기본적으로 브라우저는 다른 출처에서 오는 요청을 제한함, 이때 출러는 url과 프로토콜, 포트까지 포함

### 출처 허용 방법

- 브라우저는 요청 메시지의 Origin 헤더와 응답 메시지의 Access-Control-Allow-Origin 헤더를 비교해서 CORS 위반 여부를 확인함
- 서버에서 Access-Control-Allow-Origin 헤더를 설정을 통해 특정 도메인의 요청을 허용할 수 있음
- 인증된 요청의 방식 경우  Access-Control-Allow-Credentials를 true로 설정하고 Access-Control-Allow-Origin에 와일드 카드를 사용하지 못함