# Spring Boot
> An **opinionated** runtime for Spring projects
> - setup is based on the **classpath** contents
> i.e. JPA EntityManagerFactory is setuped when the implementation is in the classpath. Spring MVC is setuped when it is in the classpath.
> - setup can be overriden, but not needed mostly
> 
> Handles low-level predictable setup
> Support different Spring project types i.e. Web, Batch,  etc.

## HelloWorld Example
- **pom.xml** -- setup Spring boot dependencies
	- in **parent POM** : `spring-boot-starter-parent` & version i.e. `1.3.0.RELEASE`
	- **starter POMs** the recommended dependencies : 
	`spring-boot-starter-<module>`
	```
	spring-boot-starter-web
	```
	- **plugin**: 
	`spring-boot-maven-plugin`
- **business modules**: HelloController classes --Spring MVC **controller**
- **application.properties** 
	- configure an InternalResourceViewResolver
	 i.e. `spring.mvc.view.prefix=/WEB-INF/views
     spring.mvc.view.suffix=.jsp`
- hello.jsp --**View** 
- Application class -- **Application** launcher
```java
**@SpringBootApplication**
public class Application {
public static void main(String[] args) { 
SpringApplication.run(Application.class, args);

}
```
- execution
```java
mvn package //generates an archive file
helloApp-0.0.1-snapshot.jar//generated file with Tomcat inside
java -jar helloApp-0.0.1-snapshot.jar//runs app using command line
```
## Explain dependency , configuration and packaging
### starter POMs
```java
	spring-boot-starter-web
	spring-boot-starter-test
	spring-boot-starter-jdbc
	spring-boot-starter-jpa
	spring-boot-starter-batch
	etc...
```
### auto configuration
```java
//@Configuration
//@ComponentScan //- scan current package and its subpackages
//@EnableAutoConfiguration
@SpringBootApplication
public class MyAppConfig {  
public static void main(String[] args) {

	SpringApplication.run(MyAppConfig.class, args); 
}
```
>---work flow--->
>
| dependency |runtime, boot checks  |what it does
|--|--|--|
|starter web  | spring mvc on classpath |DispatcherServlet registered
|HSQL db dependency|HSQLDB in classpath|embedded datasource registed
|starter JDBC|if datasource is registed| JdbcTemplate registered


### packaging
- Spring Boot creates a single archive JAR/WAR
- run the jar file: `java -jar my-app.jar`
- maven plugin for packaging
`spring-boot-maven-plugin`
- `.jar.original` contains only your source code
`.jar` contains source code and libs
(with web starter dependency, Tomcat included), so it is executable

## Web application with Spring Boot
### default is embedded Tomcat & includes Jetty support
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions> 

 <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId> 

 </exclusion>
  </exclusions> 

</dependency>
<dependency> 

 <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jetty</artifactId> 

</dependency>
```
### explicitly run Spring Boot app inside a Servlet container
-  Change artifact type to WAR (instead of JAR) 
- extends SpringBootServletInitializer 
```java 
@SpringBootApplication
public class Application extends SpringBootServletInitializer{
protected SpringApplicationBuilder configure( SpringApplicationBuilder application){
	return application.sources(Application.class); 
}

public static void main(String[] args) { 
	SpringApplication.run(Application.class, args);
}

}
```
-  Override configure method
- no web.xml needed
- hybrid WAR file (executed in either Tomcat server or standalone with embedded Tomcat , 2 Tomcat versions must match)
- .war.original is a traditional WAR file
- .war is a hybrid WAR file, additionally containing the embedded Tomcat
- 
## Other features of Spring Boot
- **application.properties** is easy to be consumed via **PropertySource, Environment and @Value**
- configure DataSource bean with `spring-boot-starter-jdbc/jpa`
> Spring Boot creates the DataSource according to the properties set even the connection pool if the jar is in classpath
```properties
spring.datasource.url=jdbc:mysql://localhost/test 
spring.datasource.username=dbuser 
spring.datasource.password=dbpass 
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```
- support configuring logging levels (SLF4J preferred)
```properties
logging.level.org.springframework=DEBUG 
logging.level.com.acme.your.code=INFO
```
- YAML support
```yaml
database:
  host: localhost
  user: admin
  ```
## Web application convenience
1.    DispatcherServlet & ContextLoaderListener is covered. Spring MVC using same defaults as @EnableWebMvc (not used when web starter in classpath)
2. static resources are in `/static , /public, /resources, /META-INF/resources`
3. templates are put in `/templates` (Thymeleaf etc in classpath)
4. default `/error` mapping

## How AutoConfiguration works
-   Extensive use of pre-written @Configuration classes 
-   **@Conditional** on
	- The contents of the **classpath** 
	-  **Properties** you have set  
    - **Beans** already defined
i.e. `@Profile` is a special case of `@Conditional`

### @Conditional annotations
>-   Only create if other beans exist (or don't exist)
```java
@Bean  
@ConditionalOnBean(name={"dataSource"})  
public JdbcTemplate jdbcTemplate(DataSource dataSource) {
	return new JdbcTemplate(dataSource); 
}
@ConditionalOnBean(type={DataSource.class})
@ConditionalOnClass
@ConditionalOnProperty
@ConditionalOnMissingBean
@ConditionalOnMissingClass

```
### AutoConfiguration classes
>Pre-written Spring configurations  
>`org.springframework.boot.autoconfigure` package 
>See `spring-boot-autoconfigure` JAR file

### How to customize Spring Boot
- 1. Set some of Spring Boot's **properties**  
	-	`application.properties` in working directory/classpath/their sub-directory `/config or config package`
	-	create a `PropertySource`
>override DataSource Configuration
```properties
spring.datasource.url=            # Connection settings
spring.datasource.username=
spring.datasource.password=
spring.datasource.driver-class-name= 

spring.datasource.schema=
spring.datasource.data= 

spring.datasource.initial-size=
spring.datasource.max-active=
spring.datasource.max-idle=
spring.datasource.min-idle=
```
>Web Container Configuration
>SSL (keystore, truststore for client authentication)
>Tomcat specifics (access log, compression, etc)
```properties
server.port=9000
server.address=192.168.1.20
server.session-timeout=1800
server.context-path=/rewards
server.servlet-path=/admin
```
- 2. Define certain **beans** yourself so Spring Boot won't 
	- beans you declare explicitly disable any auto- created ones.
	i.e. Your `DataSource` stops Spring Boot creating a default `DataSource`, bean type matters.
-  3. Explicitly disable some auto-configuration  
```java
@EnableAutoConfiguration(**exclude**=DataSourceAutoConfiguration.class) 
public class ApplicationConfiguration {

... }
```
- 4. Changing **dependencies**
>to change dependencies version , set the appropriate Maven property in your pom.xml
```xml
<properties>
    <spring.version>4.2.0.RELEASE</spring.version> 
</properties>
```
### Order of evaluation of the properties (non-exhaustive) 
-  Command line arguments  
-   Java system properties
- OS environment variables
- Property file(s) – including application.properties
- rename `application.properties`
```java
//"myserver" is the renamed properties file
public static void main(String[] args) { 
	System.setProperty("spring.config.name", "myserver"); 
	SpringApplication.run(Application.class, args);

}
```
- path == PATH,  java.home == JAVA_HOME
```java
@Configuration
class AppConfig {
@Value("${java.home}") String javaInstallDir;  
...

}
```
- the usage of **@ConfigurationProperties & @EnableConfigurationProperties**
```java
@Configuration
public class RewardsClientConfiguration {

@Value("${rewards.client.host}") String host; 
@Value("${rewards.client.port}") int port; 
@Value("${rewards.client.logdir}") String logdir; 
@Value("${rewards.client.timeout}") int timeout;

... }

//1st @ConfigurationProperties
@Component @ConfigurationProperties(prefix="rewards.client") 
public class ConnectionSettings {

	private String host; 
	private int port; 
	private String logdir; 
	private int timeout; ... // getters/setters

}
//2nd @EnableConfigurationProperties
@Configuration
@EnableConfigurationProperties(ConnectionSettings.class) 
public class RewardsClientConfiguration {

// Spring initialized this automatically
@Autowired ConnectionSettings connectionSettings;
@Bean public RewardClient rewardClient() { return new RewardClient(
connectionSettings.getHost(),
connectionSettings.getPort(), ... );

} }
```
### Logging frameworks recommended practice
>stick to SLF4J interface
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
    <exclusions> 

 <exclusion>
           <groupId>ch.qos.logback</groupId>
           <artifactId>logback-classic</artifactId> 

 </exclusion>
    </exclusions> 

</dependency> 

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId> 

</dependency>
```
>Spring Boot logs to the console
>to log to files, specify file/path in application.properties
```properties
# Use only one of the following properties
# absolute or relative file to the current directory 

logging.file=rewards.log 

# will write to a spring.log file
logging.path=/var/log/rewards
```
### YAML
>Java parser for YAML is called SnakeYAML, provided by spring-boot-starters
>Multiple Profiles Inside a Single YAML File
>"- - -" implies a separation between profiles
```yaml
//used for all profiles
logging.level:
  org.springframework: INFO 
---
//only for development profile
spring.profiles: development
database: 
	host: localhost 
	user: dev 
---
//only for production profile
spring.profiles: production
database: 
	host: 198.18.200.9
	user: admin
```
### Testing in Spring Boot
#### Integration test 
```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(class=TransferApplication.class) 
public final class TransferServiceTests  {

@Autowired  
private TransferService transferService;

@Test  
public  void  successfulTransfer()  {
TransferConfirmation  conf=transferService.transfer(...);
... }

}

//the same configuration class that the application would use, but for testing purpose
@SpringBootApplication  
public  class  TransferApplication  {
	public  static  void  main(String[]  args)  { 									
	   SpringApplication.run(TransferApplication.class,  args);

} }
```
#### Web Application Testing
```java
@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration  
public  final  class  TransferServiceTests  {
...}
```
`@WebAppConfiguration` :
- Creates a **WebApplicationContext**  
 - Can test code that uses web features
	-	**ServletContext**, **Session and Request bean scopes**
-	 Configures the **location of resources**
		-	**Defaults** to `src/main/webapp  `
			-	Override using annotation's *value* attribute
		-	For **classpath** resources use `classpath:prefix`
