#### Referece
[이론](https://dyjung.tistory.com/50),
 [Insert](https://dyjung.tistory.com/49),
 [Select](https://m.blog.naver.com/PostView.nhn?blogId=heoguni&logNo=130170490707&proxyReferer=https:%2F%2Fwww.google.com%2F),
 [Create](https://www.qctutorials.com/learning/jdbc/jdbc-create-database.html)

## JDBC
Java DataBase Connection<br/>
JAVA에서 DB 프로그래밍을 하기 위해 사용되는 API

![image](https://user-images.githubusercontent.com/48672212/98656114-a45a4780-2383-11eb-8928-e3b1b1371155.png)

### JDBC 사용
- 1 JDBC 드라이버 load 
- 2 DB 연결
- 3 DB에 데이터를 SQL문을 통해 DML 
- 4 DB 연결 종료

### 1 DB 연결 생성
```java
  // DB 정보
  String url = "jdbc:postgresql://localhost/postgres";
  String user = "postgres";
  String password = "1234";
  
  Connection connection=null;	// JDBC 연결 커넥션 선언
  Statement statement=null;	// Statement 생성 후 실행할 쿼리 정보 선언
  ResultSet resultSet=null;	// 결과 담을 resultSet 선언

  try {
	        connection = DriverManager.getConnection(url,user,password);
	        System.out.println("Connected PostgreSQL database");
	        System.out.println("");
	    }catch (SQLException sqlEX) {
	        System.out.println(sqlEX);
	    }
```

### 2 Statement를 이용한 쿼리 실행
#### 2-1 Create
```java
  try {    
        connection = DriverManager.getConnection(url,user,password);
        // statement 생성
        statement = connection.createStatement();
        // SQL 문 할당
        String sql = "CREATE TABLE if not exists "+getDatabaseName()+getQuery();

        // SQL 문 실행
        statement.executeUpdate(sql);

        System.out.println(getDatabaseName()+" Database is created successfully...");
    }catch (SQLException sqlEX) {
        System.out.println(sqlEX);
    } finally {	// 쿼리문으로 데이터 출력하면 이렇게 닫아준다
        try {
            connection.close();
        } catch (SQLException sqlEX) {
            System.out.println(sqlEX);
        }
    }
```

#### 2-2 Insert
```java
  try {
     connection = DriverManager.getConnection(url,user,password);
     String sql;
     
     // SQL 문으로 입력할 column 종류와 값 개수에 대해 할당해준다.
     sql = "INSERT INTO "+getTableName()+" (sID, cName, major, decision) "+" VALUES(?,?,?,?)";
     
     // sql을 사용할 preparedstatement를 선언 및 할당해준다
     preparedStatement statement = connection.prepareStatement(sql);
     
     // 해당 preparedStatement에 다음과 같이 값을 set 해준다
     statement.setInt(index, value);
     statement.setString();
     statement.setDouble();
     
     // 해당 SQL문을 Execute 해준다
     statement.executeUpdate();
     System.out.println(getTableName()+"테이블에 INSERT 쿼리가 실행되었습니다");
  } catch(SQLException sqlEX) {
          System.out.println(sqlEX);
      } finally {	
          try {
              connection.close();
              statement.close();
          } catch (SQLException sqlEX) {
              System.out.println(sqlEX);
          }
      }
```
#### 2-3 Select
![image](https://user-images.githubusercontent.com/48672212/98658422-5bf05900-2386-11eb-8ff2-f1481ef4064a.png)

```java
  try {
    connection = DriverManager.getConnection(url,user,password);
    
    // query 진행할 statement 생성
    statement = connection.createStatement();
    String sql = query;
    
    // statement에 진행할 query를 할당해준다
    // 그러면 결과값이 results에 저장
    results = statement.executeQuery(sql);
    
    // 저장된 값은 위의 그림처럼 한줄씩 진행되기 때문에 다음과 같이 
    while(results.next()) {
      sID = results.getInt("sID");
      cName = results.getString("cName");
      major = results.getString("major");
      decision = results.getString("decision");
      System.out.printf("%d  %s   %s     %s%n", sID, cName, major, decision);
    }
  }

```
