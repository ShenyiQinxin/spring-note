# Dependency Injection
POJOs + Configs --> SpringApplicationContext --> working application
## accessing a bean
```java
// Classic way: cast is needed

TransferService ts1 = (TransferService) context.getBean(“transferService”); 

// Use typed method to avoid cast

TransferService ts2 = context.getBean(“transferService”, TransferService.class);

// No need for bean id if type is unique

TransferService ts3 = context.getBean(TransferService.class);

@Bean  
@Description("Handles all transfer related use-cases") 
public  TransferService  transferService()  {  ...  }
```
## Spring quick start
- Application Classes
```java
public  class  TransferServiceImpl  implements  TransferService  { 
public  TransferServiceImpl(AccountRepository  ar)  {

this.accountRepository  =  ar; }

... }
```
- Configuration Instructions
```java
@Configuration
public class ApplicationConfig{
@Bean public TransferService transferService(){...}
@Bean public AccountRepository accountRepository(){...}
}
```
- Creating and Using the Application
```java
// Create the application from the configuration
ApplicationContext context = SpringApplication.run( ApplicationConfig.class );
// Look up the application service interface
TransferService service =  
(TransferService) context.getBean(“transferService”);
// Use the application
service.transfer(new MonetaryAmount(“300.00”), “1”, “2”);
```
## Creating an application context
> Spring application contexts can be bootstrapped in any environment, including
– JUnit system test  
– Web application  
– Standalone application

i.e. Instantiating Within a System (Integration) Test
```java
//Bootstraps the system to test the system
public  class  TransferServiceTests  { private  TransferService  service;

@Before  public  void  setUp()  {  
//  Create  the  application  from  the  configuration 
ApplicationContext  context  =

SpringApplication.run(  ApplicationConfig.class  ) 
//  Look  up  the  application  service  interface  
service  =  context.getBean(TransferService.class);

}


@Test  public  void  moneyTransfer()  { 
Confirmation  receipt  =

service.transfer(new  MonetaryAmount("300.00"),  "1",  "2")); 
Assert.assertEquals("500.00",  receipt.getNewBalance());

}
```
##  Multiple Configuration Files
>-   Best practice: separate out “application” beans (Service beans, application Repository beans) from “infrastructure” beans(Datasource)
```java
@Configuration  
@Import({InfrastructureConfig.class,  WebConfig.class  }) 
public  class  ApplicationConfig  {
... }

@Configuration  
public  class  InfrastructureConfig  {
... }
```
>It is not illegal to define the same bean more than once – You get the last bean Spring sees defined
## Bean scope
- Singleton : every getBean() returns the only one single instance
```java
@Bean  
@Scope(“singleton”)  
public AccountService accountService() {

return ...
```
- prototype : New instance created every time bean is referenced
- session : A new instance is created once per user session - web environment only
- request : A new instance is created once per request –web environment only

## Dependency Injection Summary

-   Your object is handed what it needs to work  
    – Frees it from the burden of resolving its dependencies – Simplifies your code, improves code reusability
    
-   Promotes programming to interfaces  
    – Conceals implementation details of dependencies
    
-   Improves testability  
    – Dependencies easily stubbed out for unit testing
    
-   Allows for centralized control over object lifecycle – Opens the door for new possibilities

## Setting property values
-   **Environment** object used to obtain properties from runtime environment
	-   Properties from many sources: 
	– JVM System Properties  
    – Java Properties Files  
    – Servlet Context Parameters
    – System Environment Variables 
    – JNDI
```java
@Configuration  
public  class  DbConfig  {
//property names
private  static  final  String  DB_DRIVER  =  "db.driver"; 
private  static  final  String  DB_URL  =  "db.url"; 
private  static  final  String  DB_USER  =  "db.user"; 
private  static  final  String  DB_PWD  =  "db.password";

@Autowired  public  Environment  env;

@Bean  public  DataSource  dataSource()  {  
DataSource  ds  =  new  BasicDataSource(); 
//Fetch property values from environment variables
ds.setDriverClassName(  env.getProperty(  DB_DRIVER  )); 
ds.setUrl(  env.getProperty(  DB_URL  ));  
ds.setUser(  env.getProperty(  DB_USER  )); 
ds.setPassword(  env.getProperty(  DB_PWD  ));  
return  ds;

} }


```

- Property Sources
	- Environment obtains values from “property sources” 
	- Environment Variables and Java System Properties always populated automatically  
	- **@PropertySource** contributes additional properties 
	- Available resource prefixes: classpath: file: http:
```java
@Configuration  
@PropertySource(“classpath:/com/organization/config/app.properties”  ) 
@PropertySource(“file:config/local.properties”  )  
public class ApplicationConfig  {
...
```
- Accessing Properties using **@Value**
	- better than Environment
```java
@Configuration
//app-dev.properties; app-qa.properties; app-prod.properties
@PropertySource(“classpath:/com/acme/config/app-${ENV}.properties”)  
public  class  DbConfig  {

@Bean  
public  DataSource  dataSource(

@Value("${db.driver}")  String  driver, 
@Value("${db.url}")  String  url, 
@Value("${db.user}")  String  user, 
@Value("${db.password}")  String  pwd)  {

DataSource  ds  =  new  BasicDataSource(); 
ds.setDriverClassName(  driver); 
ds.setUrl(  url);  
ds.setUser(  user);
ds.setPassword(  pwd  ));

return  ds; }

}
```
- **PropertySourcesPlaceholderConfigurer**
	- a static bean to resolve ${..} placeholders
```java
@Bean
public static PropertySourcesPlaceholderConfigurer pspc() { 
return new PropertySourcesPlaceholderConfigurer();

}
```
## @Profile
Beans can be grouped into Profiles  
– Profiles can represent purpose: “web”, “offline”  
– Or environment: “dev”, “qa”, “uat”, “prod”  
– Beans with no profile always available
```java
@Configuration
@Profile("dev")  //all the methods in this class are under dev profile
public class DevConfig {

@Bean(name="dataSource")
@Profile("dev")//profile of this method
public DataSource dataSource() {
...
```
### Ways to Activate Profiles
>Profiles must be activated at run-time 
>-  System property via command-line
`Dspring.profiles.active=dev,jpa`
>- System property programmatically
```java
System.setProperty("spring.profiles.active", "dev,jpa"); 
SpringApplication.run(AppConfig.class);
```
>- Integration Test: Use **@ActiveProfiles** (later section)  
```java
System.setProperty("spring.profiles.active", "dev,jpa"); 
SpringApplication.run(AppConfig.class);
```
### Spring Expression Language
- Using @Value
```java
@Configuration

class TaxConfig {
//Set an attribute then use it
**@Value("#{ systemProperties['user.region'] }")** String region;
@Bean public TaxCalculator taxCalculator1() { 
return TaxCalculator tc =new TaxCalculator( region );

}
//Pass as a bean method argument
@Bean public TaxCalculator taxCalculator2(**@Value("#{ systemProperties['user.region'] }")** String region, ...) {
return TaxCalculator tc = new TaxCalculator( region ); 
}

@Configuration
class AnotherConfig {
@Bean public StrategyBean strategyBean() { return new StrategyBean();}
//Accessing Spring Bean
**@Value("#{strategyBean.keyGenerator}")** KeyGenerator kgen;

... }

class StrategyBean {
private KeyGenerator gen = new KeyGenerator.getInstance("Blowfish"); 
public KeyGenerator getKeyGenerator() { return gen; }}
```
- Accessing Properties
```java
@Value("${daily.limit}") int  maxTransfersPerDay;
//the same with
@Value("#{environment['daily.limit']}") int  maxTransfersPerDay;
@Value("#{new  Integer(environment['daily.limit'])  *  2}") 
@Value("#{new  java.net.URL(environment['home.page']).host}")
```
-   EL Attributes can be:  
    – Spring beans (like strategyBean) – Implicit references
    
    -   Spring's environment, systemProperties, systemEnvironment available by default
        
    -   Others depending on context
        
-   SpEL allows to create custom functions and references
    
    – Widely used in Spring projects  
    • Spring Security, Spring WebFlow • Spring Batch, Spring Integration
    
    – Each may add their own implicit references
## Proxying
>Singletons Require Proxies
>Singleton instance is cached by the ApplicationContext
```java 
@Configuration
public class AppConfig {  
@Bean public AccountRepository accountRepository() { ... } @Bean public TransferService transferService() { ... }
}
//inheritance based proxy
public  class  AppConfig$$EnhancerByCGLIB$  extends  AppConfig  { public  AccountRepository  accountRepository()  {  //  ...  }  
public  TransferService  transferService()  {  //  ...  }

}
```

## Annotation-based Configuration 
- Explicit Bean Definition - `@Configuration, @Bean`
- Implicit Configuration - `@Component, @Autowired, @ComponentScan(“com.bank”)`
- Components are scanned at startup. JAR dependencies also scanned!
`@ComponentScan  (  {  "com.bank.app.repository", "com.bank.app.service",  "com.bank.app.controller"})`
- @Autowired 
>@Autowired(required=true) exception if no dependency found 
>@Autowired(required=false) Only inject if dependency exists
```java
@Autowired

Optional<AccountService> accountService;

public void doSomething() { accountService.ifPresent( s -> {

// s is the AccountService instance,

// use s to do something

}); }
```
>Constructor-injection
```java
@Autowired  
public  TransferServiceImpl(AccountRepository  a)  {
this.accountRepository  =  a; }

@Autowired  
public  TransferServiceImpl(@Value("${daily.limit}")  int  max)  {

this.maxTransfersPerDay  =  max; }
```
> setter injection
```java
@Autowired  
public  void  setAccountRepository(AccountRepository  a)  {
this.accountRepository  =  a; }

@Autowired  
public  void  setDailyLimit(@Value("${daily.limit: 100000}}")  int  max)  {

this.maxTransfersPerDay  =  max; }
```
> field injection
```java
@Autowired  
private  AccountRepository  accountRepository;

//Providing a fall-back value 100000
@Value("#{environment['daily.limit']?: 100000}") int  maxTransfersPerDay;
```
>
```java
@Component  //bean id is transferServiceImpl
public class TransferServiceImpl implements TransferService  {
@Autowired  
public  TransferServiceImpl(AccountRepository  repo)  {
this.accountRepository  =  repo; }
}

@Configuration
@ComponentScan(“com.bank”) //Find @Component classes within designated (sub)packages
public  class  AnnotationConfig  {
//  No  bean  definition  needed  any  more }
```
| constructors | setters |
|--|--|
| Mandatory dependencies | Optional / changeable dependencies |
|Immutable dependencies|Circular dependencies|
|pass several params at once|Inherited automatically|
 - @Qualifier("jdbcAccountRepository")
 >when 2 beans are defined by the same interface TransferService but differnt beanId, we use **@Qualifier("beanId")** to specify which bean to call.
 ```java
 @Component("transferService")  
public  class  TransferServiceImpl  implements  TransferService  {

@Autowired  
public  TransferServiceImpl(  @Qualifier("jdbcAccountRepository")

AccountRepository  accountRepository)  {  ...  }

@Component("jdbcAccountRepository")  
public  class  JdbcAccountRepository  implements  AccountRepository  {..}

@Component("jpaAccountRepository")  
public  class  JpaAccountRepository  implements  AccountRepository  {..}
 ```

- Component Names

	-   When not specified  
    – Names are auto-generated
    
    • De-capitalized non-qualified classname by default `MyService -> myService`
    
    • But will pick up implementation details from classname – Recommendation: never rely on generated names!
    
	-   When specified ` @Component("transferService")`
    – Allow disambiguation when 2 bean classes implement the same interface
### Annotation & Java Config
```java
@Component("transferService") 
@Scope("prototype") @Profile("dev")  @Lazy(false)
public  class  TransferServiceImpl implements  TransferService  {

@Autowired  
public  TransferServiceImpl(AccountRepository  accRep)  {  ...  }
>>>>>>>>>>>>>
@Configuration  
public  class  TransferConfiguration

	@Bean(name="transferService") 
	@Scope("prototype") @Profile("dev")  @Lazy(false)
	public  TransferService  tsvc()  { return
	new  TransferServiceImpl( accountRepository());
}
```
| Annotation |Java Config  |
|--|--|
| Nice for frequently changing beans | More verbose but Write any Java code you need |
|Only works for your own code|used for all classes (not just your own)|
|rapid development|Strong type checking enforced by compiler|
|code and config are not sparated|Is centralized in one (or a few) places|
-   Common approach:  
    – Use annotations whenever possible
    
    • Your classes  
    – But still use Java Configuration for
    
    • Third-party beans that aren't annotated • Legacy code that can't be changed
## @PostConstruct and @PreDestroy 
>method can have any visibility, must take no parameters, must return void
>Add behavior at startup and shutdown
```java
public  class JdbcAccountRepository {
private  DataSource  dataSource;

@Autowired  
public  void  setDataSource(DataSource  dataSource)

{  this.dataSource  =  dataSource;  }
//Method called at startup **after dependency all injection** 
//Called after setter methods are called
@PostConstructvoid  populateCache()  {  }
//Method called at shutdown **prior to destroying the bean instance**
//Called when a ConfigurableApplicationContext is closed – If application (JVM) exits normally  
//– Useful for releasing resources & 'cleaning up'  
//– Not called for prototype beans
@PreDestroy  
void  clearCache()  {  }
}
//Causes Spring to invoke @PreDestroy  method
ConfigurableApplicationContext context = SpringApplication.run(...); // Triggers call of all @PreDestroy annotated methods context.close();
```
-   Beans are created in the usual ways:  
    – Returned from @Bean methods  
    – Found and created by the component-scanner
    
-   Spring then invokes these methods automatically – During bean-creation process
- -   Defined by JSR-250, part of Java since Java 6 – Injavax.annotationpackage
>
>-   Alternatively, @Bean has options to define these life-cycle methods
>  @PostConstruct/@PreDestroy for your own classes
>– @Bean properties for classes you didn't write and can't annotate
```java
@Bean (initMethod="populateCache”, destroyMethod="clearCache") 
public AccountRepository accountRepository() {
// ...
}
```
## Stereotypes and meta annotations
- annotations that are themselves annotated with @Component
	- Component scanning also checks for it
	-  Predefined Stereotype annotations
	: @Service @Repository @Controller @Configuration
```java
@Service("transferService") 
public  class  TransferServiceImpl
implements  TransferService  {...}
//Declaration of the @Service annotation
@Target({ElementType.TYPE}) ...  
@Component  
public  @interface  Service  {...}
```

- Meta-annotations
	- Annotation which can be used to annotate other annotations, @ComponentScan recognize them
	- i.e. using @Service to create Custom annotation or @Component annotates stereotype annotations
```java
@Retention(RetentionPolicy.RUNTIME) @Target(ElementType.TYPE) @Service  
@Transactional(timeout=60)

public  @interface MyTransactionalService  {

String  value()  default  ""; }
```
## @Resource  
- JSR-250
- **@Resource** Identifies dependencies by **name** (1st supplied name, 2nd property/field name, 3rd type), **@Autowired** identifies dependencies by **type**
```java
//setter injection
@Resource(name="jdbcAccountRepository”)
//if there is no name "jdbcAccountRepository", Looks for bean called **accountRepository** because method is **setAccountRepository**  
// Then looks for bean of type **AccountRepository**  
public  void  setAccountRepository(AccountRepository  repo)  {
this.accountRepository  =  repo; }
//field injection
@Resource(name="jdbcAccountRepository”) private  AccountRepository  accountRepository;
```
## Standard annotations (JSR 330)
-   Java Specification Request 330
- **@Inject** :   Subset of functionality compared to Spring's **@Autowired** support
```java
//scan JSR330
@ComponentScan  (  "...."  )
>>>>>>>>>>>>>
@Named  
public  class  TransferServiceImpl  implements  TransferService  {

@Inject  
public  TransferServiceImpl(  @Named("accountRepository")

AccountRepository  accountRepository)  {  ...  } }

@Named("accountRepository")  
public  class  JdbcAccountRepository  implements
AccountRepository  {..}
```
| Spring  |JSR 330  |Comments|
|--|--|--|
| @Autowired |@Inject  |@Inject always is required|
|@Required|@Inject||
| @Component | @Named |@ComponentScan could find it|
| @Qualifer | @Named |@Named is by name|
| @Scope |  @Scope|JSR330 Scope only for meta-annoation|
| @Scope("Singleton") | @Singleton ||
|@Value|..|SpEL specific
|@Lazy||
### construction & setter injection 
```java
//constructor injection ex:
@Bean  public  Repository  (id1)repository()  { return  new  (class1)RepositoryImpl();}

@Bean  public  Service  (id2)service()  {  
return  new  (class2)ServiceImpl( (id1) repository() );}

<bean id2=“service” class2=“com.acme.ServiceImpl”/> 
<constructor-arg ref=“repository(id1)”/>
</bean>  
<bean  id1=“repository”  class1=“com.acme.RepositoryImpl”/>

//setter injection ex:
@Bean  public  Repository  (id1)repository()  { return  new  (class1)RepositoryImpl();}

@Bean  public  Service  (id2)service()  { Service  svc  =  new  (class2)ServiceImpl(); svc.setRepository( (id1) repository()  ); return  svc;}

<bean  id2=“service”  class2=“com.acme.ServiceImpl”\> <property(setProperty(id1))  (id1)name=“repository”  (id1)ref=“repository”/>
</bean>  
<bean  id1=“repository”  class1=“com.acme.RepositoryImpl”/>

//Injecting Scalar Values
@Bean  
public  Service  service()  {
Service  svc  =  new  ServiceImpl(  ); 
svc.setStringProperty(  “foo”  ); 
return  svc;
}

<bean  id=“service”  class=“com.acme.ServiceImpl”/> 
<property  name=“stringProperty”  value=“foo”  />
</bean>

//
@Bean  public  Service  service()  { 
Service  svc  =  new  ServiceImpl(  ); 
int  val  =  29;//  Integer  parsing  logic,  29. 
svc.setIntProperty(  val  );  
return  svc;
}

<bean id=“service” class=“com.acme.ServiceImpl”> 
<property  name=“intProperty”  value=“29”  />
</bean>
```
Spring can convert:

Numeric types BigDecimal,  
boolean: “true”, “false” Date
Locale Resource

## Creating an ApplicationContext using XML
```java
//step1
@Configuration 
@ImportResource( {“classpath:com/acme/application-config.xml”,“file:C:/Users/alex/application-config.xml”, "http:" } ) 
@Import(DatabaseConfig.class)  
public class MainConfig { ... }
//<beans> <import resource=“db-config.xml” /></bean>
//step2
//it works well for any Spring application
ApplicationContext context = SpringApplication.run(MainConfig.class);
//old ways
//  Load  Java  Configuration  class  
new  AnnotationConfigApplicationContext(MainConfig.class);

//  Load  from  $CLASSPATH/com/acme/application-config.xml  
new  ClassPathXmlApplicationContext(“com/acme/application-config.xml”);

//  Load  from  absolute  path:  C:/Users/alex/application-config.xml  
new  FileSystemXmlApplicationContext(“C:/Users/alex/application-config.xml”);

//  Load  from  path  relative  to  the  JVM  working  directory  
new  FileSystemXmlApplicationContext(“./application
```
## Controlling Bean Behavior
```java
@PostConstruct
public void setup() { ...}
<bean id=“accountService” class=“com.acme.ServiceImpl” init-method=“setup”> ...</bean>

@PreDestroy
public void teardown() { ...}
<bean id=“Service” class=“com.acme.ServiceImpl” destroy-method=“teardown”> ...</bean>

@Bean  
@Scope(“prototype”)  
public AccountService accountService() {return ... }
<bean id=“accountService” class=“com.acme.ServiceImpl” scope=“prototype”> ...</bean>

@Bean  
@Lazy(“true”)  
public AccountService accountService() {return ... }
<bean id=“accountService” class=“com.acme.ServiceImpl” lazy-init=“true”> ...</bean>

<beans xmlns=http://www.springframework.org/schema/beans ...\> <bean id="rewardNetwork" ... /> <!-- Available to all profiles --> ...  
<beans profile="dev"> ... </beans>  
<beans profile="prod"> ... </beans>
</beans>
```
##   Factory Beans -set conditions for bean definitions in xml config and even Java config
>-   @Bean methods can use any Java you need to Do property lookups and Use if-then-else and iterative logic
    but No equivalent in XML– We did not implement <if>, <for-each>
>-   Instead Spring XML relies on the Factory Patter to Use a factory to create the bean(s) we want and Use any complex Java code we need in the factory's internal logic
```java
//Note: even Java Configuration may use factory beans
public  class  AccountServiceFactoryBean implements  FactoryBean  <AccountService>

{  
public  AccountService  getObject()  throws  Exception  {
//  Conditional  logic  –  for  example:  selecting  the  right  
//  implementation  or  sub-class  of  AccountService  to  create return  accountService;
}  
public  boolean  isSingleton()  {  return  true;  }

public  Class<?>  getObjectType()  {  return  AccountService.class;  } }

<bean  id=“accountService”  class=“com.acme.AccountServiceFactoryBean”  />
```
-   Beans implementing FactoryBeans are auto-detected
    
-   Dependency injection using the factory bean id causes getObject() to be invoked transparently
>getObject() called by Spring internally when ref="accountService" accountService bean is called
```xml
<bean  id=“accountService” class=“com.acme.AccountServiceFactoryBean”/>

<bean  id=“customerService”  class=“com.acme.CustomerServiceImpl”\> <property  name=“service”  ref=“accountService”  />

</bean>
```
>the same as xml, in Java config , getObject() is called when //accountService bean is injected
```java
@Bean  
public  CustomerService  customerService(AccountService  accountService)  {
return  new  CustomerService(accountService); }
```
- -   FactoryBeans are widely used within Spring – EmbeddedDatabaseFactoryBean**  
    – JndiObjectFactoryBean
    
    • One option for looking up JNDI objects  
    – Creating Remoting proxies  
    – Creating Caching proxies**  
    – For configuring data access technologies**
    
    • JPA, Hibernate or MyBatis
    
-   In XML, often hidden behind namespaces
###   Namespaces
- The default namespace in a Spring configuration file is typically the “beans” namespace
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="

http://www.springframework.org/schema/beans

http://www.springframework.org/schema/beans/spring-beans.xsd\> <!\-\- ... -->

</beans>
```
-   Defined for subsets of framework functionality\* – aop (Aspect Oriented Programming)  
    – tx (transactions)  
    – util
    – jms  
    – context –...
    
-   They allow hiding of actual bean definitions  
    – Define “programming instructions” for bean files – Greatly reduce size of bean files (see next slides)
```xml
<?xml version="1.0" encoding="UTF-8"?> 
<beans ...spring-beans.xsd>//dont use Schema Version Numbers
//hides 1 bean definition 
<context:property-placeholder location="db-config.properties" />
<aop:aspectj-autoproxy /> //5
<tx:annotation-driven /> //15
<context:component-scan base-package=“com.acme.app.repository, com.acme.app.service, com.acme.app.controller” />
</beans>
```
-   Many shortcuts and useful techniques exist
    
    • Singleton and Factory Beans • Bean Definition Inheritance  
    • Inner Beans  
    • p and c namespaces
    
    • Using collections as Spring beans
## Summary
-   Spring beans can be defined:  
-     – Explicitly using @Bean methods  
    – Implicitly using @Component and component-scanning
    
-   Most applications use both – Implicit for your classes  
    – Explicit for the rest
    
-   Can perform initialization and clean-up – Use @PostConstruct and @PreDestroy
    
-   Use Spring's stereotypes and/or define your own meta annotations
## Bean Lifecycle
Introduction

###   The initialization phase
-   When a context is created the initialization phase completes
```java
// Create the application from the configuration
ApplicationContext context = SpringApplication.run(AppConfig.class);
```
![enter image description here](https://user-images.githubusercontent.com/32177380/37104068-c2e13030-21f9-11e8-9e85-dad4ee9fa805.png)

- Load bean definitions 
	- **bean definitions** loaded in  ApplicationContext/ BeanFactory   `@Configuration class/ @Components are scanned/xml parsed`
	-   **BeanFactoryPostProcessor** process/modify bean definitions. 
```java
public interface BeanFactoryPostProcessor { 
	public void postProcessBeanFactory(beanFactory);
}

//PropertySourcesPlaceholderConfigurer as a
//BeanFactoryPostProcessor to evaluate @Value("${max.retries}")
@Configuration  
@PropertySource(“classpath:/config/app.properties”) 
public class ApplicationConfig {
@Value("${max.retries}")
int maxRetries; ...
}

@Bean
public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
return new PropertySourcesPlaceholderConfigurer(); }

<context:property-placeholder/>
```
-  Initialize each bean instance
- constructor instantiates beans in right order with its dependencies injected (except Lazy) 
- setter 
- BeanPostProcessors BPP-Initializer-BPP
```xml
<bean id=“accountRepository” class=“com.acme.JdbcAccountRepo” init-method=“populateCache”>... </bean>

<context:annotation-config/>
Or<context:component-scan/>
```
###   The use phase
example
```java
ApplicationContext context = // get it from somewhere // Lookup the entry point into the application TransferService service =
(TransferService)context.getBean(“transferService”); // Use it!

service.transfer(new MonetaryAmount(“50.00”), “1”, “2”);

//Proxy classes are created in the init phase by dedicated BeanPostProcessors
{SpringProxy(Spring TransactionInterceptor) [target(TransferServiceImpl)]}
A BeanPostProcessor may wrap your beans in a dynamic proxy– adds behavior to your bean transparently
```
| JDK Proxy(dynamic proxies) | CGLib Proxy |
|--|--|
| API is built into the JDK | not built into JDK |
|require Java interfaces |used when interfaces not avaiable|
|all interfaces proxied|final classes or methods are not proxied|

###   The destruction phase
- Application services are Shutdown 
- system resources Are released and eligible for garbage collection
```java
ConfigurableApplicationContext context = ...
// Destroy the application
context.close();

//by java config
@Bean (destroyMethod=”clearCache”) 
public AccountRepository accountRepository() {
void clearCache(){}
}

//by annotation
<context:annotation-config/>
<context:component-scan ... />
public class AccountRepository {
	@PreDestroy
	void clearCache() {  
	// close files, connections ...  
	// remove external resources ...

} }

//by XML
<bean id=“accountRepository” class=“app.impl.AccountRepository” destroy-method=“clearCache”>
... 
</bean>
```
### Spring application Lifecycle  
– The initialization phase

• Bean Post Processors for initialization and proxies – The use phase

• Proxies at Work – most of Spring's “magic” uses a proxy – The destruction phase

• Allow application to terminate cleanly
