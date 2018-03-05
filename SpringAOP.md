# Spring AOP
## What Problem Does AOP Solve?
> modularization of cross-cutting concerns
> to avoid code tangling (coupling of concerns) and scattering(same concern spread across modules)
### what are cross-cutting concerns?
- generic functionality that is needed in many places in your application
	- logging, tracing, transaction management, security, caching, error handling, performance monitoring, and custom business rules
i.e. Perform a role-based security check before every application method
### How AOP works?
- implement your mainline application logic focusing on the core problem
- write aspects to implement your cross-cutting concerns
- weave the aspects into your application (adding cross-cutting behaviours to the right places)
### Leading AOP Technologies
- AspectJ
	- byte code modification for aspect weaving
- Spring AOP
	- dynamic proxies for aspect weaving 
    
## Core AOP Concepts
 ### Join Point
 a point in the execution of a program such as a method call or exception thrown, **a point that the advice executes**
 ### Pointcut
 an **expression** that selects **one or more Join Points**
 ### Advice
 **code** to be executed **at** each selected **Join Point**
 ### Aspect
 a module that encapsulates **pointcuts and advice**
 ### Weaving
 technique by which **Aspects** are combined with **main code**
## Quick Start
> application object whose properties could change
```java
public interface Cache {  
public void setCacheSize(int size);

}
```
>Aspect implement
```java
@Aspect  
@Component  
public class PropertyChangeTracker {

private Logger logger = Logger.getLogger(getClass());

//Pointcut: an expression that selects one or more Join Points(a point in the execution of a program such as a method call or exception thrown, **a point that the advice executes**)
@Before(“execution(void set*(*))”) 
public void trackChange() {
//advice: code to be executed at each selected Join Point
	logger.info(“Property about to change...”); }

}
```
>Configure Aspect as a bean
```java
@Configuration
@EnableAspectJAutoProxy
@ComponentScan(basePackages="com.example")
public class AspectConfig{
...
}

>Include the Aspect configuration

@Configuration 
@Import(AspectConfig.class) 
public class MainConfig {
@Bean
public Cache cacheA() { return new SimpleCache(“cacheA”); } 
@Bean
public Cache cacheB() { return new SimpleCache(“cacheB”); }
@Bean
public Cache cacheC() { return new SimpleCache(“cacheC”); } 
}
```
>xml config
```xml
<beans>
<aop:aspectj-autoproxy/>
<context:component-scan base-package="com.example/>
</beans>
```
>test the app
```java
ApplicationContext context = SpringApplication.run(MainConfig.class);
@Autowired @Qualifier(”cacheA”); private Cache cache;  
...  
cache.setCacheSize(2500);
```
### How Aspects are applied
- Spring creates a proxy weaving aspect and target
- proxy implements target interfaces
- all calls routed through proxy interceptor
- matching advice is executed
- target method is executed
### JoinPoint parameter
```java
@Aspect

public class PropertyChangeTracker {  
	private Logger logger = Logger.getLogger(getClass());

	@Before(“execution(void set*(*))”)  
	public void trackChange(JoinPoint point) {

	String name = point.getSignature().getName(); 
	Object newValue = point.getArgs()[0]; 
	logger.info(name + “ about to change to ” +
		newValue + “ on ” + point.getTarget());
} }
```

## Defining Pointcuts
>Spring AOP uses AspectJ's pointcut expression language for selecting where to apply advice
### common pointcut designator
`execution(<method pattern>)`
`&&, ||, !`
method pattern : 
`[Modifiers] ReturnType [ClassType] MethodName ([Arguments]) [throws ExceptionType]`
=>
execute(public **int** com.example.MyService.**findById**(..) throw RuntimeException)
### execution expression examples
 - execute(* send*(*, ..))
	- any method starting with *send* that the first parameter is any type and any return type
	- .. signifies 0 or more parameters, * signifies 1 parameter or anything non-parameter
 -   execution(void example.MessageServiceImpl.*(..))
    - Any void method in the MessageServiceImpl class • Including any sub-class
 - execution(void example.MessageService.send(*))
    - Any void send method taking one argument, in any object implementing MessageService
 - execution(@javax.annotation.security.RolesAllowed void send*(..))
	-   Any void method whose name starts with “send” that is annotated with the @RolesAllowed annotation, i.e. `@RolesAllowed(“USER”)`
	-   Ideal for your own classes
 - execution(* rewards.*.restaurant.*.*(..))
	- There is one directory between rewards and restaurant

 - execution(* rewards..restaurant.*.*(..))
	- There may be *several directories* between rewards and restaurant

 - execution(* *..restaurant.*.*(..))
	- Any *sub-package* called restaurant
## Implementing Advice
### Advice types
 - Before
`@Before(“execution(void set*(*))”)`
proxy -> BeforeAdvice -> Target
	-	if the advice throw an exception, then the target wont be called
 - AfterReturning
`@AfterReturning(value=“execution(* service..*.*(..))”, returning=“reward”)`
proxy ->  Target -[successful return]->AfterReturningAdvice
 - AfterThrowing
```java
@AfterThrowing(value=“execution(* *..Repository.*(..))”, throwing=“e”) public void report(JoinPoint jp, DataAccessException e) {
	mailService.emailFailure(“Exception in repository”, jp, e); }
```
proxy ->  Target -[exception thrown]->AfterThrowingAdvice
>it wont stop exception propagating, however it can throw a different type of exception.
- After
proxy ->  Target -[exception or return]->AfterAdvice
	- the advice will execute after target method executed no matter exception thrown or successfully returned
```java
@Aspect
public class PropertyChangeTracker {  
private Logger logger = Logger.getLogger(getClass());

@After(“execution(void update*(..))”) 
public void trackUpdate() {

	logger.info(“An update has been attempted ...”); }

}
```
- Around
proxy -> AroundAdvice -proceed()-> Target -> AroundAdvice
Cache values returned by cacheable services
```java
@Around(“execution(@example.Cacheable * rewards.service..*.*(..))”) 
public Object cache(ProceedingJoinPoint point) throws Throwable {

Object value = cacheStore.get(CacheUtils.toKey(point));

if (value == null) {  
value = point.proceed(); 
cacheStore.put(CacheUtils.toKey(point), value);
}

return value; 
}
```
## Limitations of Spring AOP
- only advice non-private methods
- only apply aspects to Spring beans
- weaving with proxies is limited 
	- when method a() calls method b() on the same class/interface, advice will only be executed for method a()

## Summary

-   Aspect Oriented Programming (AOP) modularizes cross-cutting concerns
    
-   An aspect is a module (Java class) containing the cross- cutting behavior
    
    – Annotated with @Aspect  
    – Behavior is implemented as an “advice” method  
    – Pointcuts select joinpoints (methods) where advice applies – Five advice types
    
    • Before, AfterThrowing, AfterReturning, After and Around
   
## XML Configuration  
 ```xml
 <aop:config>  
<aop:aspect ref=“propertyChangeTracker”>

<aop:before pointcut=“execution(void set*(*))” method=“trackChange”/\> </aop:aspect>

</aop:config>  
<bean id=“propertyChangeTracker” class=“example.PropertyChangeTracker” />
 ```
 ## Named Pointcuts  
 >A pointcut expression can have a name, i.e. `setterMethods`
 ```xml
 <aop:config>

<aop:pointcut id=“setterMethods” expression=“execution(void set*(*))”/>

<aop:aspect ref=“propertyChangeTracker”>  
<aop:after-returning pointcut-ref=“setterMethods” method=“trackChange”/> 
<aop:after-throwing pointcut-ref=“setterMethods” method=“logFailure”/>

</aop:aspect> </aop:config>

<bean id=“propertyChangeTracker” class=“example.PropertyChangeTracker” />
 ```
 >The method name `serviceMethod()` `repositoryMethod()` become the pointcut ID. The method is not executed.
 ```java
 @Aspect

public class PropertyChangeTracker {  
private Logger logger = Logger.getLogger(getClass());

@Before(“serviceMethod() || repositoryMethod()”) public void monitor() {

logger.info(“A business method has been accessed...”); }

@Pointcut(“execution(* rewards.service..\*Service.\*(..))”) 
public void serviceMethod() {}

@Pointcut(“execution(* rewards.repository..\*Repository.\*(..))”)
public void repositoryMethod() {} 

//Fully-qualified pointcut name
@Before( “com.acme.Pointcuts.serviceMethods()” ) 
public void monitor() {

logger.info(“A service method has been accessed...”); }
}
 ```
## Context-Selecting Pointcuts
> ‘target’ binds the server starting up
> ‘args’ binds the argument value
```java
@Before(“serverStartMethod(**server, input**)”)  
public void logServerStartup(**Server server, Map input**) {

... }

@Pointcut(“execution(void example.Server.start(java.util.Map)) && target(server) && args(input)”)

public void serverStartMethod (Server server, Map input) {}
```
 
## Pointcut Expression using Annotations
```java
execution(@org..transaction.annotation.Transactional * *(..)) 
Any method marked with the @Transactional annotation

execution( (@example.Sensitive *) *(..))
Any method that returns a type marked as @Sensitive

@Sensitive  
public  class  MedicalRecord  {  ...  }

public  class  MedicalService  {  
public  MedicalRecord  lookup(...)  {  ...  }

}
```
