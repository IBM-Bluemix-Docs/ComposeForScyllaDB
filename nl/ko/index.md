---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.composeForScyllaDB}} 정보
{: #about-compose-for-scylladb}

ScyllaDB는 Cassandra 와이드 컬럼 분산 데이터베이스를 위치 내에서 대체합니다. ScyllaDB는 벤치마크에서 성능을 10배 향상시킬 수 있는 더 효율적인 리소스 사용을 위해 Cassandra의 Java가 아니라 C++로 작성됩니다. Cassandra 도구 및 데이터 파일과의 호환성을 유지하는 외에도 ScyllaDB는 자체 튜닝 기능을 추가합니다. {{site.data.keyword.composeForScyllaDB_full}}는 ScyllaDB의 기능을 확장하여 ScyllaDB를 관리하고 고가용성 및 중복성 외에도 자동화된 백업을 제공하는 사용하기 쉬운 자동 확장 배치 시스템을 제공합니다.
{:shortdesc}

## {{site.data.keyword.composeForScyllaDB}} 서비스 인스턴스 작성

{{site.data.keyword.cloud_notm}} 카탈로그의 [{{site.data.keyword.composeForScyllaDB}} 페이지](https://console.{DomainName}/catalog/services/compose-for-scylladb/)에서 {{site.data.keyword.composeForScyllaDB}} 서비스를 작성할 수 있습니다.

서비스 이름과 서비스를 프로비저닝할 지역, 조직 및 영역을 선택하십시오. **데이터베이스 버전 선택** 필드를 사용하여 사용 가능한 데이터베이스 버전 중에서 선택할 수 있습니다.

{{site.data.keyword.composeForScyllaDB}} 인스턴스를 프로비저닝할 때 *표준* 또는 *엔터프라이즈* 플랜을 선택할 수 있습니다. *엔터프라이즈* 플랜을 사용하면 {{site.data.keyword.composeForScyllaDB}} 인스턴스를 사용 가능한 {{site.data.keyword.composeEnterprise}} 클러스터에 프로비저닝할 수 있습니다. {{site.data.keyword.composeEnterprise}}는 엔터프라이즈 준수에 필요한 보안 및 격리를 제공하며 전용 네트워킹을 사용하여 배치된 데이터베이스의 성능을 보장합니다. 세부사항은 [{{site.data.keyword.composeEnterprise}} 문서](/docs/services/ComposeEnterprise/index.html)를 참조하십시오.

## {{site.data.keyword.composeForScyllaDB}} 관리

서비스 대시보드에서 서비스를 관리할 수 있습니다. 여기서 {{site.data.keyword.cloud_notm}} Compose 데이터베이스 및 연결 방법에 대한 정보를 찾을 수 있습니다. 또한 다음을 수행할 수 있습니다.

- 백업 관리
- 서비스에 대한 추가 리소스 할당 
- 화이트리스트를 사용하여 데이터베이스에 대한 액세스 제한. 

자세한 정보는 [설정](./dashboard-settings.html)을 참조하십시오.

{{site.data.keyword.composeForScyllaDB}}는 Cloud Foundry 역할에 의존하여 서비스에 대한 액세스를 관리합니다. 개발자 역할이 있는 사용자만 서비스 대시보드를 보거나 사용할 수 있습니다. Cloud Foundry 역할에 대한 자세한 정보는 [Cloud Foundry 액세스](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess) 및 [Cloud Foundry 액세스 관리](https://console.{DomainName}/docs/iam/mngcf.html#mngcf) 페이지를 참조하십시오.
{: .tip}

## {{site.data.keyword.composeForScyllaDB}}에 연결

서비스와 함께 작성된 신임 정보를 사용하거나 서비스 대시보드의 *개요* 탭에서 제공되는 연결 문자열 및 명령행을 사용하여 서비스에 연결할 수 있습니다.

## {{site.data.keyword.cloud_notm}} 애플리케이션을 {{site.data.keyword.composeForScyllaDB}}에 연결

{{site.data.keyword.cloud_notm}} 애플리케이션을 서비스에 연결하려면 서비스와 함께 작성되는 신임 정보를 사용하십시오. [{{site.data.keyword.cloud_notm}} 애플리케이션 연결](./connecting-bluemix-app.html)에서 {{site.data.keyword.cloud_notm}} 애플리케이션을 {{site.data.keyword.composeForScyllaDB}} 서비스에 연결하는 방법에 대한 정보를 찾을 수 있습니다.

## {{site.data.keyword.cloud_notm}} 외부에서 {{site.data.keyword.composeForScyllaDB}}에 연결

{{site.data.keyword.cloud_notm}} 외부에서 {{site.data.keyword.composeForScyllaDB}}에 연결하려는 경우 제공된 연결 문자열 또는 명령행을 사용할수 있습니다. [외부 애플리케이션 연결](./connecting-external.html)에서 연결 방법에 대한 정보를 찾을 수 있습니다.
