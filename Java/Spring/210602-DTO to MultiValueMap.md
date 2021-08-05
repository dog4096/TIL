## DTO to MultiValueMap    

---
> 참고 사이트 https://jojoldu.tistory.com/478  

RestTemplate, Mockmvc, WebClient 등 HTTP Request를 날리는 친구들은 Get 요청을 할 때   
Query Parameter 생성을 위한 객체로 MultiValueMap을 요구합니다.  
Controller에선 DTO로 받을 수 있지만 정작 요청 테스트를 할 땐 DTO로 보낼 수 없어서 불편하니  
DTO를 MuiltiValueMap으로 변환하는 방법을 찾아야 했습니다.

###                                                                                                        