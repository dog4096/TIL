## WebClient의 Buffer Size 설정    

----
Dev 환경에서 WebClient를 사용 중에 DataBufferLimitException가 발생했다.  
확인해보니 WebClient의 기본 버퍼 사이즈는 256kb라고 한다. 이전엔 property 설정도 가능했던걸로 보이나  
현재는 WebClient Build시에 같이 넣어줘야한다.
```java
ExchangeStrategies exchangeStrategies = ExchangeStrategies.builder()
            .codecs(clientCodecConfigurer -> clientCodecConfigurer.defaultCodecs().maxInMemorySize(BUFFER_SIZE))
            .build();

this.webClient = WebClient.builder()    
            .exchangeStrategies(exchangeStrategies)
            .build();
```