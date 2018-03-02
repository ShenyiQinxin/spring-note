# Spring Security
## Overview
### Concepts
- Principal  
>User, device or system that performs an action
- Authentication  
>Establishing that a principal’s **credentials are valid**
- Authorization  
>Deciding if a principal **is allowed** to perform an action
- Secured item  
>**Resource** that is being secured
### Authentication
- mechanisms
>- custom UserDetailsService delegating to an existing DAO
>- Single-sign-on: OAuth,SAML, SiteMinder, Kerberos, JA-SIG Central Authentication Service 
>- LDAP 
>- JAAS Login Module
>- X.509 certificates
- storage options for credential and authority information:
>Database, LDAP, in-memory (development)
### Authorization
-   Authorization depends on authentication 
-  establish user identity -user role
- deciding if a user can perform an action based on roles
i.e.
	- ADMIN can cancel orders  
	-	MEMBER can place orders  
    -	GUEST can browse the catalog
## Motivations of Spring Security
- Spring Security supports securied achive be deployed in server
- also supports application runing standalonely
- Security **config** is in Spring config
- Security is **decoupled** from Spring business logic
- authentication and authorization are decoupled
- supports all comment **security mechanism**
	- Basic, Form, X.509, Cookies, Single-Sign-On, etc.
- Configurable **storage** options for user details (credentials and authorities): RDBMS, LDAP, custom DAOs, properties file, etc.
- All the following can be customized  
	- How a **principal** is defined  
	- How **authorization** decisions are made
	- Where security constraints are **stored**
- consistency of authentication and authorization
>
![enter image description here](https://user-images.githubusercontent.com/32177380/36878353-97f34168-1d8c-11e8-8405-c888a499995e.png)
## Spring Security in a Web Environment
- SpringSecurityFilterChain -- DelegatingFilterProxy
- @EnableWebSecurity
- AbstractSecurityWebApplicationInitializer
- <filter> in web.xml
### Configuration in the ApplicationContext
```java
@Configuration  
@EnableWebSecurity  
public class SecurityConfig extends WebSecurityConfigurerAdapter {

@Override
//Web-specific security settings
protected void configure(HttpSecurity http) throws Exception {  
} Web-specific security settings

@Autowired
//General security settings : authentication manager etc
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {

} }
```
#### Adds specific authorization requirements to URLs
> first match is used, put specific matches first 
```java
protected void configure(HttpSecurity http) throws Exception { 
http.authorizeRequests() .antMatchers("/css/**","/images/**","/javascript/**").permitAll() 
.antMatchers("/accounts/edit*").hasRole("ADMIN") 
.antMatchers("/accounts/account*").hasAnyRole("USER",”ADMIN”) 
.antMatchers("/accounts/**").authenticated() .antMatchers("/customers/checkout*").fullyAuthenticated() 
.antMatchers("/customers/**").anonymous();
```
#### Specifying login and logout
```java
protected void configure(HttpSecurity http) throws Exception { 
http.authorizeRequests() .antMatchers("/aaa*").hasRole("ADMIN")
.and()// method chaining!
.formLogin() //setup form-based authentication
.loginPage("/login.jsp") .permitAll()  
.and()// method chaining!
.logout()//configure logout
.permitAll();
```
### Login Security
```html
<c:url var=’loginUrl’value=’/login.jsp’ /\> <form:form action=“${loginUrl}” method=“POST”>

<input type=“text” name=“username”/\> <br/>  
<input type=“password” name=“password”/\> <br/>

<input type=“submit” name=“submit” value=“LOGIN”/\> </form:form>
```
## Configuring Web Authentication
### @Profile
```java
public class SecurityBaseConfig extends WebSecurityConfigurerAdapter { 
protected void configure(HttpSecurity http) throws Exception {

http.authorizeRequests().antMatchers("/resources/**").permitAll(); }

}
@Configuration @EnableWebSecurity
@Profile(“development”)
public class SecurityDevConfig extends SecurityBaseConfig {  
@Autowired  
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {

//using in-memory provider
auth.inMemoryAuthentication().withUser("hughie").password("hughie").roles("GENERAL");

} }

//using database provider
@Profile(“production”)
public class SecurityProdConfig extends SecurityBaseConfig {  
@Autowired  
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
auth.jdbcAuthentication().dataSource(dataSource); }

}
```
### configureGlobal()
- sourcing users from a database
i.e.
```java 
usersByUsernameQuery();
authoritiesByUsernameQuery();
groupAuthoritiesByUsername()
```
- Password encoding
	- sha-256, md5, bcrypt, salt (using a well-known string)
```java
@Autowired DataSource dataSource;

@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception { 

auth.inMemoryAuthentication()
.withUser("hughie").password("hughie").roles("GENERAL")
.and()
.withUser("dewey").password("dewey").roles("ADMIN")
.and()
.withUser("louie").password("louie").roles("SUPPORT");

auth.jdbcAuthentication()  
.dataSource(dataSource)  
//with "salt"
//new StandardPasswordEncoder(“sodium-chloride”)
//sha-256
.passwordEncoder(new StandardPasswordEncoder());

}
```

## Using Spring Security's Tag Libraries
### declared as following
```html
//------Spring Security tag library is declared
<%@  taglib  prefix="security" uri="http://www.springframework.org/security/tags"  %>

//-------In JSF, Principal available in SpEL
SpEL.  #{principal.username}

//------Display properties of the Authentication object!
<security:authentication property=“principal.username”/>

//------Hide sections of output based on role
<security:authorize access=“hasRole('ADMIN')”>  
TOP-SECRET INFORMATION  
Click <a href=“/admin/deleteAll”>HERE</a> to delete all records.
</security:authorize>

//------Pattern that matches the URL and roles to be protected
<security:authorize url=“/admin/deleteAll”\> TOP-SECRET INFORMATION  
Click <a href=“/admin/deleteAll”>HERE</a>
</security:authorize>
//In Spring config file
.antMatchers("/admin/*").hasAnyRole("MANAGER",”ADMIN”)
```
## Method security
>JSR-250
```java
@EnableGlobalMethodSecurity(jsr250Enabled=true)

import  javax.annotation.security.RolesAllowed;

public  class  ItemManager  { 
@RolesAllowed({"ROLE_MEMBER",  "ROLE_USER"}) 
public  Item  findItem(long  itemNumber)  {

... }

}
```
>
>@Secured
```java
@EnableGlobalMethodSecurity(securedEnabled=true)

import  org.springframework.security.annotation.Secured;
public  class  ItemManager  { 
//@Secured("ROLE_MEMBER") 
//@Secured({"ROLE_MEMBER", "ROLE_USER"})
@Secured("IS_AUTHENTICATED_FULLY") 
public  Item  findItem(long  itemNumber)  {

... }

}
```
>
>Method Security with SpEL
```java
@EnableGlobalMethodSecurity(prePostEnabled=true)

import org.springframework.security.annotation.PreAuthorize;

public class ItemManager { @PreAuthorize("hasRole('MEMBER')") public Item findItem(long itemNumber) {

... }

}
```
## Advanced security: working with filters
>springSecurityFilterChain
>- Spring Boot does it automatically/auto-configured
>- or configure it in Servlet configuration
>- delegates to a chain of Spring- managed filters
	- Drive authentication  
	- Enforce authorization  
	- Manage logout  
	- Maintain SecurityContext in HttpSession 
	- and more
```xml
<filter> 
<filter-name>springSecurityFilterChain</filter-name> 
<filter-class>
org.springframework.web.filter.DelegatingFilterProxy
</filter-class> 
</filter>

<filter-mapping> 
<filter-name>springSecurityFilterChain</filter-name> 
<url-pattern>/*</url-pattern>
</filter-mapping>
```
### FilterChain mechanism
![enter image description here](https://user-images.githubusercontent.com/32177380/36907450-b050b838-1e06-11e8-9bf9-7a26d2db6058.png)
### Custom Filter Chain

## Summary
- Spring Security  
    - – Secure URLs using a chain of Servlet filters  
    - – And/or methods on Spring beans using AOP proxies
    
-   Out-of-the-box setup usually sufficient – you define:
    
    -   – URL and/or method restrictions
        
    -   – How to login (typically using an HTML form)
        
    -   – Supports in-memory, database, LDAP credentials (and more)
        
    -   – Password encryption using familiar hashing techniques
        
    -   – Support for security tags in JSP views

