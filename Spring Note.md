---


---

<h1 id="data-management">Data Management</h1>
<ul>
<li>Data Management</li>
<li>Spring JDBC</li>
<li>Transaction</li>
</ul>
<h2 id="the-role-of-spring-in-enterprise-data-access">The Role of Spring in Enterprise Data Access</h2>
<ul>
<li>
<p>Steps Required<br>
– Access a data source and establish a connection – Begin a transaction<br>
– Do the work – execute business logic<br>
– Commit or rollback the transaction<br>
– Close the connection</p>
</li>
<li>
<p>Spring Advantages</p>
<ul>
<li>No code to implement (classic cross-cutting concern)</li>
<li>No connection or session leakage</li>
<li>Throws own exceptions, independent of underlying API</li>
<li>Transaction management behavior is added around your code</li>
</ul>
</li>
<li>
<p>Declarative <strong>Transaction Management</strong> by <strong>@Transactional</strong></p>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">TransferServiceImpl</span> <span class="token keyword">implements</span> <span class="token class-name">TransferService</span> <span class="token punctuation">{</span> 
<span class="token annotation punctuation">@Transactional</span> <span class="token comment">// marks method as needing a txn  </span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">transfer</span><span class="token punctuation">(</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> <span class="token comment">// your application logic }</span>

<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong>Template</strong> Design Pattern
<ul>
<li>JdbcTemplate</li>
<li>JmsTemplate</li>
<li>RestTemplate</li>
<li>get data source manually: **DataSourceUtils **</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">  DataSourceUtils<span class="token punctuation">.</span><span class="token function">getConnection</span><span class="token punctuation">(</span>dataSource<span class="token punctuation">)</span>
</code></pre>
</li>
<li>Data Access in a Layered Architecture
<ul>
<li>Service Layer (or application layer)</li>
<li>Data access Layer</li>
<li>Infrastructure Layer</li>
</ul>
</li>
</ul>
<h2 id="springs-dataaccessexceptionhierarchy">Spring’s DataAccessExceptionHierarchy</h2>
<ul>
<li>hides JPA/Hibernate/JDBC</li>
<li>RuntimeException</li>
<li><strong>DataAccessException</strong></li>
</ul>
<p>B[DataAccessException] --&gt; A[RuntimeException]<br>
C[DataAccessResource FailureException] --&gt; B<br>
D[DataIntegrity ViolationException] --&gt; B<br>
E[CleanupFailure DataAccessException] --&gt; B<br>
F[OptimisticLocking FailureException] --&gt; B</p>
<h2 id="using-test-databases">Using Test Databases</h2>
<ul>
<li><strong>EmbeddedDatabaseBuilder</strong> i.e. HSQL/H2/Derby</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Bean</span>
<span class="token keyword">public</span> DataSource <span class="token function">dataSource</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
EmbeddedDatabaseBuilder builder <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">EmbeddedDatabaseBuilder</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> builder
<span class="token punctuation">.</span><span class="token function">setName</span><span class="token punctuation">(</span><span class="token string">"testdb"</span><span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">setType</span><span class="token punctuation">(</span>EmbeddedDatabaseType<span class="token punctuation">.</span>HSQL<span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span><span class="token string">"classpath:/testdb/schema.db"</span><span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span><span class="token string">"classpath:/testdb/test-data.db"</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">build</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>JDBC Namespace Equivalent</li>
</ul>
<ol>
<li><code>&lt;jdbc:embedded-database id=“dataSource” type=“H2”&gt;</code></li>
<li><code>&lt;jdbc:initialize-database data-source=“dataSource”&gt;</code></li>
</ol>
<blockquote>
<p>embedded database</p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>bean</span> <span class="token attr-name">class</span><span class="token attr-value"><span class="token punctuation">=</span>“example.order.JdbcOrderRepository”</span> <span class="token attr-name">\</span><span class="token punctuation">&gt;</span></span> <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>property</span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”</span> <span class="token attr-name">ref</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”</span> <span class="token punctuation">/&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>bean</span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">jdbc:</span>embedded-database</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”</span> <span class="token attr-name">type</span><span class="token attr-value"><span class="token punctuation">=</span>“H2”\</span><span class="token punctuation">&gt;</span></span> &lt;jdbc:script location=“classpath:schema.sql” /\&gt; <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">jdbc:</span>script</span> <span class="token attr-name">location</span><span class="token attr-value"><span class="token punctuation">=</span>“classpath:test-data.sql”</span> <span class="token punctuation">/&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span><span class="token namespace">jdbc:</span>embedded-database</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<blockquote>
<p>other data source</p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>bean</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”</span> <span class="token attr-name">class</span><span class="token attr-value"><span class="token punctuation">=</span>“org.apache.commons.dbcp.BasicDataSource”\</span><span class="token punctuation">&gt;</span></span> <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>property</span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span>“url”</span> <span class="token attr-name">value</span><span class="token attr-value"><span class="token punctuation">=</span>“${dataSource.url}”</span> <span class="token punctuation">/&gt;</span></span>  
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>property</span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span>“username”</span> <span class="token attr-name">value</span><span class="token attr-value"><span class="token punctuation">=</span>“${dataSource.username}”</span> <span class="token punctuation">/&gt;</span></span>  
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>property</span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span>“password”</span> <span class="token attr-name">value</span><span class="token attr-value"><span class="token punctuation">=</span>“${dataSource.password}”</span> <span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>bean</span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">jdbc:</span>initialize-database</span> <span class="token attr-name">data-source</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”\</span><span class="token punctuation">&gt;</span></span> &lt;jdbc:script location=“classpath:schema.sql” /\&gt; <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">jdbc:</span>script</span> <span class="token attr-name">location</span><span class="token attr-value"><span class="token punctuation">=</span>“classpath:test-data.sql”</span> <span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span><span class="token namespace">jdbc:</span>initialize-database</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<ul>
<li>Java configuration on initializing an existing test DB</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Configuration</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">DatabaseInitializer</span> <span class="token punctuation">{</span>
<span class="token annotation punctuation">@Value</span><span class="token punctuation">(</span><span class="token string">"classpath:schema.sql"</span><span class="token punctuation">)</span> <span class="token keyword">private</span> Resource schemaScript<span class="token punctuation">;</span>
<span class="token annotation punctuation">@Value</span><span class="token punctuation">(</span><span class="token string">"classpath:test-data.sql"</span><span class="token punctuation">)</span> <span class="token keyword">private</span> Resource dataScript<span class="token punctuation">;</span>

<span class="token keyword">private</span> DatabasePopulator <span class="token function">databasePopulator</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token keyword">final</span> ResourceDatabasePopulator populator <span class="token operator">=</span>
<span class="token keyword">new</span> <span class="token class-name">ResourceDatabasePopulator</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
populator<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span>schemaScript<span class="token punctuation">)</span><span class="token punctuation">;</span>
populator<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span>dataScript<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> populator<span class="token punctuation">;</span>

<span class="token annotation punctuation">@Bean</span>
<span class="token keyword">public</span> DataSourceInitializer <span class="token function">anyName</span><span class="token punctuation">(</span><span class="token keyword">final</span> DataSource dataSource<span class="token punctuation">)</span> <span class="token punctuation">{</span> 
<span class="token keyword">final</span> DataSourceInitializer initializer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">DataSourceInitializer</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
initializer<span class="token punctuation">.</span><span class="token function">setDataSource</span><span class="token punctuation">(</span>dataSource<span class="token punctuation">)</span><span class="token punctuation">;</span> 
initializer<span class="token punctuation">.</span><span class="token function">setDatabasePopulator</span><span class="token punctuation">(</span><span class="token function">databasePopulator</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> initializer<span class="token punctuation">;</span> <span class="token punctuation">}</span>

<span class="token operator">&gt;</span>a key<span class="token operator">-</span>value store <span class="token operator">=</span> Map
<span class="token operator">&gt;</span> Where <span class="token keyword">do</span> we use <span class="token keyword">this</span> caching
– Any method that always returns the same result <span class="token keyword">for</span> the same <span class="token function">argument</span><span class="token punctuation">(</span>s<span class="token punctuation">)</span>
i<span class="token punctuation">.</span>e<span class="token punctuation">.</span> Calculate data on the fly<span class="token punctuation">,</span> Execute a database query<span class="token punctuation">,</span> Request data via RMI<span class="token punctuation">,</span> JMS<span class="token punctuation">,</span> a web<span class="token operator">-</span>service <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span class="token operator">-</span> AOP and FactoryBean
	<span class="token operator">-</span> Mark methods cacheable<span class="token punctuation">.</span>  fetch data from cache using key<span class="token punctuation">,</span> method not executed
	<span class="token operator">-</span> caching <span class="token function">key</span><span class="token punctuation">(</span>s<span class="token punctuation">)</span>
	<span class="token operator">-</span> Name of cache to <span class="token function">use</span> <span class="token punctuation">(</span>multiple caches supported<span class="token punctuation">)</span> 
	<span class="token operator">-</span> Define one or more caches in Spring configuration
<span class="token operator">&gt;</span>annotation is used on your own code 
```java
<span class="token annotation punctuation">@Cacheable</span><span class="token punctuation">(</span>value<span class="token operator">=</span><span class="token string">"topBooks"</span><span class="token punctuation">,</span> key<span class="token operator">=</span><span class="token string">"#refId.toUpperCase()"</span><span class="token punctuation">,</span> condition<span class="token operator">=</span><span class="token string">"#title.length&lt;32"</span><span class="token punctuation">)</span> 
<span class="token keyword">public</span> Book <span class="token function">findBook</span><span class="token punctuation">(</span>String refId<span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">}</span>
<span class="token comment">//clear cache before method invoked</span>
<span class="token annotation punctuation">@CacheEvict</span><span class="token punctuation">(</span>value<span class="token operator">=</span><span class="token string">"topBooks"</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">loadBooks</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span> <span class="token punctuation">}</span>
<span class="token comment">//EnableCaching</span>
<span class="token annotation punctuation">@Configuration</span> <span class="token annotation punctuation">@EnableCaching</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">MyConfig</span> <span class="token punctuation">{</span>
	<span class="token annotation punctuation">@Bean</span> <span class="token keyword">public</span> BookService <span class="token function">bookService</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span> <span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>xml enable caching</p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>annotation-driven</span> <span class="token punctuation">/&gt;</span></span>  
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>bean</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>bookService<span class="token punctuation">"</span></span> <span class="token attr-name">class</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>example.BookService<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
</code></pre>
<blockquote>
<p>3rd party code</p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>bean</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>bookService<span class="token punctuation">"</span></span> <span class="token attr-name">class</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>example.BookService<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span> 
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">aop:</span>config</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">aop:</span>advisor</span> <span class="token attr-name">advice-ref</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>bookCache<span class="token punctuation">"</span></span> <span class="token attr-name">pointcut</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>execution(* *..BookService.*(..))<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span><span class="token namespace">aop:</span>config</span><span class="token punctuation">&gt;</span></span>  
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>advice</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>bookCache<span class="token punctuation">"</span></span> <span class="token attr-name">cache-manager</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>cacheManager<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>caching</span> <span class="token attr-name">cache</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>topBooks<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>  
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>cacheable</span> <span class="token attr-name">method</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>findBook<span class="token punctuation">"</span></span> <span class="token attr-name">key</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>#refId<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span> 
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>cache-evict</span> <span class="token attr-name">method</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>loadBooks<span class="token punctuation">"</span></span> <span class="token attr-name">all-entries</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>true<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">cache:</span>caching</span><span class="token punctuation">&gt;</span></span> 
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span><span class="token namespace">cache:</span>advice</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">//ConcurrentHashMap</span>
<span class="token annotation punctuation">@Bean</span>
<span class="token keyword">public</span> CacheManager <span class="token function">cacheManager</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
	SimpleCacheManager cacheManager <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">SimpleCacheManager</span><span class="token punctuation">(</span><span class="token string">"topAuthors"</span><span class="token punctuation">,</span> <span class="token string">"topBooks"</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token keyword">return</span> cacheManager<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
```<span class="token punctuation">}</span>
</code></pre>
<h2 id="implementing-caching">Implementing Caching</h2>
<h2 id="nosql-databases">NoSQL databases</h2>
<blockquote>
<p>Non-tabular data</p>
</blockquote>
<h2 id="summary">Summary</h2>
<ul>
<li>Enables layered architecture principles
<ul>
<li>Higher layers should not know about data management<br>
below</li>
</ul>
</li>
<li>Isolate via Data Access Exceptions
<ul>
<li>Hierarchy makes them easier to handle</li>
</ul>
</li>
<li>Provides consistent transaction management</li>
<li>Supports most leading data-access technologies
<ul>
<li>Relational and non-relational (NoSQL)</li>
</ul>
</li>
<li>A key component of the core Spring libraries</li>
<li>Automatic caching facility</li>
</ul>
<hr>
<h1 id="spring-jdbc">Spring JDBC</h1>
<h2 id="spring’s-jdbctemplate">Spring’s JdbcTemplate</h2>
<ol>
<li>implementing JDBC based repository</li>
</ol>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">JdbcCustomerRepository</span> <span class="token keyword">implements</span> <span class="token class-name">CustomerRepository</span> <span class="token punctuation">{</span>
	<span class="token comment">//create jdbcTemplate with data souce</span>
	<span class="token keyword">private</span> JdbcTemplate jdbcTemplate<span class="token punctuation">;</span>
	<span class="token keyword">public</span> <span class="token function">JdbcCustomerRepository</span><span class="token punctuation">(</span>DataSource dataSource<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">this</span><span class="token punctuation">.</span>jdbcTemplate <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">JdbcTemplate</span><span class="token punctuation">(</span>dataSource<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
	<span class="token comment">//apply queries</span>
	<span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">getCustomerCount</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		String sql <span class="token operator">=</span> <span class="token string">"select count(*) from customer"</span><span class="token punctuation">;</span>
		<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForObject</span><span class="token punctuation">(</span>sql<span class="token punctuation">,</span> Integer<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ol start="2">
<li>simple queries execution</li>
</ol>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">//1 Query with no bind variables</span>
jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForObject</span><span class="token punctuation">(</span>sql<span class="token punctuation">,</span> Date<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token comment">//2 Query With Bind Variables</span>
<span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">getCountOfNationalsOver</span><span class="token punctuation">(</span>Nationality nationality<span class="token punctuation">,</span> <span class="token keyword">int</span> age<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	String sql <span class="token operator">=</span> <span class="token string">"select count(*) from PERSON "</span> <span class="token operator">+</span>
	<span class="token string">"where age &gt; ? and nationality = ?"</span><span class="token punctuation">;</span>
	jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForObject</span>
	<span class="token punctuation">(</span>sql<span class="token punctuation">,</span> Integer<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">,</span> age<span class="token punctuation">,</span> nationality<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token comment">//3 Query for a single row</span>
<span class="token keyword">public</span> Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span>Object<span class="token operator">&gt;</span> <span class="token function">getPersonInfo</span><span class="token punctuation">(</span><span class="token keyword">int</span> id<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	String sql <span class="token operator">=</span> <span class="token string">"select * from PERSON where id=?"</span><span class="token punctuation">;</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForMap</span><span class="token punctuation">(</span>sql<span class="token punctuation">,</span> id<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token comment">//4 Query for multiple rows</span>
<span class="token keyword">public</span> List<span class="token operator">&lt;</span>Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span>Object<span class="token operator">&gt;&gt;</span> <span class="token function">getAllPersonInfo</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	String sql <span class="token operator">=</span> <span class="token string">"select * from PERSON"</span><span class="token punctuation">;</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForList</span><span class="token punctuation">(</span>sql<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ol start="3">
<li>Working with result sets</li>
</ol>
<ul>
<li>map relational data into domain objects (ResultSet to Account)</li>
<li>ORM alternative</li>
<li>implements <strong><code>RowMapper&lt;T&gt;</code></strong></li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> Person <span class="token function">getPerson</span><span class="token punctuation">(</span><span class="token keyword">int</span> id<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForObject</span><span class="token punctuation">(</span>
	<span class="token string">"select first_name, last_name from PERSON where id=?"</span><span class="token punctuation">,</span>
	<span class="token keyword">new</span> <span class="token class-name">PersonMapper</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> id<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token keyword">class</span> <span class="token class-name">PersonMapper</span> <span class="token keyword">implements</span> <span class="token class-name">RowMapper</span><span class="token operator">&lt;</span>Person<span class="token operator">&gt;</span> <span class="token punctuation">{</span>
	<span class="token keyword">public</span> Person <span class="token function">mapRow</span><span class="token punctuation">(</span>ResultSet rs<span class="token punctuation">,</span> <span class="token keyword">int</span> rowNum<span class="token punctuation">)</span> <span class="token keyword">throws</span> SQLException<span class="token punctuation">{</span>
		<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Person</span><span class="token punctuation">(</span>rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"first_name"</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
		rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"last_name"</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token keyword">public</span> List<span class="token operator">&lt;</span>Person<span class="token operator">&gt;</span> <span class="token function">getAllPersons</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">query</span><span class="token punctuation">(</span>
	<span class="token string">"select first_name, last_name from PERSON"</span><span class="token punctuation">,</span>
	<span class="token keyword">new</span> <span class="token class-name">PersonMapper</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">public</span> List<span class="token operator">&lt;</span>Person<span class="token operator">&gt;</span> <span class="token function">getAllPersons</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">query</span><span class="token punctuation">(</span>
		<span class="token string">"select first_name, last_name from PERSON"</span><span class="token punctuation">,</span>
		<span class="token punctuation">(</span>rs<span class="token punctuation">,</span> rowNum<span class="token punctuation">)</span> <span class="token operator">-</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span>
		<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Person</span><span class="token punctuation">(</span>rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"first_name"</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
		rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"last_name"</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>implements <strong>RowCallbackHandler</strong>
<ul>
<li>no return object</li>
<li>streaming rows to a file, converting rows to XML, filtering rows (SQL filtering is better), faster than JPA equivalent</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">JdbcOrderRepository</span> <span class="token punctuation">{</span>
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">generateReport</span><span class="token punctuation">(</span><span class="token keyword">final</span> PrintWriter out<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// select all orders of year 2009 for a full report</span>
		jdbcTemplate<span class="token punctuation">.</span><span class="token function">query</span><span class="token punctuation">(</span><span class="token string">"select * from order where year=?"</span><span class="token punctuation">,</span>
			<span class="token punctuation">(</span>RowCallbackHandler<span class="token punctuation">)</span><span class="token punctuation">(</span>rs<span class="token punctuation">)</span> <span class="token operator">-</span><span class="token operator">&gt;</span>
			<span class="token punctuation">{</span> out<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span> rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"customer"</span><span class="token punctuation">)</span> … <span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
			<span class="token number">2016</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
<span class="token keyword">public</span> <span class="token keyword">interface</span> <span class="token class-name">RowCallbackHandler</span> <span class="token punctuation">{</span>
	<span class="token keyword">void</span> <span class="token function">processRow</span><span class="token punctuation">(</span>ResultSet rs<span class="token punctuation">)</span> <span class="token keyword">throws</span> SQLException<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong>ResultSetExtractor</strong>
<ul>
<li>processing an entire ResultSet at once to a single object</li>
<li>You are responsible for iterating the ResultSet</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">JdbcOrderRepository</span> <span class="token punctuation">{</span>
	<span class="token keyword">public</span> Order <span class="token function">findByConfirmationNumber</span><span class="token punctuation">(</span>String number<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// execute an outer join between order and item tables</span>
		<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">query</span><span class="token punctuation">(</span>
		<span class="token string">"select...from order o, item i...conf_id = ?"</span><span class="token punctuation">,</span>
		<span class="token punctuation">(</span>ResultSetExtractor<span class="token operator">&lt;</span>Order<span class="token operator">&gt;</span><span class="token punctuation">)</span><span class="token punctuation">(</span>rs<span class="token punctuation">)</span> <span class="token operator">-</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span>
			Order order <span class="token operator">=</span> null<span class="token punctuation">;</span>
			<span class="token keyword">while</span> <span class="token punctuation">(</span>rs<span class="token punctuation">.</span><span class="token function">next</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			<span class="token keyword">if</span> <span class="token punctuation">(</span>order <span class="token operator">==</span> null<span class="token punctuation">)</span>
			order <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Order</span><span class="token punctuation">(</span>rs<span class="token punctuation">.</span><span class="token function">getLong</span><span class="token punctuation">(</span><span class="token string">"ID"</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
							  rs<span class="token punctuation">.</span><span class="token function">getString</span><span class="token punctuation">(</span><span class="token string">"NAME"</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			order<span class="token punctuation">.</span><span class="token function">addItem</span><span class="token punctuation">(</span><span class="token function">mapItem</span><span class="token punctuation">(</span>rs<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span>
			<span class="token keyword">return</span> order<span class="token punctuation">;</span>
		<span class="token punctuation">}</span><span class="token punctuation">,</span>
		number<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>Summary of Callback Interfaces<br>
• <strong>RowMapper</strong><br>
– Best choice when each row of a ResultSet maps to a<br>
domain object<br>
• <strong>RowCallbackHandler</strong><br>
– Best choice when no value should be returned from the<br>
callback method for each row, especially large queries<br>
• <strong>ResultSetExtractor</strong><br>
– Best choice when multiple rows of a ResultSet map to a<br>
single object</li>
</ul>
<hr>
<ul>
<li><strong>jdbcTemplate.update()</strong></li>
</ul>
<blockquote>
<p>Inserting a new row  – Returns number of rows modified</p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">insertPerson</span><span class="token punctuation">(</span>Person person<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">update</span><span class="token punctuation">(</span>
	“insert into <span class="token function">PERSON</span> <span class="token punctuation">(</span>first_name<span class="token punctuation">,</span> last_name<span class="token punctuation">,</span> age<span class="token punctuation">)</span>” <span class="token operator">+</span>
	“<span class="token function">values</span> <span class="token punctuation">(</span><span class="token operator">?</span><span class="token punctuation">,</span> <span class="token operator">?</span><span class="token punctuation">,</span> <span class="token operator">?</span><span class="token punctuation">)</span>”<span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getFirstName</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getLastName</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getAge</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>Updating an existing row</p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">updateAge</span><span class="token punctuation">(</span>Person person<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">update</span><span class="token punctuation">(</span>
	“update PERSON set age<span class="token operator">=</span><span class="token operator">?</span> where id<span class="token operator">=</span><span class="token operator">?</span>”<span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getAge</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getId</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h2 id="exception-handling">Exception handling</h2>
<blockquote>
<p>The <em>JdbcTemplate</em> transforms <strong>SQLExceptions</strong> into<br>
<strong>DataAccessExceptions</strong></p>
</blockquote>
<h2 id="summary-1">Summary</h2>
<p>• JDBC is useful<br>
– But using JDBC API directly is tedious and error-prone<br>
• JdbcTemplate simplifies data access and enforces<br>
consistency<br>
– DRY principle hides most of the JDBC<br>
– Many options for reading data<br>
• SQLExceptions typically cannot be handled where<br>
thrown<br>
– Should not be checked Exceptions<br>
– Spring provides DataAccessException instead</p>
<hr>
<h1 id="transaction">Transaction</h1>

<table>
<thead>
<tr>
<th>ACID</th>
<th>Set of tasks take place as a single transaction</th>
</tr>
</thead>
<tbody>
<tr>
<td>atomic</td>
<td>unit of work is <strong>all-or-nothing</strong> operation</td>
</tr>
<tr>
<td>consistent</td>
<td>db <strong>interity constaints</strong> are never voilated</td>
</tr>
<tr>
<td>isolated</td>
<td><strong>isolating transactions</strong> from each other</td>
</tr>
<tr>
<td>durable</td>
<td><strong>committed</strong> changes are permanent</td>
</tr>
</tbody>
</table><h2 id="java-transaction-management">Java Transaction Management</h2>
<ul>
<li>jdbc, jms, jta, hibernate, jpa handle transaction differently and programatically in repository layer</li>
<li>local transaction - single resource</li>
<li>global/distributed transactions - separate transaction manager</li>
</ul>
<h2 id="spring-transaction-management">Spring Transaction Management</h2>
<ul>
<li>Demarcation expressed declaratively via AOP</li>
<li>interface <strong>PlatformTransactionManager</strong> implementations
<ul>
<li>DataSourceTransactionManager</li>
<li>HibernateTransactionManager</li>
<li>JpaTransactionManager</li>
<li>…</li>
</ul>
</li>
<li>global and local use the same API</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkImpl</span> <span class="token keyword">implements</span> <span class="token class-name">RewardNetwork</span> <span class="token punctuation">{</span>
	<span class="token annotation punctuation">@Transactional</span>
	<span class="token keyword">public</span> RewardConfirmation <span class="token function">rewardAccountFor</span><span class="token punctuation">(</span>Dining d<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// atomic unit-of-work</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
<span class="token annotation punctuation">@Configuration</span>
<span class="token annotation punctuation">@EnableTransactionManagement</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">TxnConfig</span> <span class="token punctuation">{</span>
<span class="token annotation punctuation">@Bean</span>
<span class="token keyword">public</span> PlatformTransactionManager <span class="token function">transactionManager</span><span class="token punctuation">(</span>DataSource ds<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">DataSourceTransactionManager</span><span class="token punctuation">(</span>ds<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token comment">//or</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>annotation<span class="token operator">-</span>driven<span class="token operator">/</span><span class="token operator">&gt;</span>

<span class="token operator">&lt;</span>bean id<span class="token operator">=</span>“transactionManager” 
<span class="token keyword">class</span><span class="token operator">=</span>”org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>jdbc<span class="token punctuation">.</span>datasource<span class="token punctuation">.</span>DataSourceTransactionManager”<span class="token operator">&gt;</span>
	<span class="token operator">&lt;</span>property name<span class="token operator">=</span>“dataSource” ref<span class="token operator">=</span>“dataSource”<span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>bean<span class="token operator">&gt;</span>
</code></pre>
<ul>
<li>Target object wrapped in a proxy (Around advice)</li>
</ul>
<p>[spring proxy ]–proxy wraps target object<br>
–&gt;((RewardNetworkImpl))</p>
<ul>
<li>@Transactional
<ul>
<li>class level</li>
<li>method level</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>timeout<span class="token operator">=</span><span class="token number">60</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkImpl</span> <span class="token keyword">implements</span> <span class="token class-name">RewardNetwork</span> <span class="token punctuation">{</span>
	<span class="token keyword">public</span> RewardConfirmation <span class="token function">rewardAccountFor</span><span class="token punctuation">(</span>Dining d<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// atomic unit-of-work</span>
	<span class="token punctuation">}</span>
<span class="token comment">//override attributes at method level</span>
<span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>timeout<span class="token operator">=</span><span class="token number">45</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> RewardConfirmation <span class="token function">updateConfirmation</span><span class="token punctuation">(</span>RewardConfirmantion rc<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// atomic unit-of-work</span>
<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="transaction-config-local--global">Transaction config Local &amp; Global</h3>
<ul>
<li>Local JDBC config
<ul>
<li>Define local data source</li>
<li>config DataSourceTransactionManager</li>
<li>Purpose:
<ul>
<li>Integration testing;</li>
<li>Deploy to Tomcat or other servlet container</li>
</ul>
</li>
</ul>
</li>
<li>Global Java EE config
<ul>
<li>Use container-managed datasource (JNDI)</li>
<li>JTA Transaction Manager</li>
<li>Purpose:
<ul>
<li>Deploy to JEE container</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
<p>No code changes Just configuration</p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token comment">&lt;!--local--&gt;</span>
&lt;bean id="transactionManager" class="...DataSourceTransactionManager"\&gt; ...
&lt;jdbc:embedded-database id="dataSource"\&gt; ...
<span class="token comment">&lt;!--global--&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">tx:</span>jta-transaction-manager</span><span class="token punctuation">/&gt;</span></span> 
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span><span class="token namespace">jee:</span>jndi-lookup</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span>“dataSource”</span> <span class="token attr-name">...</span> <span class="token punctuation">/&gt;</span></span>
</code></pre>
<h2 id="isolation-levels">Isolation Levels</h2>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkImpl</span> <span class="token keyword">implements</span> <span class="token class-name">RewardNetwork</span> <span class="token punctuation">{</span>
	<span class="token annotation punctuation">@Transactional</span> <span class="token punctuation">(</span>isolation<span class="token operator">=</span>Isolation<span class="token punctuation">.</span>READ_UNCOMMITTED<span class="token punctuation">)</span>
	<span class="token keyword">public</span> BigDecimal <span class="token function">totalRewards</span><span class="token punctuation">(</span>String merchantNumber<span class="token punctuation">,</span> <span class="token keyword">int</span> year<span class="token punctuation">)</span>
	<span class="token comment">// Calculate total rewards for a restaurant for a whole year</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>

<table>
<thead>
<tr>
<th>isolation level</th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>READ_UNCOMMITTED</td>
<td>dirty reads</td>
<td>lowest level</td>
</tr>
<tr>
<td>READ_COMMITTED</td>
<td>no dirty reads(only see committed info)</td>
<td>default for most DB</td>
</tr>
<tr>
<td>REPEATABLE_READ</td>
<td>no dirty reads, non-repeatable reads prevented</td>
<td></td>
</tr>
<tr>
<td>SERIALIZABLE</td>
<td>non-repeatable reads prevented, no phantom reads</td>
<td>highest level</td>
</tr>
</tbody>
</table><ul>
<li>dirty reads : see the results of an uncommited unit of work</li>
<li>non-repeatable reads : a row is read twice in the same transaction, results are different</li>
<li>phantom reads: All the rows in the query have the same value before and after, but for the same query different result is queried i.e. different rows are selected. cus the transaction is not concurrent.<br>
ropagation</li>
</ul>
<blockquote>
<p>ClientServiceImpl calls AccountServiceImpl, both of them are transactional?<br>
Should everything run into a single transaction? – Should each service have its own transaction?</p>
</blockquote>
<ul>
<li>7 levels of p## Transaction Propagation</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span> propagation<span class="token operator">=</span>Propagation<span class="token punctuation">.</span>REQUIRES_NEW <span class="token punctuation">)</span>
</code></pre>
<p>| Propagation | there is a current tx |there is not a current tx</p>

<table>
<thead>
<tr>
<th>–meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td>REQUIRED</td>
</tr>
<tr>
<td>REQUIRES_NEW</td>
</tr>
<tr>
<td>MANDATORY</td>
</tr>
<tr>
<td>NEVER</td>
</tr>
<tr>
<td>NOT_SUPPORTED</td>
</tr>
<tr>
<td>SUPPORTS</td>
</tr>
<tr>
<td>NESTED</td>
</tr>
</tbody>
</table><h2 id="rollback-rules">Rollback rules</h2>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>rollbackFor<span class="token operator">=</span>MyCheckedException<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">,</span>
noRollbackFor<span class="token operator">=</span><span class="token punctuation">{</span>JmxException<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">,</span> MailException<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
</code></pre>
<ul>
<li>By default, a transaction is rolled back if a RuntimeException (DataAccessException, any runtimeException) has been thrown</li>
</ul>
<h2 id="testing">Testing</h2>
<ul>
<li>by default , <strong>@Transaction</strong> will be rolled back after testing</li>
<li>with <strong>@Commit</strong> , transaction will be committed</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@ContextConfiguration</span><span class="token punctuation">(</span>classes<span class="token operator">=</span>RewardsConfig<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span>
<span class="token annotation punctuation">@RunWith</span><span class="token punctuation">(</span>SpringJUnit4ClassRunner<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkTest</span> <span class="token punctuation">{</span>
	<span class="token annotation punctuation">@Test</span> <span class="token annotation punctuation">@Transactional</span> <span class="token comment">//@Commit</span>
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">testRewardAccountFor</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h2 id="advanced-topics">Advanced topics</h2>
<h3 id="xml-configuration">XML configuration</h3>
<pre class=" language-java"><code class="prism  language-java"><span class="token operator">&lt;</span>aop<span class="token operator">:</span>config<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>aop<span class="token operator">:</span>pointcut id<span class="token operator">=</span>“rewardNetworkMethods”
expression<span class="token operator">=</span>“<span class="token function">execution</span><span class="token punctuation">(</span><span class="token operator">*</span> rewards<span class="token punctuation">.</span>RewardNetwork<span class="token punctuation">.</span>*<span class="token punctuation">(</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">)</span><span class="token punctuation">)</span>”<span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>aop<span class="token operator">:</span>advisor pointcut<span class="token operator">-</span>ref<span class="token operator">=</span>“rewardNetworkMethods” advice<span class="token operator">-</span>ref<span class="token operator">=</span>“txAdvice”<span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>aop<span class="token operator">:</span>config<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>advice id<span class="token operator">=</span>“txAdvice”<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>attributes<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>method name<span class="token operator">=</span><span class="token string">"get*"</span> read<span class="token operator">-</span>only<span class="token operator">=</span><span class="token string">"true"</span> timeout<span class="token operator">=</span><span class="token string">"10"</span><span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>method name<span class="token operator">=</span><span class="token string">"find*"</span> read<span class="token operator">-</span>only<span class="token operator">=</span><span class="token string">"true"</span> timeout<span class="token operator">=</span><span class="token string">"10"</span><span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>method name<span class="token operator">=</span><span class="token string">"*"</span> timeout<span class="token operator">=</span><span class="token string">"30"</span><span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>tx<span class="token operator">:</span>attributes<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>tx<span class="token operator">:</span>advice<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>bean id<span class="token operator">=</span>“transactionManager”
<span class="token keyword">class</span><span class="token operator">=</span>“org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>jdbc<span class="token punctuation">.</span>datasource<span class="token punctuation">.</span>DataSourceTransactionManager”<span class="token operator">&gt;</span>
<span class="token operator">&lt;</span>property name<span class="token operator">=</span>“dataSource” ref<span class="token operator">=</span>“dataSource”<span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>bean<span class="token operator">&gt;</span>
</code></pre>
<h3 id="programmatic-transactions">Programmatic transactions</h3>
<ul>
<li>declarative transaction management is recommended, but programmatic transaction is enabled too.</li>
<li>use TransactionTemplate plus callback, working with JTA transaction manager or local</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> RewardConfirmation <span class="token function">rewardAccountFor</span><span class="token punctuation">(</span>Dining dining<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
	<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">TransactionTemplate</span><span class="token punctuation">(</span>txManager<span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span> <span class="token punctuation">(</span>status<span class="token punctuation">)</span> <span class="token operator">-</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span>
	<span class="token keyword">try</span> <span class="token punctuation">{</span>
	<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
		accountRepository<span class="token punctuation">.</span><span class="token function">updateBeneficiaries</span><span class="token punctuation">(</span>account<span class="token punctuation">)</span><span class="token punctuation">;</span>
		confirmation <span class="token operator">=</span> rewardRepository<span class="token punctuation">.</span><span class="token function">confirmReward</span><span class="token punctuation">(</span>contribution<span class="token punctuation">,</span> dining<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
	<span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">RewardException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		status<span class="token punctuation">.</span><span class="token function">setRollbackOnly</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		confirmation <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">RewardFailure</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
		<span class="token keyword">return</span> confirmation<span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
	<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="read-only-transactions">Read-only transactions</h3>
<ul>
<li>put multiple connections in one readOnly transaction</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>readOnly<span class="token operator">=</span><span class="token boolean">true</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">rewardAccount2</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForList</span><span class="token punctuation">(</span>…<span class="token punctuation">)</span><span class="token punctuation">;</span>
jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForInt</span><span class="token punctuation">(</span>…<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>With a high isolation level, a read-only transaction prevents<br>
data from being modified until the transaction commits</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>readOnly<span class="token operator">=</span><span class="token boolean">true</span><span class="token punctuation">,</span> isolation<span class="token operator">=</span>Isolation<span class="token punctuation">.</span>REPEATABLE_READ<span class="token punctuation">)</span>
</code></pre>
<h3 id="more-on-transactional-tests">More on Transactional Tests</h3>
<ul>
<li>@Before test runs within a transaction</li>
<li>@BeforeTransaction test runs before a transaction starts</li>
<li>@After test runs within a transaction</li>
<li>@AfterTransaction test runs after a transaction starts</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@ContextConfiguration</span><span class="token punctuation">(</span>locations<span class="token operator">=</span><span class="token punctuation">{</span><span class="token string">"/rewards-config.xml"</span><span class="token punctuation">}</span><span class="token punctuation">)</span>
<span class="token annotation punctuation">@RunWith</span><span class="token punctuation">(</span>SpringJUnit4ClassRunner<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkTest</span> <span class="token punctuation">{</span>
<span class="token annotation punctuation">@BeforeTransaction</span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">verifyInitialDatabaseState</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>…<span class="token punctuation">}</span>
<span class="token annotation punctuation">@Before</span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">setUpTestDataInTransaction</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>…<span class="token punctuation">}</span>
<span class="token annotation punctuation">@Test</span> <span class="token annotation punctuation">@Transactional</span>
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">testRewardAccountFor</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> … <span class="token punctuation">}</span>
</code></pre>
<ul>
<li></li>
</ul>
<h3 id="mutiple-transactions">Mutiple Transactions</h3>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span><span class="token string">"myOtherTransactionManager"</span><span class="token punctuation">)</span>
<span class="token annotation punctuation">@Transactional</span> <span class="token comment">//default transactionManager</span>
</code></pre>
<h3 id="propagation-options">Propagation Options</h3>

