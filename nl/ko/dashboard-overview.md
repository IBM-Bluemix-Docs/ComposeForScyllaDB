---

Copyright:
  years: 2017,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 서비스 개요

_개요_ 페이지에는 {{site.data.keyword.cloud}} Compose 데이터베이스에 대한 정보가 표시됩니다. 개요에는 필수 식별 정보와 현재 리소스 사용량이 포함되어 있습니다. 또한 도구와 함께 사용하거나 도구를 활용하여 데이터베이스에 연결할 수 있는 연결 문자열에 대한 섹션도 찾을 수 있습니다.

## 배치 세부사항

_배치 세부사항_ 패널에는 서비스의 세부사항이 표시됩니다.

![배치 세부사항](./images/scylla-deployment-details.png "배치 세부사항 패널의 보기")

### 유형

서비스에서 제공하는 데이터베이스의 유형과 서비스에서 사용하는 데이터베이스 버전입니다. 더 최신의 데이터베이스 버전이 사용 가능한 경우에는 알림이 서비스 대시보드의 [버전 업그레이드](/docs/services/ComposeForScyllaDB/dashboard-settings.html#upgrade-version) 섹션에 대한 링크와 함께 표시됩니다.

### ID

서비스의 내부 ID입니다.

### 사용량

서비스 플랜에서 제공되는 데이터베이스의 크기와 스토리지의 양입니다.

## 현재 작업

서비스에 대한 관리 관련 변경(스케일링, 수동 백업 작성 등)을 수행하면 작업이 시작됩니다. 작업이 실행 중인 동안에는 _개요_ 페이지에 _현재 작업_ 패널이 표시되며 작업 이름 및 진행 표시줄을 표시합니다. 작업이 완료되면 _개요_ 페이지에 _현재 작업_ 패널이 더 이상 표시되지 않습니다.

## 연결 문자열

_연결 문자열_ 패널의 다른 탭에서 서비스에 대한 각 연결 문자열을 찾을 수 있습니다.

### HTTPS

일부 클라이언트 라이브러리에서 사용될 수 있으며 다른 라이브러리가 연결하는 데 필요한 모든 정보를 포함하는 URI 형식의 연결 문자열입니다. [외부 애플리케이션 연결](./connecting-external.html)에서 연결 문자열을 사용하여 연결하는 방법을 알아볼 수 있습니다.

### 명령행

**명령행**은 올바른 매개변수와 함께 `cqlsh`를 호출하는 사전 형식화된 명령입니다. 표시되는 명령에는 필수 플래그(`--ssl` 및 `--cqlversion`)가 포함됩니다. 이 명령을 사용하려면 PostgreSQL 클라이언트 도구를 로컬 시스템에 설치해야 합니다. [외부 애플리케이션 연결](./connecting-external.html)에서 이를 수행하는 방법에 대해 자세히 알아볼 수 있습니다.

### 맵
이러한 주소 변환 맵은 고가용성이 필요한 애플리케이션에서 사용될 수 있으며 자동 검색을 사용하여 클러스터에서 노드를 찾을 수 있습니다. 주소 변환 맵은 외부 포털 주소를 이 기능이 사용되는 클라이언트 드라이버에 대한 내부 클러스터 주소로 변환합니다.

### Socks 및 nodetool
nodetool이 Scylla를 관리할 수 있도록 하기 위해, 서비스는 SOCKS 프록시로서 구성된 SSH 캡슐과 함께 제공됩니다. 프록시를 사용하려면 배치에 SSH 키를 추가해야 합니다. 더 자세한 정보는 [nodetool 사용](./scylla-nodetool.html)에서 찾아볼 수 있습니다.


## 인스턴스 관리 API

{{site.data.keyword.cloud_notm}} Compose API를 통해 {{site.data.keyword.composeForScyllaDB}} 서비스를 관리할 수 있습니다.

### 기반 엔드포인트

기반 엔드포인트는 서비스가 있는 지역과 서비스 인스턴스 ID로 구성됩니다. 이는 모든 엔드포인트의 시작 부분에 있습니다.

### 배치 ID

배치 ID는 대부분의 호출에 필요하며, 특정 배치 인스턴스를 식별합니다.

### 참조

모든 {{site.data.keyword.cloud_notm}} Compose 서비스에서 {{site.data.keyword.cloud_notm}} Compose API를 사용하는 데 대한 추가 문서 및 참조를 보려면 [{{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/)를 읽어 보십시오.
