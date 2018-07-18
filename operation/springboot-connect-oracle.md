因为Oracle是付费软件，并且maven不维护Oracle的连接jar包，所以，需要自己手动下载maven的jar

1. 下载jar包

​    有两种方式，一种是Oracle的官方网站中下载，一种是去11g的安装目录下面拷贝

2. 使用maven命令，将jar安装到本地仓库中

​    mvn install:install-file -Dfile=D:\softwares_bak\jdbc\jdbc\lib\ojdbc6.jar -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0 -Dpackaging=jar

3. 修改pom文件

```
<dependency>

    <groupId>com.oracle</groupId>

    <artifactId>ojdbc6</artifactId>

    <version>11.2.0</version>

</dependency>
```

4. 配置连接字符串

```

spring:

    jmx: # 解决 javax.management.InstanceAlreadyExistsException 异常

        default-domain: northmeter

    datasource:

        type: com.alibaba.druid.pool.DruidDataSource

        driverClassName: oracle.jdbc.driver.OracleDriver

        druid:

            first:  #schoolmain

                url: jdbc:oracle:thin:@10.168.1.203:1521:orcl

                username: psmis

                password: ps

            second:  #schoolbase

                url: jdbc:mysql://10.168.1.200:3306/schoolbase_test?allowMultiQueries=true&useUnicode=true&characterEncoding=UTF-8&useSSL=true

                username: admin

                password: admin@2017!@#
```

5. 测试

```


public class ConnectOracle {

    public static Connection con;

    public static void testConnect() {

        try{

            Class.forName("oracle.jdbc.driver.OracleDriver");//加载驱动类

            String url="jdbc:oracle:thin:@10.168.1.203:1521:orcl";//数据库为test  IP:为本地的即可；

            String user="psmis";//用户名test

            String password="ps";//登录密码



            con= DriverManager.getConnection(url,user,password);

            System.out.println("connect success!");



        }catch(Exception e){

            e.printStackTrace();

        }

    }

    public static void main(String[] args) throws SQLException {

        testConnect();

    }



}
```

 