---
layout: post
title: "[Hyperledger]Fabric SDK에서 제한시간 값 설정"
date: 2020-12-14 09:44:23 +0900
category: Hyperledger
---

## Fabric SDK에서 제한시간 값 설정

# 트랙잭션 관리
애플리케이션 클라이언트에서는 해당 트랜잭션 제안의 유효성이 검증되고 제안이 성공적으로 완료되었는지 확인해야 합니다.    
네트워크 가동 중단 또는 컴포넌트 실패와 같은 여러 이유 때문에 제안이 지연되거나 유실될 수 있다.   
컴포넌트 실패를 처리하려면 고가용성응 위해 애플리케이션을 코딩해야한다.    
또한 네트워크에서 응답하기 전에 **제안의 제한시간이 초과되지 않도록 제한시간 값을 증가** 시킬 수 있다.     
  
체인코드가 실행 중이 아니라면 첫번째로 전송된 트랜잭션 제안이 체인코드를 실행한다.     
체인코드가 시작되는 동안에 체인코드가 시작중임을 나타내는 오류가 리턴되면서 다른 모든 제안을 거부한다.     
이건 트랜잭션 무효화와 다르다. 그래서 다시 제안을 보내야 한다.     
그래서 트랜잭션 제안을 유실하지 않도록 메시지 큐를 사용한다.      
 

<br/>

# 네트워크 연결 열기 및 닫기    
트랜잭션 제안을 제출하기 전에 SDK를 이용해서 피어 및 순서 지정자를 작성한 경우 네트워크 컴포넌트간에 gRPC 연결을 열게된다.      
네트워크와 상호작용할 때 트랜잭션을 제출하기 위해 새 연결을 열지 않고 피어 및 순서 지정자 오브젝트를 재사용합니다. 피어 및 순서 지정자를 재사용하는 경우 리소스를 절약하여 더 좋은 성능으로 이어질 수 있다.     

<br/>

# 고가용성 애플리케이션    
장애 복구를 위해 조직당 최소 두 개의 피어를 배치하도록 권장한다.     
두 피어 모두 체인코드 설치하고 채널에 추가, 네트워크를 설정하고 빌드 할 때 장애 복구를 위해 순서 지정자가 있으며 하나를 사용할 수 없는 경우 다른 피어로 트랜잭션을 전송할 수 있다.     
순서 지정자가 channel 섹션에 추가되어있는지 확인해라.    

<br/>

# Fabric SDK 에서 제한시간 값 설정
파일 경로는 src\main\java\org\hyperledger\fabric\sdk\helper\Config.java 이다.    

```java

    /**
     * Timeout settings
     **/
    public static final String PROPOSAL_WAIT_TIME = "org.hyperledger.fabric.sdk.proposal.wait.time";
    public static final String CHANNEL_CONFIG_WAIT_TIME = "org.hyperledger.fabric.sdk.channelconfig.wait_time";
    public static final String TRANSACTION_CLEANUP_UP_TIMEOUT_WAIT_TIME = "org.hyperledger.fabric.sdk.client.transaction_cleanup_up_timeout_wait_time";
    public static final String ORDERER_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer_retry.wait_time";
    public static final String ORDERER_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer.ordererWaitTimeMilliSecs";
    public static final String PEER_EVENT_REGISTRATION_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.eventRegistration.wait_time";
    public static final String PEER_EVENT_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.retry_wait_time";
    public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
    public static final String EVENTHUB_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.eventhub.reconnection_warning_rate";
    public static final String PEER_EVENT_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.peer.reconnection_warning_rate";
    public static final String GENESISBLOCK_WAIT_TIME = "org.hyperledger.fabric.sdk.channel.genesisblock_wait_time";

```

```java

            // Default values
            /**
             * Timeout settings
             **/
            defaultProperty(PROPOSAL_WAIT_TIME, "120000");
            defaultProperty(CHANNEL_CONFIG_WAIT_TIME, "15000");
            defaultProperty(ORDERER_RETRY_WAIT_TIME, "200");
            defaultProperty(ORDERER_WAIT_TIME, "10000");
            defaultProperty(PEER_EVENT_REGISTRATION_WAIT_TIME, "5000");
            defaultProperty(PEER_EVENT_RETRY_WAIT_TIME, "500");
            defaultProperty(EVENTHUB_CONNECTION_WAIT_TIME, "5000");
            defaultProperty(GENESISBLOCK_WAIT_TIME, "5000");

```


<br/>
  
기본 제한시간 값을 변경해야 할 수 있다.     
예를들어 이벤트 허브 연결 제한시간 값이 5000밀리초보다 응답하는데 오래 걸리면 실패 오류가 발생한다.     

변경하려면     
파일 경로는 src\main\java\org\hyperledger\fabric\sdk\helper\Config.java입니다.     
제한시간을 직접 지정할 수 있다.      

```java
 public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
 private static final long EVENTHUB_CONNECTION_WAIT_TIME_VALUE = 15000;

 static {
     System.setProperty(EVENTHUB_CONNECTION_WAIT_TIME, EVENTHUB_CONNECTION_WAIT_TIME_VALUE);
 }

```


<br/>

* * *  
참고   
https://cloud.ibm.com/docs/blockchain?topic=blockchain-best-practices-app&locale=ko#best-practices-app-connectivity-availability