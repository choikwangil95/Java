## JDBC with PostgreSQL
eclipse IDE에서 JAVA를 사용해 PostgreSQL DB date 값을 CRUD

### 1 Data
```java
  // law data
  String[][] student = {
      {"123", "Amy", "3.9", "1000"},
      {"234", "Bob", "3.6", "1500"},
      {"345", "Craig", "3.5", "500"},
      {"456", "Doris", "3.9", "1000"},
      {"567", "Edward", "2.9", "2000"},
      {"678", "Fay", "3.8", "200"},
      {"789", "Gray", "3.4", "800"},
      {"987", "Helen", "3.7", "800"},
      {"876", "Irene", "3.9", "400"},
      {"765", "Jay", "2.9", "1500"},
      {"654", "Amy", "3.9", "1000"},
      {"543", "Craig", "3.4", "2000"},
  };

  String[][] apply = {
      {"123", "Stanford", "CS", "Y"},
      {"123", "Stanford", "EE", "N"},
      {"123", "Berkeley", "CS", "Y"},
      {"123", "Cornell", "EE", "Y"},
      {"234", "Berkeley", "biology", "N"},
      {"345", "MIT", "bioengineering", "Y"},
      {"345", "Cornell", "bioengineering", "N"},
      {"345", "Cornell", "CS", "Y"},
      {"345", "Cornell", "EE", "N"},
      {"678", "Stanford", "history", "Y"},
      {"987", "Stanford", "CS", "Y"},
      {"987", "Berkeley", "CS", "Y"},
      {"876", "Stanford", "CS", "N"},
      {"876", "MIT", "biology", "Y"},
      {"876", "MIT", "marine biology", "N"},
      {"765", "Stanford", "history", "Y"},
      {"765", "Cornell", "history", "N"},
      {"765", "Cornell", "psychology", "Y"},
      {"543", "MIT", "CS", "N"},
  };

  String[][] college = {
      {"Stanford", "CA", "1000"},
      {"Berkeley", "CA", "36000"},
      {"MIT", "MA", "10000"},
      {"Cornell", "NY", "21000"},
  };
```

### 2 Create DB
```java
// Create.java

package task2;
import java.sql.*;

public class Create {
	private static final String
	    url = "jdbc:postgresql://localhost/postgres",
	    user = "postgres",
	    password = "1234";
	
	private String databaseName;
	private String query;
	Connection connection = null;
	Statement statement = null;
	
	public Create() {
		this.databaseName = null;
		this.query = null;
	}
	
	public String getDatabaseName() {
		return this.databaseName;
	}
	
	public String getQuery() {
		return this.query;
	}
	
	public void setDatabase(String databaseName ,String query) {
		this.databaseName = databaseName;
		this.query = query;
	}
	
	public void createDatabase(String databaseName,String query) {
		try {
            this.setDatabase(databaseName, query);        
            connection = DriverManager.getConnection(url,user,password);
            statement = connection.createStatement();
            String sql = "CREATE TABLE if not exists "+getDatabaseName()+getQuery();

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
	}
}
```
### 3 Insert DB
```java
// Insert.java

package task2;
import java.sql.*;

public class Insert {
	private static final String
	    url = "jdbc:postgresql://localhost/postgres",
	    user = "postgres",
	    password = "1234";
	
	private String tableName;
	private String[][] values;
	Connection connection = null;
	PreparedStatement statement = null;
	
	public Insert(String name, String[][] values) {
		this.tableName = name;
		this.values = values;
		
		try {
			 connection = DriverManager.getConnection(url,user,password);
	         String sql;
	         
	         switch(name) {
	         	case "apply":
	         		sql = "INSERT INTO "+getTableName()+" (sID, cName, major, decision) "+" VALUES(?,?,?,?)";
	                statement = connection.prepareStatement(sql);
			    	 for (int i=0; i<values.length; i++ ) {
			    		 for(int j=0; j<values[i].length; j++) {
				        	 if(j==0) {
				        		 int value = Integer.parseInt(values[i][j]);
				        		 statement.setInt(j+1, value);
				        	 }else {
				        		 statement.setString(j+1, values[i][j]);
				        	 }
			    		 }
			    		 statement.executeUpdate();
			    	 }
			    	 break;
	         	case "student":
	         		for (int i=0; i<values.length; i++ ) {
	         			sql = "INSERT INTO "+getTableName()+" (sID, sName, GPA, sizeHS) "+" VALUES(?,?,?,?)";
		                statement = connection.prepareStatement(sql);
	         			for(int j=0; j<values[i].length; j++) {
				        	 if(j==1) {
				        		 statement.setString(j+1, values[i][j]);
				        	 }
				        	 if(j==0 || j==3){
				        		 String valueString = values[i][j];
				        		 int valueInt = Integer.parseInt(valueString);
				        		 statement.setInt(j+1, valueInt);
				        	 }
				        	 if(j==2){
				        		 String valueString = values[i][j];
				        		 double valueDouble = Double.parseDouble(valueString);
				        		 statement.setDouble(j+1, valueDouble);
				        	 }
	         			}
	         			statement.executeUpdate();
			    	 }
			    	 break;
	         	case "college":
	         		sql = "INSERT INTO "+getTableName()+" (cName, state, enrollment) "+" VALUES(?,?,?)";
	                statement = connection.prepareStatement(sql);
	         		for (int i=0; i<values.length; i++ ) {
	         			for(int j=0; j<values[i].length; j++) {
				        	 if(j==2) {
				        		 int value = Integer.parseInt(values[i][j]);
				        		 statement.setInt(j+1, value);
				        	 }else {
				        		 statement.setString(j+1, values[i][j]);
				        	 }
	         			}
	         			statement.executeUpdate();
			    	 }
			    	 break;
	        }
	         
	        System.out.println(getTableName()+"테이블에 INSERT 쿼리가 실행되었습니다");
	        
			} catch(SQLException sqlEX) {
	            System.out.println(sqlEX);
	        } finally {	// 쿼리문으로 데이터 출력하면 이렇게 닫아준다
	            try {
	                connection.close();
	                statement.close();
	            } catch (SQLException sqlEX) {
	                System.out.println(sqlEX);
	            }
	        }
	}
	
	public String getTableName() {
		return this.tableName;
	}
	
	public String[][] getValues() {
		return this.values;
	}
}
```

### 4 Select DB 
```java
// Select.java

package task2;
import java.sql.*;

public class Select {
	private static final String
	    url = "jdbc:postgresql://localhost/postgres",
	    user = "postgres",
	    password = "1234";

	private String databaseName;
	private String query;
	Connection connection = null;
	Statement statement = null;
	ResultSet results = null;
	
	public Select(String[] name, String query) {
		try {
		connection = DriverManager.getConnection(url,user,password);
        statement = connection.createStatement();
        String sql = query;
        
    	switch(name[0]) {
	     	case "apply":
	     		int sID;
	     		String cName, major, decision;
	     		
	     		System.out.println("sID  cName    major  decision");
	     		System.out.println("---------------------------");
	     		results = statement.executeQuery(sql);
	     		
	     		while(results.next()) {
	     			sID = results.getInt("sID");
	     			cName = results.getString("cName");
	     			major = results.getString("major");
	     			decision = results.getString("decision");
	     			System.out.printf("%d  %s   %s     %s%n", sID, cName, major, decision);
	     		}
	     		
		    	 break;
	     	case "student":
	     		int sID2, sizeHS;
	     		double GPA;
	     		String sName;
	     		results = statement.executeQuery(sql);

	     		System.out.println("sID  cName  GPA  sizeHS");
	     		System.out.println("---------------------------");

	     		while(results.next()) {
	     			sID2 = results.getInt("sID");
	     			sName = results.getString("sName");
	     			GPA = results.getDouble("GPA");
	     			sizeHS = results.getInt("sizeHS");
	     			System.out.printf("%d  %s   %1.1f   %d%n", sID2, sName, GPA, sizeHS);
	     		}
		    	 break;
		    	 
	     	case "college":
	     		int enrollment;
	     		String cName2, state;
	     		results = statement.executeQuery(sql);
	     		
	     		System.out.println("cName  state  enrollment");
	     		System.out.println("---------------------------");
	     		
	     		while(results.next()) {
	     			cName2 = results.getString("cName");
	     			state = results.getString("state");
	     			enrollment = results.getInt("enrollment");
	     			System.out.printf("%s  %s   %d%n", cName2, state, enrollment);
	     		}
		    	 break;
    	}
        
        
        
		}catch (SQLException sqlEX) {
            System.out.println(sqlEX);
        } finally {	// 쿼리문으로 데이터 출력하면 이렇게 닫아준다
            try {
                connection.close();
            } catch (SQLException sqlEX) {
                System.out.println(sqlEX);
            }
        }
	}
        
	public Select(String[] name, String query, int num) {
		try {
			connection = DriverManager.getConnection(url,user,password);
	        statement = connection.createStatement();
	        String sql = query;
	        int number = num;
	        
	        switch(number) {
	        	case 1:
	        		int sID, sizeHS;
		     		String cName, major, decision, sName;
		     		double GPA;
		     		results = statement.executeQuery(sql);
		     		
		     		System.out.println("sID  sName  count(distinct cName)");
		     		System.out.println("---------------------------------");
		        	
		     		while(results.next()) {
		     			sID = results.getInt("sID");
		     			sName = results.getString("sName");
		     			
		     			System.out.printf("%d  %s  %n", sID, sName);
		     		}
		     		break;
	        	case 2:
		     		String major2;
		     		results = statement.executeQuery(sql);
		     		
		     		System.out.println("major");
		     		System.out.println("-----");
		     		
		     		while(results.next()) {
		     			major2 = results.getString("major");
		     			
		     			System.out.printf("%s%n", major2);
		     		}
	        		break;
	        	case 3:
	        		String sName2;
		     		double GPA2;
		     		results = statement.executeQuery(sql);
		     		
		     		System.out.println("sName  GPA");
		     		System.out.println("-----------");
		     		
		     		while(results.next()) {
		     			sName2 = results.getString("sName");
		     			GPA2 = results.getDouble("GPA");
		     			
		     			System.out.printf("%s  %1.1f%n", sName2, GPA2);
		     		}
	        		break;
	        }
		}catch (SQLException sqlEX) {
	        System.out.println(sqlEX);
	    } finally {	// 쿼리문으로 데이터 출력하면 이렇게 닫아준다
	        try {
	            connection.close();
	        } catch (SQLException sqlEX) {
	            System.out.println(sqlEX);
	        }
	    }
	}
}
```
### 5 TestCase
```java
// TestSQL.java

// JDBC를 이용해 PostgreSQL 서버 및 데이터베이스 연결
      String url = "jdbc:postgresql://localhost/postgres";
      String user = "postgres";
      String password = "1234";

      Scanner in = new Scanner(System.in);
      System.out.println("SQL Programming Test");
      System.out.println("Connecting PostgreSQL database");
      Connect connectDB = new Connect(url, user, password);

      // 3개 테이블 생성: Create table문 이용
      Create studentTable = new Create();
      Create applyTable = new Create();
      Create collegeTable = new Create();
      collegeTable.createDatabase(" College", "(cName varchar(20), state char(2), enrollment int, primary key(cName));");
      studentTable.createDatabase(" Student", "(sID int, sName varchar(20), GPA numeric(2,1), sizeHS int, primary key(sID));");
      applyTable.createDatabase(" Apply", "(sID int, cName varchar(20), major varchar(20), decision char, primary key(sID, cName, major));");
      System.out.println("");

      // 3개 테이블 데이터 삽입 : Insert value 문 이용
      Insert studentInsert = new Insert("student", student);
      Insert applyInsert = new Insert("apply", apply);
      Insert collegeInsert = new Insert("college", college);
      System.out.println("");

      // Query1 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 1");
      String[] table = {null, null};
      table[0] = "apply";
      Select query1 = new Select(table, "Select * from apply");
      System.out.println("");

      // Query2 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 2");
      table[0] = "college";
      Select query2 = new Select(table, "Select * from college");
      System.out.println("");

      // Query3 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 3");
      table[0] = "student";
      Select query3 = new Select(table, "Select * from student");
      System.out.println("");

      // Query4 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 4");
      table[0] = "student";
      Select query4 = new Select(table, "select * from Student S1"
          + " where (select count(*) from Student S2"
          + " where S2.sID <> S1.sID and S2.GPA = S1.GPA) ="
          + " (select count(*) from Student S2"
          + " where S2.sID <> S1.sID and S2.sizeHS = S1.sizeHS);");
      System.out.println("");

      // Query5 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 5");
      table[0] = "student";
      table[1] = "apply";
      Select query5 = new Select(table, "select Student.sID, sName, count(distinct cName)"
          +" from Student, Apply"
          +" where Student.sID = Apply.sID"
          +" group by Student.sID, sName;", 1);
      System.out.println("");

      // Query6 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 6");
      Select query6 = new Select(table, "select major"
          +" from Student, Apply"
          +" where Student.sID = Apply.sID"
          +" group by major"
          +" having max(GPA) < (select avg(GPA) from Student);", 2);
      System.out.println("");

      // Query7 실행 및 결과 출력
      System.out.println("Continue? (Enter 1 for continue) :");
      in.nextLine();
      System.out.println("Query 7");
      System.out.println("Searching할 sizeHS, major, cName을 입력해주세요");
      int sizeHS = in.nextInt();
      String major = in.next();
      String cName = in.next();

      Select query7 = new Select(table, "select sName, GPA"
          +" from Student join Apply"
          +" on Student.sID = Apply.sID"
          +" where sizeHS < "+ sizeHS+" and major = '"+major+"' and cName = '"+cName+"';", 3);
	}

```
