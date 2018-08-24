---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 연결 구성
{: #connection-configuration}

{{site.data.keyword.composeForScyllaDB_full}} 데이터베이스 연결은 3개의 HAProxy 포털에서 관리합니다. 각 포털에는 64MB의 메모리가 있습니다. 

클라이언트 측의 장애 복구는 애플리케이션 디자이너의 책임입니다. 여러 연결 문자열과 단계별 장애 복구를 자동으로 처리하기 위해 하나의 연결 문자열, 세 개의 연결 문자열 모두, 연결 맵 또는 이 모두를 조합하여 제공할 수 있다는 점에서 대부분의 Scylla 드라이버에서는 클러스터를 인식하고 있습니다. 장애 복구 오류를 완전히 처리하는 드라이버로 작업하지 않는 한 다음을 수행해야 합니다.

* 해당 연결 구성에서 다중 엔드포인트를 허용하는 드라이버로 작업합니다.
* 데이터베이스를 읽거나 쓰기 위한 애플리케이션 호출이 오류에 적절하게 반응하는지 확인합니다. 옵션에는 다음이 포함됩니다.
  + 오류를 로깅한 후 다시 연결
  + 다시 연결하여 오류의 원인이 된 오퍼레이션 재시도
  + 실패한 오퍼레이션을 큐에 넣고 다시 연결 스케줄

## 전송 중 암호화

모든 {{site.data.keyword.composeForScyllaDB}} HAProxy 포털에서는 TLS/SSL이 사용되며 TLS 버전 1.2를 지원합니다. 서비스 인증서는 Let's Encrypt 인증서입니다. 특정 드라이버와 명령행 인터페이스인 cqlsh에서 서비스에 연결하려면 인증서 체인의 사본이 필요합니다. 자세한 정보는 [인증서 사용](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)을 참조하십시오.

## 연결 한계

두 개의 mongos 라우터에서 HAProxy당 연결 한계는 1000개입니다. 연결 한계를 초과하면 서비스가 응답하지 않습니다. 드라이버의 연결 풀링 기능을 통해 애플리케이션에서의 연결을 관리할 수 있습니다.

## 프록시 제한시간

HAProxy 포털에서 서버와 클라이언트 간 연결의 라이프사이클을 관리합니다. 여기에서는 드라이버 작성자 및 {{site.data.keyword.composeForScyllaDB}} 데이터베이스 연결의 하위 레벨 활동에 관심이 있는 사용자용 참조서로 자세히 설명합니다.

설정 |값 | 적용
----------|-----------|-----------
client | 5m | 클라이언트에서 데이터를 수신확인하거나 전송할 것으로 예상되는 경우.
connect | 4s | 프록시와 서버 간에 연결 중인 경우.
server | 5m | 서버에서 데이터를 수신확인하거나 전송할 것으로 예상되는 경우.
tunnel | 1일 | 클라이언트와 서버 간에 양방향 연결이 설정되고 연결이 두 방향 모두에서 비활성 상태로 유지되는 경우.
client-fin | 1h | 한 방향이 이미 종료되어 있는 동안 클라이언트에서 데이터를 수신확인하거나 전송할 것으로 예상되는 경우.
check | 5s | 프록시와 서버를 서로 연결 중일 때 2차 제한시간 확인으로 사용하는 경우.

{: caption="표 1. Scylla HAProxy 제한시간" caption-side="top"}
