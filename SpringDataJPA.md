# JPA

## JPA concepts
- **entity manager**
	- manage persistent objects in **PersistentContext**
	- Lifecyle bounds to transaction
- **entity manager factory**
	- thread-safe, shareable object represents a single **data source**/persistence unit
	- provides the access to new EntityManager
## Entity manager work flow
>**EntityMangerFactory** --creates--> 
>**EntityManager**--maps--> (application context)
>**List<*Person*> instances** --to--> 
>**Person Entities** --> persist to --> (persistence context)
>**database**
## EntityManager API
| method |persistence context  |SQL
|--|--|--
| persist(Object o) | persist entity to *   |insert into table
|remove(Object o)|remove entity from *| delete from table
|find(Class entity, Object pk)|find entity by pk|select * from table where id=?
|query createQuery(String jpql)|create a JPQL query|sql
|flush()|force entity be writtern to db immediately|..
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
### properties / getters - column
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
```java
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
...
```
>column cid is the FK in T_ADDRESS table, where in parent table T_CUSTOMER, the join column is cust_id the PK.
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

“select  c  from  Customer  c  where  c.address.city  =  :city”,  Customer.class); query.setParameter(“city”,  “Chicago”);  
List<Customer>  customers  =  query.getResultList();

//  ...  or  using  a  single  statement List<Customer>  customers2  =  entityManager.

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
### steps
#### define a EntityManagerFactory bean
>*LocalContainerEntityManagerFactoryBean class*
>
>Java config
```java
@Bean  
//FactoryBean is used
public LocalContainerEntityManagerFactoryBean entityManagerFactory(){

HibernateJpaVendorAdapter adapter = new HibernateJpaVendorAdapter(); adapter.setShowSql(true);  
adapter.setGenerateDdl(true);  
adapter.setDatabase(Database.HSQL);

Properties props = new Properties(); props.setProperty("hibernate.format_sql", "true");

LocalContainerEntityManagerFactoryBean emfb =  
new LocalContainerEntityManagerFactoryBean();

emfb.setDataSource(dataSource); 
//no persistence.xml needed now
emfb.setPackagesToScan("rewards.internal"); emfb.setJpaProperties(props); emfb.setJpaVendorAdapter(adapter);

return emfb; }
```
>XML config
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

transactionManager(EntityManagerFactory emf) { return new JpaTransactionManager(emf);
//JtaTransactionManager

}

@Bean  
public DataSource dataSource() { // Lookup via JNDI or create locally. }
```
#### define  mapping metadata
#### define DAOs / *Repository
>no spring involved in DAOs
```java
public class JpaCustomerRepository implements CustomerRepository { private EntityManager entityManager;
//inject EntityManager with TransactionManager proxy
@PersistenceContext
//JPA equivalent to @Autowired in Spring
public void setEntityManager (EntityManager entityManager) { this. entityManager = entityManager;

}

public Customer findById(long orderId) {  
return entityManager.find(Customer.class, orderId);
//when need to use EM, proxy resolves to EM
} }
```
> define repository bean
```java
@Bean  
public CustomerRepository jpaCustomerRepository() {
//entitymanager is injected by @PersistenceContext
	return new JpaCustomerRepository(); 
}
```
### configuration relationships

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
### annotate domain classes
>define keys , enable persistence
> Spring scans for Repository interfaces
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

public T findOne(ID id); public Iterable<T> findAll();

public void delete(ID id); public void delete(T entity); public void deleteAll();

}

PagingAndSortingRepository<T, K>  {
	Iterable<T> findAll(Sort);  
	Page<T> findAll(Pageable);
}
```
### define JPA repository methods
>Auto-generated finders obey naming convention  
> - find(First)By"DataMember""Op"
> - "Op" can be GreaterThan, NotEquals, Between, Like ...
```java
public interface CustomerRepository  
extends CrudRepository<Customer, Long> {

public Customer findFirstByEmail(String someEmail); // No <Op> for Equals public List<Customer> findByOrderDateLessThan(Date someDate);  
public List<Customer> findByOrderDateBetween(Date d1, Date d2);

@Query(“SELECT c FROM Customer c WHERE c.email NOT LIKE '%@%'”)

public List<Customer> findInvalidEmails(); 
<S extends Customer> save(S entity); // Definition as per CrudRepository

Customer findOne(long i); // Definition as per CrudRepository Customer findFirstByEmailIgnoreCase(String email); // Case insensitive search

@Query("select u from Customer u where u.emailAddress = ?1")

Customer findByEmail(String email); // ?1 replaced by method param
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

