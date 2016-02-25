为什么要使用PreparedStatement？
一、通过PreparedStatement提升性能
     Statement主要用于执行静态SQL语句，即内容固定不变的SQL语句。Statement每执行一次都要对传入的SQL语句编译一次，效率较差。
     某些情况下，SQL语句只是其中的参数有所不同，其余子句完全相同，适用于PreparedStatement。
PreparedStatement的另外一个好处就是预防sql注入攻击
     PreparedStatement是接口，继承自Statement接口。
    使用PreparedStatement时，SQL语句已提前编译，三种常用方法 execute、 executeQuery 和 executeUpdate 已被更改，以使之不再需要参数。
     PreparedStatement 实例包含已事先编译的 SQL 语句，SQL 语句可有一个或多个 IN 参数，IN参数的值在 SQL 语句创建时未被指定。该语句为每个 IN 参数保留一个问号（“？”）作为占位符。
     每个问号的值必须在该语句执行之前，通过适当的setInt或者setString 等方法提供。
     由于 PreparedStatement 对象已预编译过，所以其执行速度要快于 Statement 对象。因此，多次执行的 SQL 语句经常创建为 PreparedStatement 对象，以提高效率。
通常批量处理时使用PreparedStatement。
  
 
1 //SQL语句已发送给数据库，并编译好为执行作好准备2 PreparedStatement pstmt = con.prepareStatement(3          "UPDATE  emp   SET job= ? WHERE empno = ?");4 //对占位符进行初始化 5 pstmt.setLong(1, "Manager");6 pstmt.setInt(2,1001);7 //执行SQL语句8 pstmt.executeUpdate();


二、通过PreparedStatement防止SQL Injection
      对JDBC而言，SQL注入攻击只对Statement有效，对PreparedStatement无效，因为PreparedStatement不允许在插入参数时改变SQL语句的逻辑结构。
      使用预编译的语句对象时，用户传入的任何数据不会和原SQL语句发生匹配关系，无需对输入的数据做过滤。如果用户将”or 1 = 1”传入赋值给占位符，下述SQL语句将无法执行：select * from t where username = ? and password = ?;
      PreparedStatement是Statement的子类，表示预编译的SQL语句的对象。在使用PreparedStatement对象执行SQL命令时，命令被数据库编译和解析，并放入命令缓冲区。缓冲区中的预编译SQL命令可以重复使用。
