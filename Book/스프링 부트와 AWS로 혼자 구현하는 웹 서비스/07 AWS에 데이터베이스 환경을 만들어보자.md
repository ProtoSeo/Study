# 07 AWS에 데이터베이스 환경을 만들어보자 - AWS RDS
웹 서비스의 백엔드를 다룬다고 했을 때 **애플리케이션 코드를 작성하는 것 만큼 중요한 것이 데이터베이스를 다루는 일**이다.   
이번 장에서는 데이터베이스를 구축하고 앞 장에서 만든 EC2서버와 연동을 해보자. 다만, **직접 데이터베이스를 직접 설치하지 않는다.** 직접 데이터베이스를 설치해서 다루게 되면 모니터링, 알람, 백업, HA 구성 등을 모두 직접 해야한다. 처음 구축할 때 며칠이 걸릴 수 있다.   
AWS에서는 앞에서 언급한 작업을 모두 지원하는 **관리형 서비스**인 RDS(Relational Database Service)를 제공한다.   
RDS는 AWS에서 지원하는 클라우드 기반 관계형 데이터베이스이다. 하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업과 같이 잦은 운영 작업을 자동화하여 개발자가 개발에 집중할 수 있게 지원하는 서비스이다.   
추가로 **조정 가능한 용량**을 지원하여 예상치 못한 양의 데이터가 쌓여도 비용만 추가로 내면 정상적으로 서비스가 가능하다.

## 7.1 RDS 인스턴스 생성하기
1. 서비스 탭에서 RDS 대시보드를 들어가서 **데이터베이스 생성**버튼을 클릭한다.
2. RDS 생성과정이 진행된다. *표준생성*, *MariaDB*, *프리티어*를 선택한다.
- RDS에는 Aurora,오라클, MSSQL, PostgreSQL,MySQL, MariaDB 등이 있다.
- **본인이 가장 잘 사용하는 데이터베이스**를 고르면 되지만, 꼭 다른 데이터베이스를 선택할 이유가 있는 것이 아니라면, **MySQL, MariaDB, PostgreSQL**중에 고르길 추천한다.
> MariaDB의 MySQL대비 장점
> - 동일 하드웨어 사양으로 MySQL보다 향상된 성능
> - 좀 더 활성화된 커뮤니티
> - 다양한 기능
> - 다양한 스토리지 엔진
3. 상세 설정
    - **상세 설정**
        + DB 인스턴스 식별자
        + 마스터 사용자 이름
        + 마스터 암호
    - **스토리지**
        + 스토리지 유형
        + 할당된 스토리지
    - **네트워크 및 보안**
        + VPC
        + 서브넷 그룹
        + 퍼블릭 액세스 가능
    - **추가 구성**
        + 초기 데이터베이스 이름

## 7.2 RDS 운영환경에 맞는 파라미터 설정하기
RDS를 처음 생성하면 몇 가지 설정을 필수로 해야한다. 우선 다음 3개의 설정을 차례로 진행해보자.
- 타임존
- Character Set
- Max Connection
1. 왼쪽 메뉴탭에서 **파라미터 그룹**을 클릭한다.
2. 화면 오른쪽 위의 **파라미터 그룹 생성**버튼을 클릭한다.
3. 파라미터 그룹 세부정보를 작성한다.
    - 파라미터 그룹 패밀리 = 방금 생성한 MariaDB와 같은 버전을 맞춰준다.
    - 그룹 이름
    - 설명
4. 생성된 그룹을 클릭한 뒤, **파라미터 편집**버튼을 눌러서 편집모드로 전환한다.
5. 설정값들을 변경한다.
    - time_zone = Asia/Seoul
    - character_set_client = utf8mb4
    - character_set_connection = utf8mb4
    - character_set_database = utf8mb4
    - character_set_filesystem = utf8mb4
    - character_set_results = utf8mb4
    - character_set_server = utf8mb4
    - collation_connection = utf8mb4_general_ci
    - collation_server = utf8mb4_general_ci
    - max_connections = 150
6. 오른쪽 위의 **변경 사항 저장**버튼을 클릭해 최종 저장한다.
7. 이렇게 생성된 파라미터 그룹을 데이터베이스에 연결한다.
8. 생성한 데이터베이스에 **수정**버튼을 누르고 **데이터베이스 옵션**에 가서 **DB 파라미터 그룹** 위에서 생성한 파라미터 그룹으로 수정한다.
9. **저장**을 누르고 나면 수정 사항이 요약된 것을 볼 수 있다. 여기서 반영 시점을 **즉시 적용**으로 한다.
10. 간혹 파라미터 그룹이 제대로 반영되지 않을 수도 있으므로 정상 적용을 위해 한 번 더 재부팅을 진행한다.

## 7.3 내 PC에서 RDS 접속해보기
1. 로컬PC에서 RDS에 접근하기 위해서 **RDS의 보안 그룹에 본인 PC의 IP를 추가**한다. 
2. RDS의 세부정보 페이지에서 **보안 그룹**항목을 클릭한다.
3. 브라우저에 다른 창에서 **EC2에 사용된 보안 그룹의 그룹 ID**를 복사한다.
4. 복사된 보안 그룹 ID와 본인의 IP를 **RDS 보안 그룹의 인바운드**로 추가한다.
    - 보안 그룹 첫 번째 줄 : 현재 내PC의 IP를 등록한다.
    - 보안 그룹 두 번째 줄 : EC2의 보안 그룹을 추가한다.
> 이렇게 하면 EC2와 RDS 간에 접근이 가능하다.
### Database 플러그인 설치
인텔리제이에 Database 플러그인을 설치해서 진행한다.
1. Action 검색에서 `plugins`를 검색한다.
2. `Database Browser`를 실행한다.
- 인텔리제이 유료 버전을 사용하면 공식적으로 강력한 기능의 데이터베이스 기능을 사용할 수 있다.

---
1. 오른쪽 탭에 **Database**탭을 누른다.
2. +버튼을 누르고 **Data Source**버튼을 누른뒤, 여기서 사용하려는 DB를 선택한다.
3. RDS 접속 정보를 등록한다.
    - Name
    - Comment
    - Host(RDS에서 복사한 엔드포인트를 입력한다.)
    - User
    - Password
4. **Test Connection** 버튼을 클릭해서 연결 테스트를 해본다.
5. 연결이 되었다면, Apply -> OK 버튼을 차례로 눌러 최종 저장을 한다.
6. 위쪽에 **Open SQL Console**버튼을 클릭하고 **New SQL Console**항목을 선택해서 SQL을 실행할 콘솔창을 연다.
7. `use AWS RDS 웹 코솔에서 지정한 데이터베이스 명` 을 통해서 쿼리가 수행될 database를 선택한다.
```sql
use webserver_db;
```
- completed 메시지가 떴다면 쿼리가 정상적으로 수행된 것이다.
8. 데이터베이스가 선택된 상태에서 **현재의 character_set, collation**설정을 확인한다. 
```sql
show variables like 'c%'
```
- 쿼리 결과를 보면 다른 필드들은 모두 `utf8mb4`가 잘 적용되었는데, `character_set_database`,`collation_connection` 2가지 항목이 `latin1`으로 되어있다.
9. 이 2개의 항목을 MariaDB에서만 RDS 파라미터 그룹으로 변경이 안되므로 직접 변경한다.
```sql
ALTER DATABASE webserver_db
CHARACTER SET = 'utf8mb4'
COLLATE = 'utf8mb4_general_ci'
```
- 이후 `show variables like 'c%'` 명령어로 다시 확인해 본다.
10. 타임존도 확인해 본다.
```sql
select @@time_zone, now()
```
11. 한글명이 잘 들어가는지 간단한 테이블 생성과 insert쿼리를 실행해 본다.
```sql
CREATE TABLE test(
    id bigint(20) NOT NULL AUTO_INCREMENT,
    content varchar(255) DEFAULT NULL,
    PRIMARY KEY (id)
)ENGINE =InnoDB;

insert into test(content)values ('테스트');

select * from test;
```
## 7.4 EC2에서 RDS에서 접근 확인
1. EC2에 ssh접속을 한다.
2. `sudo yum install mysql`명령어를 통해서 MySQL CLI를 설치한다.
3. 설치가 다 되었으면 로컬에서 접근하듯이 계정, 비밀번호, 호스트 주소를 사용해 RDS에 접속한다.
> `mysql -u 계정 -p -h Host주소`
```
$ mysql -u admin -p -h ...
Enter password:
# 비밀번호를 입력한다.
```
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 31
Server version: 10.4.13-MariaDB-log Source distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
4. 실제로 생성한 RDS가 맞는지 간단한 쿼리를 실행해본다.
```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| innodb             |
| mysql              |
| performance_schema |
| webserver_db       |
+--------------------+
5 rows in set (0.00 sec)
```