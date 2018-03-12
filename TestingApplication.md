# Testing
## Test Driven Development
 -   – Writing automated tests that verify code actually **works**
-   – Driving development with well defined **requirements** in the form of tests
## Unit Testing
– Tests one unit of functionality  
– Keeps dependencies minimal  
– Isolated from the environment (including Spring) – Uses simplified alternatives for dependencies
	- Stubs and/or Mocks   
## Integration Testing with Spring
-   Integration (System) Testing  
    – Tests the interaction of multiple units working together
    
    - All should work individually (unit tests showed this)
    
-   Tests application classes in context of their surrounding infrastructure
    
    – Out-of-container testing, no need to run up full JEE system – Infrastructure may be “scaled down”
    
    ??• Use Apache DBCP connection pool instead of container- provider pool obtained through JNDI
    
    • Use ActiveMQ to save expensive commercial JMS licenses
### Spring’s Integration Test Support
-   Packaged as a separate module – spring-test.jar
    -   Consists of several JUnit test support classes
    
-   Central support class is **SpringJUnit4ClassRunner**
- Caches **a shared ApplicationContext** across test methods
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=SystemTestConfig.class)
//?@SpringBootTest in SpringBoot
public  final class  TransferServiceTests  { 
@Autowired  
private TransferService transferService;

	@Test  
	public  void  shouldTransferMoneySuccessfully()  {

		TransferConfirmation  conf  =  transferService.transfer(...);

	... }

}
```
>xml
```java
@RunWith(SpringJUnit4ClassRunner.class) 
@ContextConfiguration(“classpath:com/acme/system-test-config.xml”) 
//@ContextConfiiguration - Defaults to ${classname}-context.xml in same package
//-{“classpath:config-1.xml”,  “file:db-config.xml”}
public  finall class  TransferServiceTests  {
```
### Test Property Sources
>Defaults to looking for **[classname].properties**
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=SystemTestConfig.class)
@TestPropertySource(properties  =  {  "username=foo",  "password=bar"  } locations  =  "cllasspath:/transfer-test.propertiies")
public  ?final? class  TransferServiceTests  { ...}
```
## Testing with Profiles
-   @ActiveProfiles inside the test class  
    – Define one or more profiles  
    – Beans associated with that profile are instantiated – Also beans not associated with any profile
```java
@RunWith(SpringJUnit4ClassRunner.class) @ContextConfiguration(classes=DevConfig.class) 
@ActiveProfiles( { "jdbc", "dev" } )
public class TransferServiceTests { ... }
```
-   @ActiveProfiles inside the test class
```java
@RunWith(SpringJUnit4ClassRunner.class) @ContextConfiguration(classes=DevConfig.class) @ActiveProfiles("jdbc")  
public class TransferServiceTests{...}

//@Profile annotates at the @Configuration class
//only @Configurations matching an active profile or with no profile are loaded
@Configuration
@Profile("jdbc")  
public class DevConfig {
@Bean {...}
}

//@Profile at the Component class
//Only beans with current profile / no profile are component-scanned
@Repository @Profile("jdbc")  
public class JdbcAccountRepository { ...}

//profile attribute inside <bean> tag
//Only beans with current profile / no profile are loaded
<beans xmlns=...>
<!-- Available to all profiles -->
<bean id="rewardNetwork" ... /> ...

<beans profile="jdbc"> ... </beans>

<beans profile="jpa"> ... </beans> </beans>
```
## Testing with Databases
-   Integration testing against SQL database is common.
    
-   In-memory databases useful for this kind of testing
    
    – No prior install needed
    
-   Common requirement: populate DB before test runs
    
    – Use the @Sql annotation:
 ```java
@RunWith(SpringJUnit4ClassRunner.class) @ContextConfiguration(...)  
?@Sql({“/testfiles/schema.sql”, “/testfiles/general-data.sql”  }  )
public  final class  MainTests  {

@Test  
@Sql
?//Run script named (by default) MainTests.success.sql in same package
public  void  success()  {  ...  }

@Test  
@Sql ( “/testfiles/error.sql”) //?Run before @Test method 
@Sql (scripts=“/testfiles/cleanup.sql”,
executionPhase=Sql.ExecutionPhase.AFTER_TEST_METHOD  )//...run after @Test method
public void transferError()  {  ...  }

}
 ``` 
>– executionPhase: before (default) or after the test method – config: SqlConfig has many options to control SQL scripts

>• What to do if script fails? **@SqlConfig**(errorMode= FAIL_ON_ERROR/ CONTINUE_ON_ERROR/ IGNORE_FAILED_DROPS/ DEFAULT)*

>• SQL syntax control: comments, statement separator    
```java
@Sql( scripts = "/test-user-data.sql",  
executionPhase = ExecutionPhase.AFTER_TEST_METHOD, config = @SqlConfig(errorMode = ErrorMode.FAIL_ON_ERROR,
commentPrefix = "//", separator = "@@"?) )
```
## Mokito
```java
import  static  org.mockito.Mockito.*;

public class AuthenticatorImplTests  {  
//  Create  a  mock  object private
private  AccountRepository  accountRepository=  mock(  AccountRepository.class  );   
 
//  Inject  the  mock  object
AuthenticatorImpl  authenticator=  new  AuthenticatorImpl(accountRepository);  

@Test 
public  void  validUserWithCorrectPassword()  { 
//  Train  the  mock
when(accountRepository.getAccount(“lisa”)).  
thenReturn(new  Account(“lisa”,  “secret”));

//  Run  test verify(accountRepository);
//  Verify  getAccount()  was invoked  on  the  mock
assertTrue(  authenticator.authenticate(“lisa”,  “secret”)  );  
  

}

}
```
## Summary
-   Testing is an essential part of any development
    
-   Unit testing tests a class in isolation
    
    – External dependencies should be minimized – Consider creating stubs or mocks to unit test – You don’t need Spring to unit test
    
-   Integration testing tests the interaction of multiple units working together
    
    – Spring provides good integration testing support  
    – Profiles for different test & deployment configurations – Built-in support for testing with Databases
