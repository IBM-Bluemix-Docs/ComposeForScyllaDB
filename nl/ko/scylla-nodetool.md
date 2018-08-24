---

copyright:
  years: 2017,2018
lastupdated: "2017-12-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# nodetool 사용
{: #using-nodetool}

nodetool을 사용하여 {{site.data.keyword.composeForScyllaDB_full}} 서비스를 관리하려면 먼저 SSH 키를 추가하고 연결 문자열의 _Socks_ 탭에서 SSH 명령을 사용하여 터널을 여십시오.

터널이 열리면 연결 문자열의 _Nodetool_ 탭에 표시된 형식으로 `nodetool` 명령을 사용할 수 있습니다. 제공된 명령은 Scylla 클러스터의 상태를 리턴합니다.

## 사용 가능한 nodetool 명령

Nodetool에는 다양한 명령이 있지만, 여기서는 클러스터의 자동화된 유지보수에 문제를 발생시키는 상황을 방지하기 위해 사용을 제한합니다. 사용 가능한 명령은 표에 표시됩니다.

명령|기능
----------|-----------
`cfhistograms`|SSTable 수, 읽기/쓰기 대기 시간, 파티션 크기 및 열 개수를 비롯한, 테이블에 대한 통계를 제공합니다.
`cfstats`|열 군에 대한 자세한 진단을 제공합니다.
`cleanup`|더 이상 노드에 속하지 않는 키의 즉각적 정리를 트리거합니다.
`compact`|하나 이상의 열 군에 대해 (주) 컴팩션을 강제 실행합니다.
`compactionhistory`|컴팩션 히스토리를 출력합니다.
`compactionstats`|컴팩션에 대한 통계를 출력합니다.
`describecluster`|클러스터의 이름, 스니치, 파티셔너 및 스키마 버전을 출력합니다.
`describering <keyspace>`|키 영역의 토큰 범위 정보를 표시합니다.
`flush`|하나 이상의 열 군을 비웁니다.
`help`|도움말을 출력합니다.
`getendpoints <keyspace> <cfname> <key>`|키를 소유한 엔드포인트를 출력합니다.
`gossipinfo`|클러스터의 가십 정보를 표시합니다.
`info`|노드 정보를 출력합니다.
`move <new token>`|토큰링의 노드를 새 토큰으로 이동합니다.
`netstats`|제공된 호스트(기본 연결 노드)에 대한 네트워크 정보를 출력합니다.
`proxyhistograms`|네트워크 오퍼레이션에 대한 통계 히스토그램을 출력합니다.
`repair`|하나 이상의 열 군을 복구합니다.
`ring`|토큰링 정보를 표시합니다.
`status`|클러스터 정보를 출력합니다.
`statusbackup`|ScyllaDB 백업의 상태를 출력합니다.
`statusbinary`|기본 전송(2진 프로토콜)의 상태를 출력합니다.
`statusgossip`|가십의 상태를 출력합니다.
`version`|DB 버전을 출력합니다.
{: caption="표 1. 사용 가능한 nodetool 명령" caption-side="top"}


## 차단된 nodetool 명령

다음 명령은 {{site.data.keyword.composeForScyllaDB}} 서비스에 적용되는 경우 nodetool에서 사용할 수 없도록 차단되었습니다.

- clearsnapshot
- decommision
- disablebackup
- disablebinary
- disablegossip
- drain
- enablebackup
- enablebinary
- enablegossip
- getlogginglevels
- listsnapshots
- rebuild
- refresh
- removenode
- setlogginglevel
- settraceprobability
- snapshot
- stop
