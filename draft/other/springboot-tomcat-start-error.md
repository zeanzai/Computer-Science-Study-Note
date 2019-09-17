
# SpringBoot启动和Tomcat部署出现的问题

```Java
报错：
org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:deploy (default-cli) on project api: Cannot invoke Tomcat manager: Connection reset by peer: socket write error
解决办法：修改path
    <build>
        <finalName>api</finalName><!--最终生成的文件名-->
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <configuration>
                    <port>8700</port>
                    <path>/api</path> <!--此处不能使用 / ，应该使用 /api-->
                    <url>http://IP:PORT/manager/text</url>
                    <username>northmeter</username>
                    <password>admin@2017</password>
                </configuration>
            </plugin>
        </plugins>
    </build>


报错：
call 'refresh' before multicasting events via the context: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContex
解决方法：
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <!--打包时，要把 compile 改成provided，因为SpringBoot中默认是Tomcat8的依赖，使用tomcat7部署时，依赖包会冲突，会报错，无法deploy-->
            <scope>compile</scope> 
        </dependency>


```

