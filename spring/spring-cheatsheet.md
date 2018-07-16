# Boot

>Points to remember:

- opinionated runtime: 
- Dependency Management
- auto-configuration but customizable
- packaging

## How to setup a SpringBoot project?
```java
1. pom.xml 						- setup dependencies
2. HelloController.java 		- Spring MVC controller
3. application.properties 
spring.mvc.view.prefix=/WEB-INF/views
spring.mvc.view.suffix=.jsp
					 			- InternalResourceViewResolver config
4. /WEB-INF/views/hello.jsp     - Spring MVC view
5. Application.java 
@SpringBootApplication(Configuration, ComponentScan, EnableAutoConfiguration)
								- applcation launcher
```

## How to run a SpringBoot project?
```java
mvn package
helloApp-0.0.1-SNAPSHOT.jar
java -jar helloApp-0.0.1-SNAPSHOT.jar/war
---runing in localhost:8080
```
## Opinionated runtime: 
based on the classpath jars, Spring Boot sets up default configuration envrionment
```java
JPA-EntityMangerFactory etc, "spring-boot-starter-web"-embeddedTomcat(web app run in jar file), SpringMVC, Jackson etc.
```
## Dependency Management: enable auto configuration
- parent (spring-boot-starter-parent) : " spring-boot-starter" the version
- dependencies :  scanned, auto-configed
"spring-boot-starter-web"-embeddedTomcat, SpringMVC, Jackson etc.- DispatcherServlet and ContextLoaderListener
"spring-boot-starter-test" - spring-test, junit, mokito 
"spring-boot-starter-jpa/jdbc/batch" - JdbcTemplate
"hsqldb" - EmbeddedDataSource 
- plugins
```java
for packging
"spring-boot-maven-plugin"
```
## container vs embedded container
```java
public class Application extends SpringBootServletInitializer{}

java -jar app.war.original - war embeded-container and outside-container versions match
java -jar app.war - hybrid war
```
## `application.properties/yml`
define any properties here, i.e., 
```properties
database.host=localhost 
database.user=admin
#support any loging famework, SLF4J better
logging.level.org.springframework=DEGUG
logging.levle.com.acme.your.code=INFO
#override the default log-to-console
logging.file=rewards.log
#a pooled datasource is created by default, explicitly config datasource override the default one
spring.datasource.url=jdbc:mysql://localhost/test
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
#set up web config
server.port=900
server.address=192.168.1.20
server.session-timeout=1800
server.context-path=/rewards
server.servlet-path=/admin
```

## static resources, templates, error
/static, /public, /resources, /META-INF/resources; 

/templates; 

/error


# Test
first each unit tested, then integration test (multi units work together)
## Test in Boot
```java
@RunWith(SpringJUnit4ClassRunner.class)
//"TranserApplication class" is the SpringBootApplication class 
@SpringApplicationConfiguration(class=TranserApplication.class)
//web app test, default location of resource is src/main/webapp
@WebAppConfiguration
public final class TranserServiceTest{
	@Autowired
	private TranserService transerService;
	@Test
	public void successfulTransfer(){
		TransferConfirmation conf=transerService.transfer;
	}
}
```
## TDD
test driven development: requirements --in a form of --> tests

## How do you write unit test?
- use **stubs/mocks** as alternatives of non-tested dependencies.
- test a unit bussiness logic, success or fail

### Use a stub
```java
public class AuthenticatorImpl{
	//accRepo needs a stub
	private AccountRepository accRepo;
	public AuthenticatorImpl(AccountRepository accRepo){
		this.accRepo = accRepo;
	}
	public boolean authenticate(String username, String password){
		Account acc = accRepo.getAccount(username);
		//test a unit bussiness logic, success or fail
		return acc.getPassword().equals(password);
	}
}

public class StubAccountRepository implements AccountRepository{
	public Account getAccount(String username){
		return "lisa".equals(user)? new Account("lisa","secret"): null;
	}
}

public final class AuthenticationImplTests{
	private AuthenticationImpl authenticator;

	@Before public void setUp(){
		authenticator = new AuthenticatorImpl(new StubAccountRepository());
	}

	@Test public void successfulAuthentication(){
		assertTrue(authenticator.authenticate("lisa","secret"));
	}

	@Test public void invalidPassword(){
		assertFalse(authenticator.authenticate("lisa","invalid"));
	}
}
```
### Use a mock
```java
//generate a mock object using mock lib; 
mock(AccountRepository.class)

//record the mock with expectation that when methodA is used, then resultA is returned;
when(accRepo.getAccount("lisa"),thenReturn(new Account("lisa", "secret")

//run the test, assertTrue/assertFalse/assertEquals
assertTrue(authenticator.authencate("lisa","secret"))

//verify the mock
verify(accRepo)
```

## How do you write integration test?
```java
1. config spring-test dependency
2. no @Before method
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=SystemTestConfig.class)
@TestPropertySource(properties={"username=foo","password=bar"} location="classpath:/transfer-test.properties")
@ActiveProfiles({"jdbc","dev"})//@Profile in normal config class
public final class TranserServiceTest{
	@Autowired
	private TranserService transerService;
	@Test
	public void successfulTransfer(){
		TransferConfirmation conf=transerService.transfer;
	}
}
```
## How do you test with databases
in-memory databases
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=SystemTestConfig.class)
@Sql({"/testfiles/schema.sql","/testfiles/general-data.sql"})
public final class TranserServiceTest{
	@Test@Sql("/testfiles/test-data.sql")
	//sql runs before test executed
	public void successfulTransfer(){}

	@Test@Sql(scripts="/testfiles/test-data.sql", executionPhase=Sql.executionPhase.AFTER_TEST_METHOD, config=@SqlConfig(errorMode=ErrorMode.FAIL_ON_ERROR))
	//sql runs AFTER test executed
	public void successfulTransfer(){}
}
```

# Web
Spring MVC is Spring 's web framework. @Controller class handle HTTP requests. URL info available via @RequestParam, @PathVariable . Data returned via Model. output html generated by Views. ViewResolvers define where views can be found.


## Request processing lifecycle
request URL is handled by DispatcherServlet which dispatch the request to a controller, the controller returns a logic view name back to DispatcherServlet, dispatcherServlet consults viewResolver for the right view. Finally, dispatcherServlet render model data on the view.
## artifacts (dispatcherservlet, controllers, views)
- DispatcherServlet

is definded by WebApplicationInitializer aka "web.xml" in xml config
```java
public class MyWebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer{
	//get root config class which is loaded by context loader listener 
	//get servlet config class in which viewResolver is configed
	//dispatcherservlet mapping
}
```
- Controllers

(HttpServletRequest, HttpSession, Principal, Model, @RequestParam("id") long id, @PathVariable("id") long , @RequestHeader("user-agent") String agent)

"http://localhost:8080/mvc-1/rewardsadmin/" application server, webapp, servlet mapping, request mapping
```java
@Controller
@RequestMapping("/listAccounts")//ignore .html
public class AccountController{
	//"/listAccounts.html?id=123"
	public String list(Model model, @RequestParam("id") long id){}
	//"/listAccounts/{id}"
	public String list(Model model, @PathVariable("id") long id){}
}
```
- Views

InternalResourceViewResolver is created for ViewResolver bean registered in `MvcConfig.class`

by default view files are in "/WEB-INF/views/"

## how do you implement a web mvc module
1. deploy a dispatcher servlet, extends AbstractAnnotationConfigDispatcherServletInitializer, in which RootConfig, MvcConfig and DispatcherServlet mapping is initialized.
2. implement a controller
3. register the controller with the dispatcher servlet
4. implement the views
5. register a view resolver
6. deploy and test

```java
//dispather servlet
public class MyWebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer{
	//get root config class which is loaded by context loader listener 
	getRootConfigClasses(){
		return new Class<>[]{RootConfig.class};
	}
	//get servlet config class in which viewResolver is configed
	getServletConfigClasses(){
		return new Class<>[]{MvcConfig.class};
	}
	//dispatcherservlet mapping
	getServletMapping(){
		return new String[]{"/rewardsadmin/*"};
	}
}

@configuration
@ComponentScan("package.mvc.bean.name")
public class MvcConfig{//additional beans //override view resolver
}

<html>
<body>
Amount = ${reward.amount}<br/>
Date = ${reward.date}<br/>
Account = ${reward.account}<br/>
Description = ${reward.description}<br/>
</body>
</html>

@Configuration
@EnableWebMvc
public class RootConfig{
}
```
# REST Web Services
## what is REST
- an architectureal style 
- describing best practices to expose web services over HTTP
- use HTTP as application protocol for application interaction
- emphasize scalability

## What is HTTP Header
- *HTTP method: GET, PUT, POST, DELETE
- *URI: "/orders/123"
- Host: shop.spring.io
- Accept(presentation): text/plain 
- body: "hello world"

## What is HTTP Response (Header and body)
- protocol: HTTP/1.1
- *http status: 200 OK
- content-type: text/plain
- body: "hello world"
```java
ResponseEntity.ok().contentType(MediaType.TEXT_PLAIN).body("hello world")
```

## What is idempotent
A repetively operation without different outcomes.
POST, PATCH are not

## safe operations
do not modify resources
PUT, POST, DELETE, PATCH are not.

## secure REST
we can implement security in RESTful web service by using transfer layer security TLS, auth token security. TLS could be setup in Tomcat. TLS encrypts communication, HTTPS to authenticate servers.

## REST principles
1. resources are exposed through URIs and nouns
2. http methods for operations `GET, PUT, POST, DELETE to /orders/123`
3. respond representations could be HTML, XML, JSON
4. a representation could have links, HATEOS
5. stateless: no httpsession, GETs cached in URI, client tracks state, client and server loose coupling 
6. respons = headers and status code 

## benefit of REST
HTTP universal, various clients, scalable, representation various, redirect, caching, resourse identification

## Spring REST support:
from Spring MVC, 
support both HTML on brower and REST API, 
RestTemplate for consumer,

## The process dealing with a Http request:
### - request mapping with path and method 
```java
@RequestMapping(path="/order", method=RequestMethod.GET)
@GetMapping("/accounts")
//Post, Put, Delete, Patch
```

### - response status code,
```java
@ResponseStatus(HttpStatus.NO_CONTENT)
200 OK, 201 created successfully, 204 no content
30x redirect, 
404 Not Found, 405 method not supported, 406 cannot generate response body requested, 415 request body not supported
500 server error(unhandled exception etc), 
```
### - converts HTTP request/response body data
```java
//@PutMapping(path="/orders/{id}")
//@ResponseStatus(HttpStatus.NO_CONTENT)
public void updateOrder(@RequestBody Order updateOrder, //@PathVariable("id") long id)
{
	orderService.updateOrder(id, updateOrder);
}

//@GetMapping(path="/orders/{id}")
//@ResponseStatus(HttpStatus.OK)
public @ResponseBody Order getOrder(//@PathVariable("id") long id)
{
	return orderService.findOrderById(id);
}

@RestController//@Contoller + @responseBody
public class OrderController{
	//@GetMapping(path="/orders/{id}")
//@ResponseStatus(HttpStatus.OK)
	public Order getOrder(@PathVariable("id") long id){
		return orderService.findOrderById(id);
	}
}
```

### access request and response body data,
```java
@RequestBody(http request body --convert to--> entity)
@ResponseBody(returned object/entity --convert to-> http response body)
//http status, content type, body -- http headers builder pattern
ResponseEntity.ok().contentType(MediaType.TEXT_PLAIN).body("hello world");(@ResponseBody but explicitly build)
//http headers (content-type)
HttpHeaders httpHeaders=new HttpHeaders().set("Content-Type","text/xml");
return new HttpEntity<Order>(order, httpHeaders);
```
### build valid location URIs - `UriComponentsBuilder`
```java
String templateUrl="http://store.spring.io.orders/{orderId}/items/{itemId}";
URI location = UriComponentsBuilder.fromHttpUrl(templateUrl).buildAndExpand("123","itemA").toUri();
```

## Demos for GET/POST/PUT/DELETE
```java
@GetMapping(path="/orders/{id}")
//@ResponseStatus(HttpStatus.OK)
public @ResponseBody Order getOrder(@PathVariable("id") long id){
	return orderService.findOrderById(id);
}

@PostMapping(path="/orders/{id}/items")
@ResponseStatus(HttpStatus.CREATED)//201
public ResponseEntity<Void> createItem(@PathVariable("id") long id, @RequestBody Item newItem){
	//orderService add newItem from the requestBody into repo
	orderService.findOrderById(id).addItem(newItem);

	URI location = ServletUriComponentsBuilder
	.fromCurrentRequestUri().path("items/{itemId}").buildAndExpand(newItem.getId()).toUri();

	return ResponseEntity.created(location).build();
}

@PutMapping(path="/orders/{orderId}/items/{itemId}")
@ResponseStatus(HttpStatus.NO_CONTENT)//204
public void updateItem(@PathVariable("orderId") long orderId, @PathVariable("itemId") String itemId, @RequestBody Item item){
	orderService.findOrderById(orderId.updateItem(itemId, item));
}

@DeleteMapping(path="/orders/{orderId}/items/{itemId}")
@ResponseStatus(HttpStatus.NO_CONTENT)//204
public void deleteItem(@PathVariable("orderId") long orderId, @PathVariable("itemId") String itemId){
	orderService.findOrderById(orderId.deleteItem(itemId));
}
```
## Consuming a RESTful Web Service
### create a domain class to contain data from producer
### consume resource URL using RestTemplate in SpringBootApplication
```java
//Jackson JSON processing lib, any type not this type is ignored
@JsonIgnoreProperties(ignoreUnknown=true)
public class Quote{
	private String type;
	private Value value;
	//getters, setters, toString
}
@JsonIgnoreProperties(ignoreUnknown=true)
public class Value{
	//...
}

@SpringBootApplication
public class Application{
	//main() SpringApplication.run(Application.class);
	@Bean
	public RestTemplate restTemplate(RestTemplateBuilder builder){
		return builder.build();
	}

	@Bean
	public QuoterestConsumer(RestTemplate restTemplate) throws Exception(){
		
			Quote quote = restTemplate.getForObject(url, Quote.class);
			return quote;
		
	}
}
```
### RestTemplate methods for HTTP methods
```java
//get
OrderItem[] items = template.getForObject(uri, OrderItem[].class, "1");
//post
OrderItem item = template.postForLocation(uri, item, "1");
//put
//modify item
item.setAmount(2);
//put/update it to itemLocation
template.put(itemLocation, item);
//delete
template.delete(itemLocation);
```

## Exceptions in RESTful WS
Normally any unhandled exception thrown when processing a web-request causes the server to return an HTTP 500 response. 
### annotate a customized exception with @ResponseStatus
```java
@ResponseStatus(value=HttpStatus.NOT_FOUND, reason="No such Order")  // 404
 public class OrderNotFoundException extends RuntimeException {
     // ...
 }
 //use the exception in a controller method
 @RequestMapping(value="/orders/{id}", method=GET)
 public String showOrder(@PathVariable("id") long id, Model model) {
     Order order = orderRepository.findOrderById(id);

     if (order == null) throw new OrderNotFoundException(id);

     model.addAttribute(order);
     return "orderDetail";
 }
```
### for existing exceptions, annotate @ExceptionHandler with @ResponseStatus on a method on Controller

```java
//@Controller
@ControllerAdvice
class GlobalControllerExceptionHandler {
    @ResponseStatus(HttpStatus.CONFLICT)  // 409
    @ExceptionHandler(DataIntegrityViolationException.class)
    public void handleConflict() {
        // Nothing to do
    }
}
```

## mixing put/post form controller and RESTful controller
```java
@GetMapping(path="/orders/{id}", produces="application/json")
@ResponseStatus(HttpStatus.OK)
public @ResponseBody Order getOrder(@PathVariable("id") long id){
	return orderService.findOrderById(id);
}

@GetMapping(path="/orders/{id}")
public String getOrder(Model model, @PathVariable("id") long id){
	//call the RESTful method
	model.addAttribute(getOrder(id));
	return "orderDetails";
}
```
## HATEOAS
hypermedia as the engine of application state
- no prior knowledge about how to interact with a particular server
- links change as state/attributes change
```java
@Controller
@EnableHypermediaSupport(type=HypermediaType.HAL)
public class OrderController{
	//get mapping handler method
	@GetMapping(path="/orders/{id}")
	public @ResponseBody Resource<Order> getOrder(@PathVariable("id") long id){
	Link link = ControllerLinkBuilder.linkTo(AccountController.class)
	.slash(accountId)
	.slash("transfer")
	.withRel("transfer");
	return new Resource<Order>(orderService.findOrderById(id), link);
}
	
}
```
# Microservices
## How to develop microservices
- setup services using Spring Boot ("spring-cloud-starter", "spring-cloud-starter-eureka", "spring-cloud-starter-web" etc.)
- producer service using RestController 
- consumer service using RestTemplate with client-side load balancing Ribbon determines the best available service to use.
- with service discovery Eureka return the URL of all available instances.
 


## To be more precise:
- Run a "Discory Service" using Eureka
```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication(){
	//SpringApplication.run()
}

application.yml
server:
	port:8761
eureka:
	instance:
		hostname:localhost
	client:
		registerWithEureka:false
		fetchRegistry:false

pom.xml
"spring-cloud-starter-eureka-server"
```
- Run a microservice as "producer" and register its logic service name with the "Discory Service" 
```java
@SpringBootApplication
@EnableDiscoveryClient//for both consumer and producer application
public class AccountsApplication(){
	//SpringApplication.run()
}

application.yml
spring:
	application://logic service name
		name:accounts-microservice
eureka:
	client:
		serviceUrl://eureka server URLs
			defaultZone:http://localhost:8761/eureka/
```
- Run a consumer to consume service from producer using RestTemplate
```java
@SpringBootApplication
@EnableDiscoveryClient//for both consumer and producer application
public class FrontEndApplication(){
	//SpringApplication.run()
	@Bean @LoadBalanced //Ribbon
	public RestTemplate restTemplate(RestTemplateBuilder builder){
		return builder.build();
	}

}

@Service
public class RemoteAccountManager implements AccountService{
	@Autowired @LoadBalanced //
	RestTemplate restTemplate;

	public Account findAccount(String id){
		return restTemplate.getForObject("http://accounts-microservice/accounts/{id}", Account.class, id);
	}
}
```
#JDBC
## JDBC steps
```java
//open connection and participate in transaction
Connection conn=datasource.getConnection();
//prepare sql statement
PreparedStatement ps = conn.prepareStatement(sql);
//execute query to get result set
ResultSet rs=ps.executeQuery();
//*(we do this) for each resutlt set, perform certain operation
while(rs.next()){
	list.add(new Person(...));
}
conn.close();
//hanlde exception
try..catch
//release connection
```

## `JdbcTemplate`
handle everything else except 

"for each resultSet, do the work" 

and "replace SQLExceptions by runtime exceptions - DataAccessExceptions"

thread safe

### How JdbcTemplate works
```java
//in a repository class
public class JdbcCustomerRepository {
	
	// JdbcTemplate instance createe;

	//Integer, Date, Long, String,...
	jdbcTemplate.queryForObject(sql, Integer.class);
	jdbcTemplate.queryForMap(sql, Integer.class);
	jdbcTemplate.queryForList(sql, Integer.class);
	//query an integer, i.e. count(*), age binds to first ? and nationality binds to second ?
	jdbcTemplate.queryForObject(sql, Integer.class, age, nationality.toString());

	//query domain object
	class PersonMapper implements RowMapper<Person>{
		public Person mapRow(ResultSet rs, int rowNum) throws SQLException{
			//sql = "select first_name, last_name from person"
			return new Person(rs.getString("first_name"), rs.getString("last_name"));
		}
	}
	//for single row 
	Person singlePerson = jdbcTemplate.queryForObject(sql, new PersonMapper(), id);
	//for multiple rows
	List<Person> multiPerson = jdbcTemplate.query(sql, new PersonMapper());
	

	//query but no return object, instead streaming mutiple rows to a file/xml
	public void generateReport(final PrintWriter out){
		jdbcTemplate.query(sql, 
			(RowCallbackHandler)(rs)->
			{out.write(rs.getString("customer"))}, 
			2009);
	
	//process entire resultset at once
	//ResultSet with multiple rows maps to a single object
	Order order = jdbcTemplate.query(
		sql,
		(ResultSetExtractor<Order>)(rs) -> {
			Order order = null;
			while(rs.next()){
				if(order==null){
					order = new Order(rs.getLong("ID"), rs.getString("NAME"));
				}
				order.addItem(mapItem(rs));
			}
			return order;
		},
		number	
		);

	//insert and update
	//sql = "insert into person(id, name) values(?,?,?)"
	//sql = "update person set id=? where name=?"
	jdbcTemplate.update(sql, person.getId(), person.getName());
}
```
# Transaction
## What is transaction?
For the same connection, it is reused. Within one connection, there are multiple transactions, each of the transaction is atomic and runs unit-of-work.

## What is Transaction ACID principles?
- Atomic : sucess or fail
- Consistent : integrity constraints not violated
- Isolated :transactions are isolated in some levels 
(READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE)
- Durale : commit is permanent

## What is propergation?
In the single transaction, when method1 running in the transaction, method2 is called, how would method2 transaction do? should method2 creates a new transaction and suspend transaction of method1, or should method2 join method1 transaction?
REQUIRED: if existing, new method join in existing transaction
REQUIRES_NEW: if existing, suspend the existing one, and create a new transaction.

## Transction, ORM/JDBC and Spring
- JDBC, ORMs, JTA, JMS, etc handle transaction
- usually the transaction demarcation is between get connection/transaction and commit();

>Spring transaction unites different APIs' transaction.
>by declare a PlatformTransactionManager bean which have severl implementations for different APIs' transactions

## How to use Spring transaction
```java
//in configuration implements PlatformTransactionManger bean

//@Configuration @EnableTransactionManagement

@Bean
public PlatformTransactionManger transactionManager(DataSource datasource){
	return new DataSourceTransactionManager(dataSource);
}

//annotate @Transactional in class/method level where the query/database interaction works
//method level overrides class level transaction
@Transactional 
public class RewardNetworkImpl implements RewardNetwork{
	@Transactional(timeout=45, 
		isolation=Isolation.READ_COMMITTED, 
		propagation=Propagation.REQUIRES_NEW,
		rollbackFor=MyCheckedException.class, noRollbackFor={JmsException.class, MailException.class})
	public RewardConfirmation rewardAccountFor(Dining d){
		//ACID work
	}
}

```
#JPA
#Data
#Security
#AOP
#IoC
```java
```