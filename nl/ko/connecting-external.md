---

copyright:
  years: 2017,2018
lastupdated: "2018-02-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 외부 애플리케이션 연결
{: #connecting-external-app}

{{site.data.keyword.composeForPostgreSQL_full}} 서비스의 *개요* 페이지에서 {{site.data.keyword.composeForScyllaDB_full}}에 연결하는 데 필요한 정보를 찾을 수 있습니다.

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

Python 애플리케이션에 연결하려면 [DataStax Python 드라이버](https://github.com/datastax/python-driver)를 사용하십시오. pip를 통해 설치를 수행할 수 있습니다.

```shell
pip install cassandra-driver
```

이는 Python 패키지 관리자 `pip`를 사용하여 드라이버를 가져옵니다. 이 드라이버는 TLS/SSL이 사용으로 설정된 경우 사용할 인증서와 서비스에서 인증서를 사용하는 방법에 대한 지시사항이 [LE 인증서 사용](./scylla-certificates.html) 페이지에 있다고 예상합니다. 드라이버에 필요한 다른 모든 정보는 서비스 _개요_의 _연결 문자열_에 있습니다.

다음은 명령문을 준비하고 삽입을 실행하는 Java 코드와 매우 유사하게 수행됩니다.

```python
import ssl
import uuid
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

ssl_options = {
    "ca_certs":"/path_to_cert/lecert.pem",
    "ssl_version":ssl.PROTOCOL_TLSv1_2
}

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='your_password')

cluster = Cluster(
            contact_points = [
                "portal-59b813def16d6e0014003b45224-8.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b47186-6.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b49208-7.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com"
                ],
            port = 15401,
            auth_provider = auth_provider,
            ssl_options=ssl_options)

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
