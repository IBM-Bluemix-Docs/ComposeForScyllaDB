---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}} 애플리케이션 연결

{{site.data.keyword.cloud}} 애플리케이션을 서비스에 연결하려면 서비스가 프로비저닝될 때 작성된 서비스 신임 정보를 사용하십시오.

## 'Hello World' 샘플 앱으로 연결

[compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) 샘플 앱은 Node.js를 사용하여 제공된 신임 정보로 {{site.data.keyword.composeForScyllaDB}} 서비스에 연결하는 방법을 보여줍니다. 이 애플리케이션은 데이터베이스를 작성하고 데이터베이스에서 읽고 씁니다.

샘플 앱을 다운로드하고 readme 파일의 지시사항에 따르십시오. 그런 다음 {{site.data.keyword.cloud_notm}}의 애플리케이션 세부사항 페이지에서 **앱 보기**를 클릭하여 *예제* 테이블의 컨텐츠를 표시하십시오.

## 사용 가능한 신임 정보

필드 이름|설명
----------|-----------
`db_type`|서비스에서 제공하는 데이터베이스의 유형입니다. 이 경우에는 `scylla`입니다.
`uri_cli_1`|데이터베이스 인스턴스에 연결하는 2차 `cqlsh` 쉘 명령행입니다.
`maps`|다양한 드라이버에서 내부 IP 주소를 ScyllaDB 데이터베이스의 외부 DNS 이름과 연관시키는 데 필요한 정보를 제공하는 ScyllaDB 연결 맵입니다.
`name`|데이터베이스 배치 이름입니다.
`uri_cli`|데이터베이스 인스턴스에 연결하는 `cqlsh` 쉘 명령행입니다.
`uri_direct_2`|서비스에 연결하는 데 사용할 수 있는 3차 URI입니다. 형식은 `uri`와 같습니다.
`uri_direct_1`|서비스에 연결하는 데 사용할 수 있는 2차 URI입니다. 형식은 `uri`와 같습니다.
`ca_certificate_base64``(선택사항)`|애플리케이션이 적합한 서버에 연결되었는지 확인하는 데 사용되는 base64로 인코딩된 자체 서명된 인증서입니다. 인증서는 Let's Encrypt 인증서가 아니라 자체 서명된 서비스에만 있습니다. 샘플 애플리케이션에 표시된 대로 사용하기 전에 키를 디코딩해야 합니다.
`deployment_id`|Compose에서 작성된 서비스의 내부 ID입니다.
`uri_cli_2`|데이터베이스 인스턴스에 연결하는 3차 `cqlsh` 쉘 명령행입니다.
`uri`|서비스에 연결할 때 사용하는 URI이며, 스키마(`scylla:`), 비밀번호, 서버의 호스트 이름, 연결할 포트 이름 및 데이터베이스 이름을 포함합니다.
{: caption="표 1. Compose for ScyllaDB 신임 정보" caption-side="top"}

**참고:** 세 개의 `haproxy` 포털에서 Scylla 서비스에 대한 액세스를 제공합니다. `uri`, `uri_direct_1` 및 `uri_direct_2` 모두 사용하여 연결할 수 있습니다. 애플리케이션에서 `uri`, `uri_direct_1` 및 `uri_direct_2`를 전환하여 연결 실패에 대한 응답을 관리하십시오.
