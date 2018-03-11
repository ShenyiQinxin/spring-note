# Microservices using Spring
>building Cloud Native Applications
### What is Microservices Architecture?
>Main application has been divided in a set of sub-applications , different from "monolith” architecture.
### Pros and Cons of Microservices
#### Pros
- Core Spring Concepts Applied to Application Architecture
	-   Spring enables separation-of-concerns  
    – Loose Coupling: Effect of change is isolated  
    – Tight Cohesion: Code performs a single well-defined task
    
	-   Microservices exhibit the same strengths 
		-  Loose Coupling
    
		    - Applications are built from collaborating services (processes)
    
		    - Can change independently so long as protocols unchanged
		- Tight Cohesion
			- 	An application (service) that deals with a single view of data
			- Also known as “Bounded Contexts” (Domain-Driven Design)
#### Benefits
-   Smaller code base is easy to maintain
    
-   Easy to scale
    
    – Scale individual component
    
-   Technology diversity
    
    – Mix libraries, frameworks, data storage, languages
    
-   Fault Isolation
    
    – Component failure should not bring whole system down
    
-   Better support for smaller, parallel teams
    
-   Independent deployment
#### Challenges
-   Difficult to achieve strong consistency across services – ACID transactions do not span multiple processes  
    – Eventual consistency, Compensating transactions
    
-   Distributed system  
    – Harder to debug/trace  
    – Greater need for end-to-end testing  
    – Expect, test for and handle the failure of any process – More components to maintain: redundancy, HA
    
-   Typically requires “cultural” change (Dev Ops)  
    – How applications are developed and deployed  
    – Devs and Ops working together, even on same team!
## Developing Microservices
-   Setup a new service using Spring Boot
    
-   Expose resources via a **RestController**
    
-   Consume remote services using **RestTemplate**
    
-   Will leverage capabilities from **Spring Cloud Project**
### Spring Cloud
- **Building blocks** for **Cloud and Microservice applications** 
- Microservices **Infrastructure**

	-   Provides useful services such as Service Discovery, Configuration Server and Monitoring
    
	-   Several based on other Open Source projects
    

	- Netfix OSS, HashiCorp's Consul, ...

- Support **Platforms**   
	-	Access platform-specific information and services
	-	Available for Cloud Foundry, AWS and Heroku – Uses Spring Boot style starters

- Requires **Spring Boot** to work
## Tooling: Spring, Spring Cloud, Netflix
### Spring Cloud Eco-system
> Spring cloud makes it easy to use these servers i.e. discovery server
![enter image description here](https://user-images.githubusercontent.com/32177380/36935709-f05e4efa-1ec8-11e8-81c7-2d9ad9d1a8dc.png)

- Cloud Integration, Dynamic Reconfiguration, Service Discovery, Security, Data Ingestion
 >- services find each other: service discovery server- **Eureka**
 >  returns the location of multiple instances
 > 
  >- multiple instances for fault-tolerance and load-sharing:  Client-side Load Balancing - **Ribbon**
  > 
  > RestTemplate
  > - with Service-discovery and load-balancing built-in
  > @LoadBalanced


## Building a Simple Microservice System
> Discovery Service **Eureka** 
> Eureka registers Accounts-MicroService (Producer) with Eureka
> &
>Eureka performs lookup for front-end web application (Consumer) 
>
> **Consumer** requests data from **Producer**
> through RESTful API
![enter image description here](https://user-images.githubusercontent.com/32177380/37254503-285ba7ba-2515-11e8-8a3b-deec29c11db5.png) 
### Maven Dependencies
```xml
<parent>  
<artifactId>spring-cloud-starter-parent</artifactId> 
...
<dependency> 
<artifactId>spring-boot-starter-web</artifactId>
<artifactId>spring-cloud-starter</artifactId> 
<artifactId>spring-cloud-starter-eureka</artifactId>
...
```
### Eureka Server using Spring Cloud
>**Eureka Server** perfoms discovery service
>**@EnableEurekaServer**
```java
@SpringBootApplication 
@EnableEurekaServer  
public class EurekaApplication  {
public static void main(String[]  args)  {
 SpringApplication.run(EurekaApplication.class, args);
} }
```
> dependency
```xml
<dependency>
<artifactId>spring-cloud-starter-eureka-server</artifactId>
```
>application.yml
```yml
server:
	port: 8761
eureka:
	instance:
		hostname: localhost
	client: #not a client
		registerWithEureka: false //it is Eureka itself
		fetchRegistry: false
```
### Accounts Producer Microservice
>Accounts Server (producer)
>declares itself as an available service and register with Eureka
– Using **@EnableDiscoveryClient**  
– Registers using its logic service name
```java
@SpringBootApplication 
@EnableDiscoveryClient  
public  class  AccountsApplication  {
	public static void main(String[] args) {
		SpringApplication.run(Application.class,  args); 		} 
}
```
>
```yml
spring:
	application:
		name: accounts-microservice #logic service name
eureka
	client: 
		serviceUrl:
			defaultZone: http://localhost:8761/eureka/
			#eureka server url	
```
### Consumer Service
>- declares itself as an available service and register with Eureka using
>**@EnableDiscoveryClient**
>- annotating **@LoadBalanced** allows load-balanced (**Ribbon**) service lookup (consumer query Eureka to find producer) 
```java
@SpringBootApplication
@EnableDiscoveryClient
public class FrontEndApplication { 
	public  static  void  main(String[]  args)  {
		SpringApplication.run(Application.class, args);
	}

//  Declares  template  to  access  the  microservice
//  - Create using @LoadBalanced – an @Qualifier  
//  – Spring enhances it to do service lookup & load-balancing
@Bean @LoadBalanced 
public  RestTemplate  restTemplate()  {
	return  new  RestTemplate(); }
}
```
>
```java
@Service  
public  class  RemoteAccountManager  implements  AccountService  {

//  Spring  injects  the  “smart”  service-aware  template
// It performs a load-balanced lookup  
//-   Must inject using same qualifier  
//   – If there are multiple RestTemplates you get the right one – Can be used to access multiple microservices
	@Autowired  @LoadBalanced 
	RestTemplate  restTemplate;
public Account findAccount(String id) { 
//  Fetch  data  
	return  restTemplate.getForObject("http://account-microservice/accounts/{id}", Account.class,  id);

} }
```
-   Our “smart” RestTemplate automatically integrates two Netflix utilities
    
    – “Eureka” service-discovery
    
    – “Ribbon” client-side load-balancer
    
-   End result
    
    – Eureka returns the URL of all available instances
    
    – Ribbon determines the best available service to use
    
-   Just inject the load-balanced RestTemplate
    
    – Automatic lookup by logical service-name
