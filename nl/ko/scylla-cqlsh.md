---

copyright:
  years: 2018
lastupdated: "2018-05-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# cqlsh 사용
{: #using-cqlsh}

`cqlsh`를 사용하여 {{site.data.keyword.composeForScyllaDB_full}}에 직접 연결할 수 있습니다.

이 문서가 작성된 시점을 기준으로 `cqlsh`는 Python 2.7 프로그램이며 Python 3에서는 실행되지 않습니다.
{: .tip}

`cqlsh`는 Python 애플리케이션이므로 `pip install cqlsh`를 실행하여 최신 버전의 명령을 설치할 수 있습니다.

Debian(apt) 및 Red Hat(RPM) Linux 배포판에 Cassandra를 설치하는 방법에 대한 지시사항, 소스 및 2진을 제공하는 [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/)에서 다운로드하여 전체 Cassandra 배포판에서 `cqlsh`도 얻을 수 있습니다. 2진 릴리스를 다운로드하고 디렉토리에 파일의 압축을 푸십시오. 그런 다음 해당 디렉토리로 `cd`를 수행하면 `bin` 디렉토리에서 `cqlsh`를 찾을 수 있습니다.

이 도구를 로컬 디바이스로 가져오는 방법은 로컬 운영 체제에 따라 다릅니다. 가장 쉬운 방법은 운영 체제에 적합한 방법을 사용하여 최신 Cassandra 릴리스를 설치하고(최신 버전은 Scylla인 버전 2.1.8을 계속 지원함) 기본 제공 `cqlsh` 명령을 사용하는 것입니다. 

예를 들어, Mac에서는 `brew install cassandra`를 입력하여 `homebrew`로 Cassandra를 설치할 수 있습니다.

## cqlshrc 파일

`cqlsh` 명령을 실행하면 기본적으로 명령행에서 설정할 수 없는 여러 매개변수를 설정할 수 있는 `$HOME/.cassandra/cqlshrc`를 로드합니다. 기본 파일을 편집하여 매개변수를 설정하고, 변경사항이 명령행에서 대체되지 않는 경우 해당 변경사항을 모든 세션에 적용할 수 있습니다.

여러 환경에서 다중 Scylla 인스턴스를 실행 중인 경우 별도의 `cqlshrc` 파일을 작성하고 명령에 `--cqlshrc=[filename]`을 추가하여 사용할 수 있습니다.


전체 cqlshrc 파일의 예는 [cqlshrc의 Apache 샘플](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)을 참조하십시오. 

### {{site.data.keyword.composeForScyllaDB}}용 cqlshrc

서비스에 연결하기 위한 최소한의 `cqlshrc` 파일은 다음과 같습니다.

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
version=SSLv23
certfile = ~/path_to_your/lechain.pem
validate = true
```

서비스의 _연결 문자열_에 제공된 정보를 입력하고 이 파일을 `example.cqlshrc`로 저장하십시오.

이제 명령행에서 `cqlsh --ssl --cqlshrc=example.cqlshrc`를 실행하여 Scylla 데이터베이스에 로그인할 수 있습니다. `cqlshrc`에 `ssl=true`를 지정하는 옵션이 있는 경우에도 이 옵션은 무시되므로 `--ssl` 플래그를 사용해야 합니다.

## TLSv1.2

`cqlsh`는 암호화된 연결과 협상할 때 기본적으로 TLSv1.2를 설정하지 않습니다. Compose Scylla 배치에 연결하려면 `cqlsh`를 실행 중인 환경에 TLS 버전이 지정되어야 합니다. 호환성을 극대화하려면(또한 TLSv1.3용으로 향후에도 호환 가능) `cqlsh` 연결 명령 앞에 `SSL_VERSION=SSLv23`을 두거나 `export SSL_VERSION=SSLv23`을 사용하여 환경에서 `SSL_VERSION`을 SSLv23으로 설정하십시오.

또한 `cqlsh` 연결 명령 앞에 `SSL_VERSION=TLSv1_2`를 두거나 `export SSL_VERSION=TLSv1_2`를 사용하여 버전을 특정하게 TLSv1.2로 설정해야 합니다.

## 유효성 검증 없이 연결

유효성 검증은 기본적으로 사용되지만, 원격 호스트의 유효성을 검증하지 않고 TLS/SSL을 사용하여 Scylla에 연결할 수 있습니다. 그러나 이 방법은 프로덕션에는 권장되지 않습니다. 환경 변수 `SSL_VALIDATE` 또는 cqlshrc 파일의 SSL 설정을 통해 유효성 검증을 제어합니다. 환경에서 `SSL_VALIDATE`를 false로 설정(`export SSL_VALIDATE=false`)하여 유효성 검증을 사용하지 마십시오.

또는 cqlshrc 파일을 편집하여 유효성 검증을 사용하지 않게 설정할 수 있습니다.

```
[ssl]  
validate = false
```

단일 `cqlsh` 명령의 유효성 검증을 사용 안함으로 설정하려면 `cqlsh` 연결 명령 앞에 `SSL_VALIDATE=false`를 포함시키십시오. 

유효성 검증을 사용하지 않는 경우에도 TLSv1.2를 사용하도록 `cqlsh`에 알려야 합니다. 환경에 ssl 버전을 설정하지 않았고 유효성을 검증하지 않으려는 경우 연결이 다음 예제와 유사해야 합니다.

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 인증서 얻기 및 사용

cqlsh를 사용하고 원격 호스트를 확인하려면 인증서를 얻고 액세스 가능한 임의의 위치에 저장한 후 인증서의 경로를 cqlsh에 제공해야 합니다.

`cqlsh`의 모든 후속 호출에 사용하려면 환경 변수 `SSL_CERTFILE`을 저장된 인증서의 경로 및 파일 이름으로 설정하십시오. 

또는 다음을 cqlshrc 파일에 추가할 수 있습니다.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

특정 인증서를 단일 `cqlsh` 명령에만 사용하려면 cqlsh 연결 명령의 시작 부분에 `SSL_CERTFILE = ~/path_to_your/lechain.pem`을 배치하십시오. 예를 들어, 다음과 같습니다.

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## cqlsh 시작하기

`HELP`를 입력하면 쉘에 많은 기능이 있음을 알 수 있습니다. 모든 명령에는 `TAB` 완료도 있습니다. 예를 들어, 다음과 같습니다.

1. 다음을 입력하십시오. `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. 복제 클래스에 대한 선택사항을 확인해야 합니다.
2. 클러스터가 여러 데이터 센터에 걸쳐 있지 않으므로 여기서 `SimpleStrategy`를 선택하십시오.
3. `<TAB><TAB>`을 다시 누르고 `replication_factor` 값으로 3을 입력하십시오. 그런 다음, `}`로 중괄호를 닫고 다음으로 명령문을 완료하십시오. `;<enter>`.

첫 번째 키스페이스를 작성하고 클러스터에 있는 세 개의 모든 노드에 데이터를 복제하는 것을 기본값으로 설정했습니다.

```sql
scylla@cqlsh> CREATE KEYSPACE example_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3 };
scylla@cqlsh> use example_keyspace ;
scylla@cqlsh:example_keyspace> create table names (
                    ... name_id uuid,
                    ... last_name text,
                    ... first_name text,
                    ... PRIMARY KEY(name_id)
                    ... );
scylla@cqlsh:example_keyspace> 
```

이제 키스페이스를 사용할 수 있습니다.

```sql 
  USE my_new_keyspace;
```

또한 명령 프롬프트가 기본적으로 키스페이스를 사용 중임이 쉘에 표시됩니다. 모든 테이블에 키스페이스가 있어야 합니다.

쉘에서 다음 `CREATE TABLE` 명령을 입력하여 예제로 채울 테이블을 작성하십시오.

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
