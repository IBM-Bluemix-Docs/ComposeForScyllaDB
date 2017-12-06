---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Compose for ScyllaDB 시작하기
{: #getting-started-with-compose-for-scylladb}

ScyllaDB는 Cassandra 와이드 컬럼 분산 데이터베이스를 위치 내에서 대체합니다. ScyllaDB는 벤치마크에서 성능을 10배 향상시킬 수 있는 더 효율적인 리소스 사용을 위해 Cassandra의 Java가 아니라 C++로 작성됩니다. Cassandra 도구 및 데이터 파일과의 호환성을 유지하는 외에도 ScyllaDB는 자체 튜닝 기능을 추가합니다. {{site.data.keyword.composeForScyllaDB_full}}는 ScyllaDB의 기능을 확장하여 ScyllaDB를 관리하고 고가용성 및 중복성 외에도 자동화된 백업을 제공하는 사용하기 쉬운 자동 확장 배치 시스템을 제공합니다.
{:shortdesc}

**참고:** {{site.data.keyword.composeForScyllaDB_full}}에서는 Compose UI에 대한 액세스를 제공하지 않습니다. 세부사항은 [Compose on {{site.data.keyword.cloud}} 지원](https://help.compose.com/docs/bluemix-compose-support)을 참조하십시오.

## Compose for ScyllaDB 서비스 인스턴스 작성

[{{site.data.keyword.composeForScyllaDB}} 인스턴스를 작성하십시오](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

서비스의 인스턴스를 작성하는 경우 서비스의 이름과 신임 정보 이름을 모두 선택하십시오. 서비스를 바인딩되지 않은 상태로 두십시오. 서비스가 프로비저닝될 때 제공되는 신임 정보를 사용하여 나중에 애플리케이션을 서비스에 연결할 수 있습니다. 다양한 신임 정보 값이 "사용 가능한 신임 정보" 섹션에 나열됩니다.

{{site.data.keyword.composeForScyllaDB}} 인스턴스를 프로비저닝할 때 *표준* 또는 *엔터프라이즈* 플랜을 선택할 수 있습니다. *엔터프라이즈* 플랜을 사용하면 {{site.data.keyword.composeForScyllaDB}} 인스턴스를 사용 가능한 {{site.data.keyword.composeEnterprise}} 클러스터에 프로비저닝할 수 있습니다. {{site.data.keyword.composeEnterprise}}는 엔터프라이즈 준수에 필요한 보안 및 격리를 제공하며 전용 네트워킹을 사용하여 배치된 데이터베이스의 성능을 보장합니다. 세부사항은 [Compose Enterprise 문서](../ComposeEnterprise/index.html)를 참조하십시오.

## Compose for ScyllaDB 관리

서비스 대시보드에서 서비스를 관리할 수 있습니다. 여기서 {{site.data.keyword.cloud_notm}} Compose 데이터베이스 및 연결 방법에 대한 정보를 찾을 수 있습니다. 또한 다음을 수행할 수 있습니다.

- 백업 관리
- 서비스에 대한 추가 리소스 할당 
- 화이트리스트를 사용하여 데이터베이스에 대한 액세스 제한. 

자세한 정보는 [설정](./dashboard-settings.html)을 참조하십시오.

## Compose for ScyllaDB에 연결

서비스와 함께 작성된 신임 정보를 사용하거나 서비스 대시보드의 *개요* 탭에서 제공되는 연결 문자열 및 명령행을 사용하여 서비스에 연결할 수 있습니다.

## {{site.data.keyword.cloud_notm}} 애플리케이션을 Compose for ScyllaDB에 연결

{{site.data.keyword.cloud_notm}} 애플리케이션을 서비스에 연결하려면 서비스와 함께 작성되는 신임 정보를 사용하십시오. [{{site.data.keyword.cloud_notm}} 애플리케이션 연결](./connecting-bluemix-app.html)에서 {{site.data.keyword.cloud_notm}} 애플리케이션을 {{site.data.keyword.composeForScyllaDB}} 서비스에 연결하는 방법에 대한 정보를 찾을 수 있습니다.

## {{site.data.keyword.cloud_notm}} 외부에서 Compose for ScyllaDB에 연결

{{site.data.keyword.cloud_notm}} 외부에서 {{site.data.keyword.composeForScyllaDB}}에 연결하려는 경우 제공된 연결 문자열 또는 명령행을 사용할수 있습니다. [외부 애플리케이션 연결](./connecting-external.html)에서 연결 방법에 대한 정보를 찾을 수 있습니다.
