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
- afterPropertiesSet()
- methods in init-method attribute in XML 

are called

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
<qualifer type=
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
