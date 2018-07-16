# Spring Web
## web application architecture
>Internal mechanism:
>
**clients** --remote http interfaces--> 
{[**web layer** --localJava business interfaces-->
 **application layer (Spring)**
 ]*Java EE servlet container*
 }*JVM*
 


----------


![web application architecture](https://user-images.githubusercontent.com/32177380/36849383-eea4fac0-1d31-11e8-9f96-9ef2be9e2214.png)
>
![servlet container after starting up](https://user-images.githubusercontent.com/32177380/36849592-7093cf48-1d32-11e8-8873-d814d87d9b28.png)
-   Spring can be used with any web framework  
 - Spring internally provides the **ContextLoaderListener** (load a>
![servlet and config](https://user-images.githubusercontent.com/32177380/36850097-cf73f884-1d33-11e8-9ff6-a12501816dbf.png)
## Spring Web Application Context
> ApplicationContext initialization
> servlet
> - gets Spring ApplicationContext from ServletContext
```java
public class TopSpendersReportGenerator extends HttpServlet{ 
	private ClientService clientService;

	public void init() {  
	ApplicationContext context)
 - and **DispatcherServlet**  (load web layer context, plug in *mvcConfig.class*(ReviewResolver) which calls *appConfig.class* )
 - that can be declared in **web.xml**
 -   Spring MVC is a lightweight web framework where controllers are Spring beans. 
- Spring implements  
>
![servlet and config](https://user-images.githubuser = WebApplicationContextUtils.

	getRequiredWebApplicationContext(getServletContext()); clientService = (ClientService) context.getBean(“clientService”);

	}

... 
}
```
> application ready
> - DI in controller 
```java
@Controller  
public  class  TopSpendersReportController  {
	private  ClientService  clientService;
	@Autowired  
	public  TopSpendersReportController(ClientService  service)  {

	this.clientService  =  service;
}
```
> ApplicationContext.close();
### configuration
>Java

    AbstractContextLoaderInitializer{
	    new AnnotationConfigWebApplicationContext()
	    .getEnvironment()
	    .register(Config.class);
    }

```java
public class MyWebAppInitializer  
extends AbstractContextLoaderInitializer {

@Override

protected WebApplicationContext createRootApplicationContext() {

// Create the 'root' Spring application context

AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();

rootContext.getEnvironment().setActiveProfiles("jpa"); // optional rootContext.register(RootConfig.class);  
return rootContext;

} 

//define servlet
public void onStartup(ServletContext container) { super.onStartup(container);  
// Register and map a servlet 
ServletRegistration.Dynamic svlt = container.addServlet("myServlet", new TopSpendersReportGenerator()); svlt.setLoadOnStartup(1);  
svlt.addMapping("/");

}
```
>XML
```xml
<context-param> <param-name>contextConfigLocation</param-name> <param-value>

/WEB-INF/merchant-reporting-webapp-config.xml

</param-value> </context-param>

<listener> <listener-class>

org.springframework.web.contenxt.com/32177380/36850097-cf73f884-1d33-11e8-9ff6-a12501816dbf.png)ContextLoaderListener

</listener-class> </listener>
```

## Spring Web includes:
MVC, WebFlow, Mobile, Social

## Spring Web MVC
### MVC
a web framework based on Model View Controller pattern
> - alternative of JSF, struts etc
> - POJO, testable, Spring configuration
> - support view techs of JSP, XSLT, PDF, Excel, Velocity, Freemarker, Thymeleaf etc.
### request processing lifecycle
>client --request **URL** -->*Dispatcher Servlet* --dispatch request--> **controller**
>-->**model** contains data , **logic view**(for instance, a String) is returned(logic view)-->*dispatcher servlet* --consults> **view resolver** --
>return view --> *dispatcher servlet* --render model--> **view** --
>responde --> *dispatcher servlet*-->client
![enter image description here](https://user-images.githubusercontent.com/32177380/36821929-73190dec-1cc3-11e8-972a-8415a8e8a8ea.png)
### Key Artifacts
#### About DispatcherServlet
>-   coordinates all **request** handling activities
>-   **Delegates to** / invokes Web infrastructure beans
>-   **Invokes user Web components
>-   interfaces** for all infrastructure beans
>-   Sservlet callsusing WebApplicationInitializer or web.xml
cus- Beans defined in MVC Context have access to beans defined in Root Context
```java
public class MyWebInitializer  
extends AbstractAnnotationConfigDispatcherServletInitializer {

// Tell Spring what to use for the Root context:  
@Override 
protected Class<?>[] getRootConfigClasses() {
	
return new Class<?>[]{ RootConfig.class }; }

// Tell Spring what to use for the DispatcherServlet context: @Override 
protected Class<?>[] getServletConfigClasses() {
	
return new Class<?>[]{ MvcConfig.class }; }

// DispatcherServlet mapping:

@Override protected String[] getServletMappings() { 
	return new String[]{"/rewardsadmin/*"};

}
```
| servlet container  | what's in it|
|--|--|
| ServletContainerInitializer |interface from servlet3 to initialize servlet|
| SpringServletContainerInitializer|Spring's implementation|

| servlet config  | what's in it |
|--|--|
|WebApplicationInitializer|servlet config/web.xml|
| AbstractContextLoaderInitializer| ContextLoaderListener& app root context|
|AbstractAnnotationConfigDispatcherServletInitializer| dispatcherServlet & root config and mapping|
> beans defined in web context have access to  beans defined in RootApplicationContext
#### Controllers
- controller impl
```java
@Controller

public class AccountController {

	@RequestMapping("/listAccounts")
	
public String list(Model model) {...} 
}
```
- Controller Method Parameters
	- HttpServletRequest 
	- HttpSession
	- Principal
	- Model
```java
//1
Model model

//2 @RequestMapping("/showAccounts")
//http://localhost:8080/mvc-1/rewardsadmin/showAccount.htm?entityId=123
@RequestParam("entityId") long id

//3 @RequestMapping("/listAccounts/{accountId}")
@PathVariable("accountId") long id

//4
HttpServletRequest request

//5
@RequestHeader(“user-agent”) String agent

//6
Principal user

//7
HttpSession session
```
#### Views
- **Controllers** return a logic view name by a **String**
- DispatcherServlet delegates to **ViewResolvers** that select **View** based on **view name**

#### View Resolvers
- to override the default, registerdefine a ViewResolver bean within DispatcherServlet in MvcConfig.class using InternalResourceViewResolver and setPrefix, setSuffix
- by default, **JSP**: */WEB-INF/viewsreward/list.jsp* would be athe view of named "*list.jsp*"

![view resolver define](https://user-images.githubusercontent.com/32177380/36853241-f110c820-1d3b-11e8-93b2-8c33cafd0156.png)

## Developing a Spring MVC application
Steps:
1.  Deploy a DispatcherServlet (one-time only)
2.  **Implement** a **Controller**
3. Register the Controller with the DispatcherServlet
4.  Implement the **Views**
5.  **Register** a ViewResolver (one-time only & optional)
6. Deploy and Test
repeat steps 2-6 to develop new functionality
>
>**Deploy a DispatcherServlet**
>initialize the DispatcherServlet through WebInitializer/DispatcherServletInitializer
```java
class WebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer{
getRootConfigClasses();//Spring App config(services, repositories etc)
getServletConfigClasses();//Spring MVC configs
getServletMappings();//DispatcherServlet mapping
...
}
```
>**initial Spring MVC config**
>DisatcherServlet 
```java
@Configuration  
public class MvcConfig {
	// No beans required for basic Spring MVC usage.
	// Spring MVC defines several beans automatically
	//could override ViewResolvers
	
}
```
- to override the default, register a ViewResolver bean with DispatchServlet in MvcConfig.class using InternalResourceViewResolver and setPrefix, setSuffix
- by default, **JSP**: */WEB-INF/views/list.jsp* would be a view named "*list.jsp*"
- i.e.
![enter image description here](https://user-images.githubusercontent.com/32177380/37242803-3b8eb4ea-243d-11e8-8cce-7fa29925f32d.png)
>**Implement a Controller**
```java
@Controller

public class RewardController {  
	private RewardLookupService lookupService;

	@Autowired
	
public RewardController(RewardLookupService svc) { this.lookupService = svc;
	
}

	@RequestMapping("/reward/show")  
	public String show(@RequestParam("id") long id, 

Model model) {  
		Reward reward = lookupService.lookupReward(id);
		
model.addAttribute(“reward”, reward);
		
return “rewardView”; }
	}
}
```
>**Register the Controller with the DispatcherServlet**
>by **ComponentScan** in this case
```java
@Configuration
@ComponentScan("accounts.web") 
public class MvcConfig() {
```
>**Implement the Views**
```html
<html>  
<head>
	<title>Your Reward</title>
</head> 
<body>
Amount=${reward.amount} <br/> 
Date=${reward.date} <br/>  
Account Number=${reward.account} <br/> 
Merchant Number=${reward.merchant}
</body> 
</html>
```
>- *reward* is the model name,
>- /WEB-INF/views/*rewardView*.jsp
>- no references to Spring object / tags required in JSP
>
>**Register ViewResolver**
> with the DispatcherServlet by **ViewResolver bean**
```java
@Configuration 
@ComponentScan("accounts.web") 
public class MvcConfig {

	@Bean
	public ViewResolver simpleViewResolver() { 
	InternalResourceViewResolver vr = 
	new InternalResourceViewResolver(); 
	vr.setPrefix ( "/WEB-INF/views/" ); 
	vr.setSuffix ( ".jsp" );  

	return vr;

}
```
>i.e. Controller returns **"rewardList"**
> ViewResolver converts to */WEB-INF/views/rewardList.jsp*
>
>**Deploy and Test**

    http://localhost:8080/rewardsadmin/reward/show?id=1
    Your Reward
    
    Amount = $100.00  
    Date = 2006/12/29  
    Account Number = 123456789 Merchant Number = 1234567890
## Summary
-   Spring MVC is Spring's web framework  
	
----
```java
public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
      // Create the 'root' Spring application context
    -  @Controller classes handle HTTP requests 
	AnnotationConfigWebApplicationContext rootContext =
        new AnnotationConfigWebApplicationContext();
      rootContext.register(AppConfig.class);

    -  URL information available
	     - @RequestParam, @PathVariable 
	 -  Data returned via the Model  
    - Output (HTML) generated by Views
    
-   Multiple View technologies supported  
    - ViewResolvers define where Views can be found// Manage the lifecycle of the root application context
      container.addListener(new ContextLoaderListener(rootContext));

      // Create the dispatcher servlet's Spring application context
      AnnotationConfigWebApplicationContext dispatcherContext =
        new AnnotationConfigWebApplicationContext();
      dispatcherContext.register(DispatcherConfig.class);

      // Register and map the dispatcher servlet
      ServletRegistration.Dynamic dispatcher =
        container.addServlet("dispatcher", new DispatcherServlet(dispatcherContext));
      dispatcher.setLoadOnStartup(1);
      dispatcher.addMapping("/");
    }

 }
```


