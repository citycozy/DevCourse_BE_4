# ✅ 하이큐(HIQ)
<img width="585" height="313" alt="image" src="https://github.com/user-attachments/assets/c8636397-c21c-4195-a0e6-db82b4d65257" /></br>
실시간 멀티플레이 퀴즈 및 소통 플랫폼  
참가자 관리, 실시간 채팅, 멀티 퀴즈 기능 지원  

## ⚙️ 사용 기술 
#### • Backend  
  Java, Spring Boot, Spring JPA, Spring Security
#### • Database  
  MariaDB, Redis   
#### • Real-time  
  WebSocket, STOMP   
#### • Documentation  
  Swagger UI   
#### • DevOps 
  AWS, EC2, S3, Gradle 

## 🏆 주요 성과
### 1) RESTful API 설계 및 엔드포인트 표준화  
<img width="300" height="100" alt="응답 형식" src="https://github.com/user-attachments/assets/af691c1f-b2e7-4827-a980-c8c78d4ec2bd" /> <br/>
• API의 응답 구조를 GlobalResponse<T> 클래스로 통일  
#### → 팀 내 API 일관성 확보, 프론트엔드와의 협업 효율 향상



### 2) 웹소켓 기반 실시간 아키텍처 구축
<img width="300" height="100" alt="이벤트핸들러" src="https://github.com/user-attachments/assets/3c3fcddf-49f9-44f2-aac9-3ae7d430f81d" /><img width="300" height="100" alt="이벤트 문서화" src="https://github.com/user-attachments/assets/69439496-248f-40be-9ec3-f9bec8fa9a2b" /><img width="300" height="100" alt="이벤트 흐름도" src="https://github.com/user-attachments/assets/6c4d1f1e-06be-45dc-b02e-54d2892c44c6" />
<br />  
• 이벤트 규약 설계 후 STOMP 기반 실시간 브로드캐스트로 프론트엔드에 즉시 전달  
• 모든 참가자가 항상 동일한 최신 상태를 확인 가능  
• 여러 사용자가 동시에 이벤트를 발생시켜도 데이터 꼬임 없이 일관성 유지  
• 명확한 이벤트 규약과 구조화된 브로드캐스트로 협업 및 유지보수 편의성 증가  
• [WSRoomService.java](https://github.com/citycozy/HIQ/blob/develop/src/main/java/com/example/web2_3_ourtuft_be/websocket/service/WSRoomService.java#L23)
#### → 사용자 간 정보 불일치 문제 95% 이상 해결, 실시간 피드백 제공  


### 3) 실시간 방 입장/퇴장, 퀴즈 진행, 유저 상태 동기화 기능 개발  
• 실시간 게임 진행 로직 개발( 게임 시작 설정, 게임 시작, 퀴즈 전송, 답안 제출, 플레이어 점수 관리 등 )  [GameService](https://github.com/citycozy/HIQ/blob/develop/src/main/java/com/example/web2_3_ourtuft_be/websocket/service/WSGameService.java)  
• 실시간 방 관련 로직 개발( 이벤트 처리, 유저 입장, 유저 퇴장, 게임 시작 등 ) [RoomService](https://github.com/citycozy/HIQ/blob/develop/src/main/java/com/example/web2_3_ourtuft_be/websocket/service/WSRoomService.java)  
#### → 50명 동시 접속 환경에서 평균 응답 250ms 이내 성능 달성


### 4) 이벤트 리스너 기반 자동화(네트워크 단절 시 상태 동기화)  
• 연결/해제/구독 이벤트를 감지하여 별도의 API 호출 없이 자동으로 방 입장/퇴장 및 상태 동기화  
• 네트워크 단절, 브라우저 종료 등 예외 상황에서도 사용자 상태와 방 정보가 즉시 반영되도록 설계  
• [EventListner.java](https://github.com/citycozy/HIQ/blob/develop/src/main/java/com/example/web2_3_ourtuft_be/websocket/event/WSEventListener.java)  
#### → 상태 동기화 정확도 90% 이상을 안정적으로 유지

### 5) 사용자 인증 및 보안 구현
• Access Token은 클라이언트에 전달(쿠키), Refresh Token은 Redis에 저장 (보안성 확보)  
• Redis를 통해 Refresh Token 실시간 관리 및 사용자 세션 유지  
• 로그아웃 시 Redis에서 즉시 토큰 만료 처리 → 재사용 공격 방지  
• 인증 필터에서 JWT 추출/검증 → 유효 사용자인 경우 SecurityContext에 등록  
• WebSocket 핸드셰이크(Handshake) 시점에서도 JWT → 사용자 세션에 유저 정보(Attribute) 저장  
• [Security](https://github.com/citycozy/HIQ/tree/develop/src/main/java/com/example/web2_3_ourtuft_be/security)  

### 6) STOMP 기반 메시지 규약 설계 및 도입  
• /topic/room/{roomId} 등 도메인별 엔드포인트를 설계해 메시지 흐름을 분리  
• 메시지 유형(채팅, 퀴즈, 정답, 알림 등)을 구분하여 클라이언트와 서버간 실시간 동기화 신뢰성 강화  
• [WebSocketService](https://github.com/citycozy/HIQ/blob/develop/src/main/java/com/example/web2_3_ourtuft_be/websocket/service/WebSocketService.java)   
#### → 메시지 처리 오류율 2% 미만으로 감소, 유지보수성 향상



