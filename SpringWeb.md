# Spring Web
## web application architecture

**clients** --remote http interfaces--> 
{[**web layer** --localJava business interfaces-->
 **application layer (Spring)**
 ]*Java EE servlet container*
 }*JVM*
![web application architecture](https://user-images.githubusercontent.com/32177380/36849383-eea4fac0-1d31-11e8-9f96-9ef2be9e2214.png)
![servlet container after starting up](https://user-images.githubusercontent.com/32177380/36849592-7093cf48-1d32-11e8-8873-d814d87d9b28.png)
## Spring Web Application Context
> ApplicationContext initialization
> servlet
> - gets Spring ApplicationContext from ServletContext
```java
public class TopSpendersReportGenerator extends HttpServlet{ 
	private ClientService clientService;

	public void init() {  
	ApplicationContext context = WebApplicationContextUtils.

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

org.springframework.web.context.ContextLoaderListener

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
>--**model** (logic view)-->*dispatcher servlet* --> **view resolver** --
>return view --> *dispatcher servlet* --render model--> **view** --
>responde --> *dispatcher servlet*-->client
![enter image description here](https://user-images.githubusercontent.com/32177380/36821929-73190dec-1cc3-11e8-972a-8415a8e8a8ea.png)
### Key Artifacts
#### DispatcherServlet
>-   coordinates all request handling activities
>-   Delegates to Web infrastructure beans
>-   Invokes user Web components
>-   interfaces for all infrastructure beans
-   servlet using WebApplicationInitializer or web.xml
- Beans defined in MVC Context have access to beans defined in Root Context
```java
public class MyWebInitializer  
extends AbstractAnnotationConfigDispatcherServletInitializer {

// Tell Spring what to use for the Root context:  
@Override protected Class<?>[] getRootConfigClasses() {

return new Class<?>[]{ RootConfig.class }; }

// Tell Spring what to use for the DispatcherServlet context: @Override protected Class<?>[] getServletConfigClasses() {

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
#### Views

