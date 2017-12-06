---

copyright:
  years: 2017
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 외부 애플리케이션 연결
{: #connecting-external-app}

{{site.data.keyword.composeForPostgreSQL_full}} 서비스의 *개요* 페이지에서 {{site.data.keyword.composeForScyllaDB_full}}에 연결하는 데 필요한 정보를 찾을 수 있습니다.

`cqlsh`를 사용하여 {{site.data.keyword.composeForScyllaDB}}에 직접 연결할 수 있습니다. 이 도구를 로컬 디바이스로 가져오는 방법은 로컬 플랫폼에 따라 다릅니다. 가장 쉬운 방법은 플랫폼에 적합한 방법을 사용하여 최신 Cassandra 릴리스를 설치하고(최신 버전은 Scylla인 버전 2.1.8을 계속 지원함) 기본 제공 `cqlsh` 명령을 사용하는 것입니다.

예를 들어, Mac에서 다음을 수행하십시오.

1. `brew install cassandra`를 입력하여 `homebrew`로 Cassandra를 설치하십시오.
2. 개요 페이지에서 임의의 명령을 복사하십시오.

  ![예제 `cqlsh` 연결 문자열](./cqlsh_connection_string "예제 cqlsh 연결 문자열")

3. 명령을 쉘에 붙여넣어 실행하십시오.

  ![`cqlsh` 쉘](./cqlsh_shell.png "cqlsh 쉘")

4. `HELP`를 입력하면 쉘에 많은 기능이 있음을 알 수 있습니다. 더 좋은 점은 모든 명령에 `TAB` 완성 기능도 있다는 것입니다.
5. 다음을 입력하십시오. `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. 복제 클래스에 대한 선택사항을 확인해야 합니다.
6. 클러스터가 여러 데이터 센터에 걸쳐 있지 않으므로 여기서 `SimpleStrategy`를 선택할 수 있습니다.
7. `<TAB><TAB>`을 다시 누르고 replication_factor에 3을 입력하십시오. 그런 다음, `}`로 중괄호를 닫고 다음으로 명령문을 완료하십시오. `;<enter>`.

  방금 첫 번째 키스페이스를 작성하고 클러스터에 있는 세 개의 모든 노드에 데이터를 복제하는 것을 기본값으로 설정했습니다.

8. 이제 키스페이스를 사용할 수 있습니다.

  ```sql
  USE my_new_keyspace;
  ```

  이제 명령 프롬프트가 기본적으로 키스페이스를 사용 중임이 쉘에 표시됩니다.

  ![`CREATE KEYSPACE` 및 `USE` 실행](./images/running_create_keyspace_use.png "`CREATE KEYSPACE` 및 `USE` 실행")

  모든 테이블에 키스페이스가 있어야 합니다. 여기서 쉘에 키스페이스를 작성하면 기본적으로 `my_new_keyspace`로 설정됩니다.

  Scylla/Cassandra는 SQL과 매우 유사한 스키마를 가지는 것으로 발전했습니다. 실제로 그렇지는 않습니다. RDBMS와 다르게 여기서 행은 키 값 검색과 더 유사합니다. 마침 정의하려는 유연한 스키마가 값에 있을 뿐입니다.

9. 쉘에서 다음 `CREATE TABLE` 명령을 입력하여 예제로 채울 위치를 작성하십시오.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

## JVM에서 연결

Cassandra용 고급 드라이버 중 하나는 Java 드라이버입니다. Cassandra가 Java로 작성된다는 점을 고려하면 이는 당연합니다. 다음으로 따라오는 것은 [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) 스크립트입니다. JVM 언어에 대해서만 이용하는 사용자의 경우 Groovy에서 선택한 언어로 변환하는 과정이 비교적 간단해야 합니다.

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
@Grab('org.slf4j:slf4j-log4j12')

import com.datastax.driver.core.BoundStatement
import com.datastax.driver.core.Cluster
import com.datastax.driver.core.Host
import com.datastax.driver.core.PreparedStatement
import com.datastax.driver.core.Row
import com.datastax.driver.core.Session

import static java.util.UUID.randomUUID

Cluster cluster = Cluster.builder()
    .addContactPointsWithPorts(
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15399 ),
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15401 ),
        new InetSocketAddress("aws-us-east-1-portal6.dblayer.com", 15400 )
    )
    .withCredentials("scylla", "XOEDTTBPZGYAZIQD")
    .build()

Session session = cluster.connect("my_new_keyspace")

PreparedStatement myPreparedInsert = session.prepare(
  """INSERT INTO my_new_table(my_table_id, last_name, first_name)
     VALUES (?,?,?)""")

BoundStatement myInsert = myPreparedInsert
    .bind(randomUUID(), "Hutton", "Hays")

session.execute(myInsert)

session.close()
cluster.close()
```

시작하려면 최신 Cassandra 드라이버에서 가져옵니다.

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

모두 가져온 후 `Cluster.builder()`를 사용하여 구성을 빌드합니다. `ContactPoint` 중 하나만 연결하는 데 사용됩니다. 해당 연결에서 클러스터에 있는 다른 노드가 검색됩니다. `connect`에서 해당 `ContactPoint`에 연결할 수 없는 경우 다른 ContactPoint가 사용됩니다. 이러한 이유로 세 개를 모두 추가합니다.

`PreparedStatement`는 동일한 이름의 다른 DB 기능과 유사하므로 익숙할 수 있습니다. 이 명령문이 구문 분석되고 반복해서 사용될 준비가 되어 서버에 보류됩니다. `bind` 및 `execute`에 대한 다음 호출은 데이터를 채우고 실제 실행을 위해 서버에 전송합니다. 일회성 실행을 위한 더 단순한 방법이 있지만 이러한 유용한 기능을 강조표시하는 것이 좋습니다.

스크립트가 작동하는지를 증명하려면 `cqlsh`로 되돌아가서 테이블을 조회하십시오.
![`cqlsh`에서 `SELECT`의 결과](./images/results_select_java.png "Select의 결과")

## Python에서 연결

Java 이외의 언어에 대한 지원도 매우 견고합니다. Python이 가장 좋은 예입니다. `cqlsh`도 Python으로 작성됩니다. 따라서 여기에서 지원은 최신 상태 이상이라는 것은 분명합니다.

```shell
pip install cassandra-driver
```

이는 Python 패키지 관리자 `pip`를 사용하여 드라이버를 가져옵니다. 다음은 명령문을 준비하고 삽입을 실행하는 Java 코드와 매우 유사하게 수행됩니다. 

```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
import uuid

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='XOEDTTBPZGYAZIQD')

cluster = Cluster(
            contact_points = ["aws-us-east-1-portal9.dblayer.com"],
            port = 15401,
            auth_provider = auth_provider)

session = cluster.connect('my_new_keyspace')

my_prepared_insert = session.prepare("""
    INSERT INTO my_new_table(my_table_id, first_name, last_name)
    VALUES (?, ?, ?)""")

session.execute(my_prepared_insert, [uuid.uuid4(), 'Snake', 'Hutton'])
```

다시 확인하기 위해 동일한 `SELECT` 명령을 실행합니다.

![`cqlsh`에서 `SELECT`의 결과](./images/results_select_python.png "Select의 결과")

## Node.js에서 연결

노드 패키지 관리자(npm)를 사용하여 드라이버와 필요한 `uuid` 라이브러리를 설치하십시오.

```shell
npm install cassandra-driver
npm install uuid
```

 코드는 이전 예제와 유사합니다.

```javascript
var cassandra = require('cassandra-driver')
var authProvider = new cassandra.auth.PlainTextAuthProvider('scylla', 'XOEDTTBPZGYAZIQD')
var uuid = require('uuid')

client = new cassandra.Client({
                        contactPoints: [
                          "aws-us-east-1-portal9.dblayer.com:15399",
                          "aws-us-east-1-portal9.dblayer.com:15401",
                          "aws-us-east-1-portal6.dblayer.com:15400"
                        ],
                        keyspace: 'my_new_keyspace',
                        authProvider: authProvider});

client.execute("INSERT INTO my_new_table(my_table_id, first_name, last_name) VALUES(?,?,?)",
               [uuid.v4(), "V8", "Hutton"],
               { prepare: true },
               function(err, result) {
                 if(err) { console.error(err); }
                 console.log("success")
               });

```

다시 한 번 코드가 연결되고 삽입 명령문을 준비하여 실행합니다. SELECT 명령을 사용하여 코드를 확인하십시오.

![`cqlsh`에서 `SELECT`의 결과](./images/results_select_node.png "Select의 결과")
