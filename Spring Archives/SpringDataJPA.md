# JPA

## JPA concepts
- **Entity Manager**
	- manage persistent objects in **PersistentContext**
	- Lifecyle bounds to transaction
- **Eentity Mmanager Ffactory**
	- thread-safe, shareable object represents a single **data source**/persistence unit
	- provides the access to new EntityManager
## Entity manager work flow

**EntityMangerFactory** --creates--> 
>**EntityManager**--maps--> (application context)
>**List<*Person*> instances** --to--> 
>**Entities** --> persist to --> (persistence context)
>**database**
![enter image description here](https://user-images.githubusercontent.com/32177380/37162073-534b58f8-22c2-11e8-9504-cff8d995b456.png)
## EntityManager API
| method |persistence context  |SQL
|--|--|--
| **persist**(Object o) | persist entity to *   |insert into table
|**remove**(Object o)|remove entity from *| delete from table
|**find**(Class entity, Object pk)|find entity by pk|select * from table where id=?
|**query** createQuery(String jpql)|create a JPQL query|sql
|**flush**()|force entity be writtern to db immediately|and more..
## JPA implementations/providers
- hibernate entitymanager(in Jboss)
- eclipseLink(in Glassfish)
- Apache OpenJPA (in WebLogic and Websphere)
- Data Nucleus (in Google App Engine)
## JPA Mapping
> JPA requires metadata  (annotation / orm.xml(override purpose)) for mapping **class/field** to **table/column**
### class - table
```java
@Entity  
@Table(name=  “T_CUSTOMER”)

public  class  Customer  { 
@Id
@Column(name=“cust_id”) private  Long  id;

@Column(name=“first_name”) private  String  firstName;

@Transient  
private  User  curentUser;
```
> only **@Entity** and **@Id** are mandatory
> **@Column** and **@Table (name="...")** to override the names of tables and columns
### field - column
- persistent by default
- @Transient : not persistent to the table
- using reflection to achieve field access, no setters needed 
### Properties / Getters - column
```java
@Entity  
@Table(name=  “T_CUSTOMER”) 
public  class  Customer  {
private  Long  id;  
private  String  fiirstName;
@Id  
@Column  (name=“cust_id”) 
public  Long  getId()
{  return  this.iid;  }
@Column  (name=“first_name”) 
public  String  getFirstName()
{  return  this.firstName;  }
public  void  setFirstName(String  fn) {  
this.firstName  =  fn;  }

}
```
### relationships
one-to-one , one-to-many, uni, bi-directional
>
```java
//bi-directional
@Entity
public class OrderItem {
    @ManyToOne
    @JoinColumn(name = “fk_order”)
    private Order order;
    …
}
@Entity
public class Order {
//the order attribute of the OrderItem entity.
    @OneToMany(mappedBy = “order”)
    private List<OrderItem> items = new ArrayList<OrderItem>();
    …
}

//uni-directional
@Entity  
@Table(name= “T_CUSTOMER”) 
public class Customer {
	@Id  
	@Column (name=“cust_id”) 
	private Long id;
	
	@OneToMany  
	@JoinColumn (name=“cid”) 
	private Set<Address> addresses;
}

@Entity  
@Table(name= “T_ADDRESS”) 
public class Address {
	@Id private Long id; 
	private String street; 
	private String suburb; 
	private String city; 
	private String postcode; 
	private String country;
...
```
>column _cid_ is the FK in T_ADDRESS table
### embeddables
```java
@Entity  
@Table(name= “T_CUSTOMER”) 
public class Customer {

	@Id  
	@Column (name=“cust_id”) private Long id;
//Maps to ZIP column in T_CUSTOMER
	@Embedded @AttributeOverride
	(name="postcode", column=@Column(name="ZIP")) 
	private Address office;

@Embeddable
public class Address { 
	private String street; 
	private String suburb; 
	private String city; 
	private String postcode; 
	private String country;
}
```, where in parent table T_CUSTOMER, the join column is cust_id the PK.
### embeddables
## JPA querying
### by PK
```java
Customer customer = entityManager.find(Customer.class, customerId);
```
### by JPQL
```java
//  Query  with  named  parameters  
TypedQuery<Customer>  query  =  entityManager.createQuery(

“select  c  from  Customer  c  where  c.address.city  =  :city”,  Customer.class); 
query.setParameter(“city”,  “Chicago”);  
List<Customer>  customers  =  query.getResultList();

//  ...  or  using  a  single  statement 
List<Customer>  customers2  =  entityManager.

createQuery(“select  c  from  Customer  c  ...”,  Customer.class). setParameter(“city”,  “Chicago”).getResultList();

//  ...  or  if  expecting  a  single  result  
Customer  customer  =  query.getSingleResult();
```
### by dbms-specific sql
>    Consider JdbcTemplate instead

## JPA configuration 
### persistence unit - data source
- entities - persistent classes
- providers
- transaction types - local or JTA
- could be mutiple units per application
- persistence.xml
### Steps using JPA in Spring
![enter image description here](https://user-images.githubusercontent.com/32177380/37341989-b012fcd2-2699-11e8-8442-d476ba67a266.png)
1. EntityManagerFactory = EntityManagerFactoryBean.getObject()
2. DataSource bean
3. TransactionManager bean (Spring Proxy)
4. Mapping Metadata
5. DAOsteps
#### 1. define a EntityManagerFactory bean
>*LocalContainerEntityManagerFactoryBean class*
>
>Java config
```java
@Bean  
//FactoryBean is used
public LocalContainerEntityManagerFactoryBean entityManagerFactory(){

HibernateJpaVendorAdapter adapter = new HibernateJpaVendorAdapter(); 
adapter.setShowSql(true);  
adapter.setGenerateDdl(true);  
adapter.setDatabase(Database.HSQL);

Properties props = new Properties(); 
props.setProperty("hibernate.format_sql", "true");

LocalContainerEntityManagerFactoryBean emfb =  
new LocalContainerEntityManagerFactoryBean();

emfb.setDataSource(dataSource); 
//no persistence.xml needed now
emfb.setPackagesToScan("rewards.internal"); 
emfb.setJpaProperties(props); 
emfb.setJpaVendorAdapter(adapter);

return emfb; 
}
```
#### 2. define a DataSource bean 
```java
@Bean  
public DataSource dataSource() { // Lookup via JNDI or create locally. }
```
#### 3. XML config
```xml
<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"\> <property name="dataSource" ref="dataSource"/>  
<property name="packagesToScan" value="rewards.internal"/>

<property name="jpaVendorAdapter">  
<bean class="org.sfwk.orm.jpa.vendor.HibernateJpaVendorAdapter">

<property name="showSql" value="true"/\> <property name="generateDdl" value="true"/\> <property name="database" value="HSQL"/>

</bean> </property>

<property name="jpaProperties">  
<props\> <prop key="hibernate.format_sql">true</prop\> </props>

</property\> </bean>
```
#### define a DataSource bean
#### define a TransactionManager bean
>*PlatformTransactionManager I<-- JpaTransactionManager class*
```java
@Bean  
public PlatformTransactionManager 

transactionManager(EntityManagerFactory emf) { 
	//emf is gotten from Spring calling emfb.getObject() 
	return new JpaTransactionManager(emf);
//or JtaTransactionManager

}

@Bean  
public DataSource dataSource() { // Lookup via JNDI or create locally. }
```
#### 4. define  mapping metadata
#### 5. define DAOs / *Repository
>no spring involved in DAOs
```java
public class JpaCustomerRepository implements CustomerRepository { 
	private EntityManager entityManager;
	
	//inject EntityManager with TransactionManager proxy
	@PersistenceContext
	//JPA equivalent to @Autowired in Spring
	public void setEntityManager (EntityManager entityManager) { 
		this. entityManager = entityManager;
	
}

	public Customer findById(long orderId) {  
		return entityManager.find(Customer.class, orderId);
		//when need to use EM, proxy resolves to EM
	} 
}
```
> define repository bean
```java
@Bean  
public CustomerRepository jpaCustomerRepository() {
	//entitymanager is injected by @PersistenceContext
	return new JpaCustomerRepository(); 
}
```
>XML config
```xml
<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"> 
<property name="dataSource" ref="dataSource"/>  
<property name="packagesToScan" value="rewards.internal"/>

<property name="jpaVendorAdapter">  
<bean class="org.sfwk.orm.jpa.vendor.HibernateJpaVendorAdapter">
<property name="showSql" value="true"/> 
<property name="generateDdl" value="true"/> 
<property name="database" value="HSQL"/>
</bean> 
</property>

<property name="jpaProperties">  
<props> <prop key="hibernate.format_sql">true</prop> </props>
</property> 
</bean>
```
### configuration relationships
![enter image description here](https://user-images.githubusercontent.com/32177380/37173629-1e19e968-22e2-11e8-9967-8446d563325f.png)
![enter image description here](https://user-images.githubusercontent.com/32177380/37173714-574c455a-22e2-11e8-8763-4dce8b823e3f.png)### configuration relationships

DataSource --is required by-->
**LocalContainerEntityManagerFactoryBean** --creates--> 
**EntityManagerFactory** --creates-->
**{EntityManager --proxyed by-->
PlatformTransactionManager}**
--> working in JPA CustomerRepository --called by -->
CustomerServiceImpl

### JPA vs. JTA
|JPA / JTA|Transaction Manager|
|--|--|
|JPA has single data source/EM|JpaTM|
|JTA has multiple data sources/EMs joining JTA| JtaTM|


----------


# Spring Data JPA
>**Spring Data** support many data environments
> **Spring Data JPA** is one of them
> 
|||
|--|--|
|Spring Data |core-proj
|JPA, MongoDB, JDBC ex, Redis, Hadoop etc|sub-proj

## Steps
1. annotate domain classes
2. D>define your repository as an interfakeys , enable persistence
	-> Spring will implement repository at run-time 
	- by scaning for interfaces extending Spring's `Repository<T, K>`, 
    - then autogenerate CURD methods, 
    - supports advanced customization.
	- sub-proj are varied
### 1. annotate domain classes
>define keys , enable persistence `@Entity @Id @GeneratedValue`
### 2. define your repository interface : 
- Spring will implement repository at run-time 
	- by scaning for interfaces extending Spring's `Repository<T, K>`, 
    - then autogenerate CURD methodsscans for Repository interfaces
>
>Java config
```java
@Configuration 
@EnableJpaRepositories(basePackages=com.acme.**.repository") 
@EnableMongoRepositories(...)  
public class MyConfig { 
	@Autowired
	public CustomerRepository customerRepository;

	@Bean
	public CustomerService customerService() {  
		return new CustomerService( customerRepository );
	} 
}
```
>XML config
```xml
<jpa:repositories base-package="com.acme.**.repository"/> <mongo:repositories base-package="com.acme.**.repository"/> 
<gfe:repositories base-package="com.acme.**.repository" />
```
### define your repository interface : 
```java 
//marker interface
public interface Repository<T, ID> { }

//You get all these methods automatically
public interface CrudRepository<T, ID  
extends Serializable> extends Repository<T, ID> {

public <S extends T> save(S entity);  
public <S extends T> Iterable<S> save(Iterable<S> entities);

public T findOne(ID id); 
public Iterable<T> findAll();

public void delete(ID id); 
public void delete(T entity); 
public void deleteAll();

}

PagingAndSortingRepository<T, K>  {
	Iterable<T> findAll(Sort);  
	Page<T> findAll(Pageable);
}
```
### 3. Generating Repositories by annotating on Config 
>-   Spring scans for Repository interfaces  
    – Implements them and creates as a Spring bean
> Internal Behavior – Another Spring Proxy
```java
@Configuration @EnableJpaRepositories(basePackages=com.acme.**.repository") 
@EnableMongoRepositories(...)  
public class MyConfig { ... }
```
```xml
<jpa:repositories base-package="com.acme.**.repository" /> 
<mongo:repositories base-package="com.acme.**.repository" /> 
<gfe:repositories base-package="com.acme.**.repository" />
```
### 4. Ddefine JPA repository methods
>Auto-generated finders obey naming convention  
> - find(First)By"DataMember""Op"
> - "Op" can be GreaterThan, NotEquals, Between, Like ...
```java
public interface CustomerRepository  
extends CrudRepository<Customer, Long> {

public Customer findFirstByEmail(String someEmail); // No <Op> for Equals 
public List<Customer> findByOrderDateLessThan(Date someDate);  
public List<Customer> findByOrderDateBetween(Date d1, Date d2);

@Query(“SELECT c FROM Customer c WHERE c.email NOT LIKE '%@%'”)

public List<Customer> findInvalidEmails(); 
}
```
>Extend Repository and build your own interface using conventions.
```java
//define your own CustomerRepository similar to CrudRepository<Customer, Long>
public interface CustomerRepository extends Repository<Customer, Long> {
<S extends Customer> save(S entity); // Definition as per CrudRepository
<S extends Customer> save(S entity
Customer findOne(long i); 
// Definition as per CrudRepository
Customer findOne(long i);  
// Case insensitive search 
 Customer findFirstByEmailIgnoreCase(String email); 
// ?1 replaced by method paramCase insensitive search

@Query("select u from Customer u where u.emailAddress = ?1")

Customer findByEmail(String email); 
}
```
### 5.Accessing the Repository
```java
@Configuration 
@EnableJpaRepositories(basePackages="com.acme.repository") 
public class CustomerConfig {

@Autowired
public CustomerRepository customerRepository;

@Bean
public CustomerService customerService() {  
return new CustomerService( customerRepository );

} }
```
## Spring Data annotation equvalent to JPA
```java 
@Document class -- map to a JSON document 
@Region class -- Gemfire – map to a region
@NodeEntity class
@GraphId Long id; -- Neo4J – map to a graph
```
-   Templates (like JdbcTemplate) for basic CRUD access
`MongoTemplate, GemfireTemplate, RedisTemplate`

## Summary

-   Use 100% JPA to define entities and access data – Repositories have no Spring dependency  
    – Spring Data Repositories need no code!
    
-   Use Spring to configure JPA entity-manager factory
    
    – Smart proxy works with Spring-driven transactions
    
    – Optional translation to DataAccessExceptions (see advanced section)// ?1 replaced by method param
}
```
>
> - Spring implement your repository at run-time
> - auto-generate CRUD
> - paging, custom queires, sorting supported
> - sub-proj are varied
- Spring Data annotation equvalent to JPA
```java 
@Document class -- map to a JSON document 
@Region class -- Gemfire – map to a region
@NodeEntity class
@GraphId Long id; -- Neo4J – map to a graph
```
-   Templates (like JdbcTemplate) for basic CRUD access
`MongoTemplate, GemfireTemplate, RedisTemplate`


