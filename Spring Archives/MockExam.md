### Java based or annotation based configuration
- @Bean
- @Configuration
- @Scope
### Spring AOP CGLIB proxying
- final methods cannot be advised
- CGLIB classes are in spring-core jar
- proxy-target-class="true" forces the use of CGLIB
### after all the bean properties have been set (DI is done)
- methods with @PostConstruct
- afterPropertiesSet() in InitializingBean interface
- methods in init-method attribute in XML are called
### destruction customization ways (in order)
- @PreDestroy
- destroy() in DisposableBean interface
- destroy-method in XML

### __ enables the registering a bean
> 'profile' attribute in xml `<beans>`
> @Profile
> 
### AOP advices
> first parameter of the @Around advice method must be of type ProceedingJoinPoint
> `@AfterThrowing` `throwing` attribute matches the exception type as `Exception`
`@AfterThrowing(pointcut="com.xyz.myapp.SystemArchitecture.dataAccessOperation()", throwing="Exception")`

### DataAccessException
- helps convert proprietary checked exceptions to a set of runtime exceptions
- allows switching between persistence technologies easily and coding without worring about catching exceptions specific to each technology
- DataAccessException wrap the original exception

### Security authentication
- the **principal** mostly is a **UserDetails** object from the UserDetailsService is used to build the Authentication object that is stored in the SecurityContextHolder.

### DriverManagerDataSource
```java
DriverManagerDataSource myDataSource = new DriverManagerDataSource();
myDataSource.setDriverClassName(.....);
myDataSource.setUrl(.....);
myDataSource.setUsername(.....);
myDataSource.setPassword(.....);
```

### `<beans><import>`
- the / is ignored
- resource not resources
- accept relative path or absolute path
- bean element is needed
```java
<beans>
	<import resource="/services.xml">
	<import resource="../services.xml">
	<bean id="bean1"/>
```
### @RequestMapping
- DispatcherServlet supports GET, HEAD, POST, PUT, PATCH and DELETE only. 
- DispatcherServlet will process TRACE and OPTIONS with the default HttpServlet behavior
```java
@RequestMapping(method=RequestMethod.PATCH)
@RequestMapping("/")
@RequestMapping("/offline")
@RequestMapping("/{systemId}")
```
### @Autowired 
- fields
- constructors
- methods

### TransactionTemplate
- assist in programmatic transaction demarcation and transaction exception handling.
- executes code within a transaction using TransactionCallback interface

### Java interfaces
make it easy to mock / test

### Spring transactions
- `setRollbackOnly()` on the TransactionStatus object to roll back the current transaction.
- declarative transaction support is enabled via AOP proxies

### JpaRepository
- it can be extended to create a repository
- to activate a repository
```java
@Configuration
@EnableJpaRepositories(basePackages="myPackage.myInstantRepos")
public class MyInstantRepoConfiguration {}
```
### in microservices systems
- There is no specific protocol required for communication.
- lots of time, REST is a preferred one

### in web.xml
- declare the root application context
```java
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener
	</listener-class>
</listener>
```
- declare a dispatcherServlet
```java
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
```

### Spring Test module
- JUnit supported
- consistent caching of context during test execution
- mock objects are provided

### Qualifier
```java
@Autowired
@Weekend(timeOfDay="Morning")
private Greeter greeter;
```
- @Weekend is a custom qualifier
```xml
<qualifer type="Weekend">
	<attribute key="timeOfDay" value="Morning"/>
</qualifer>

<meta key="timeOfDay" value="Morning"/>
```
### Autowired
- autowire Map when the key type is String
- there is no searchBy attribute
```java
@Autowired(required=true)
public MySource(MyConnection myConnection){}

@Autowired
private MyFile[] myFiles;
```
### jdbcTemplate
- update returns the number of rows affected
- when 1 row is inserted, then 1 will be returned
- throws runtime exception DataAccessException
```java
this.jdbcTemplate.update("insert into Account (accountId, name) values (?, ?)",1, "John");
```
### @PathVariable
```java
@RequestMapping(value="/{id}")
public String viewUserAddress(@PathVariable String id, Model m){}
```
### EntityManager
@PersistenceUnit can be annotated on a property whose type is EntityManagerFactory. an implementation of a entitymanagerFactory can create an entityManager and use it to perform database operations.

### @EnableWebMVC
- enables mvc java config
- is in a class annotated with @Configuration

### lookup-method
- when singleton calls prototype bean
- the prototype bean method is overriden as look-up method to get a new instance every time required
```xml
<!-- a stateful bean deployed as a prototype (non-singleton) -->
<bean id="command" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- inject dependencies here as required -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="command"/>
</bean>
```
### Spring boot externalize configuration
- properties
- YAML
- environment variables
### `<property name="age" value="5"`
- internally, PropertyEditor converts string to actual type i.e. integer
### context which loads xml files
```java
ClassPathXmlApplicationContext
FileSystemXmlApplicationContext
```
### Embedded containers in Spring boot
- Tomcat
- Jetty
- Undertow
### xml configuration value
- spring supports Collection types been convered from "...", done by reflection
- "" is treated as empty string
- `<null/>` represents null
```java

```
```java
```
```java
```
```java
```
```java
```
```java
```
```java
```
```java
```
```java
```
