---


---

<h1 id="data-management">Data Management</h1>
<h2 id="the-role-of-spring-in-enterprise-data-access">The Role of Spring in Enterprise Data Access</h2>
<ul>
<li>Declarative <strong>Transaction Management</strong> by @Transactional</li>
<li><strong>Template</strong> Design Pattern
<ul>
<li>JdbcTemplate</li>
<li>JmsTemplate</li>
<li>RestTemplate</li>
<li>get data source manually</li>
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
<li>DataAccessException</li>
</ul>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-7Z0QPvJT0O9RjjbS" height="100%" viewBox="0 0 1540.078125 317.5" style="max-width:1540.078125px;"><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M763.12109375,175L763.12109375,200L763.12109375,225" marker-end="url(#arrowhead1745)" style="fill:none"></path><defs><marker id="arrowhead1745" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M195.953125,72.5L195.953125,97.5L658.73828125,139.31783980164607" marker-end="url(#arrowhead1746)" style="fill:none"></path><defs><marker id="arrowhead1746" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M575.4453125,72.5L575.4453125,97.5L666.9944740853658,122.5" marker-end="url(#arrowhead1747)" style="fill:none"></path><defs><marker id="arrowhead1747" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M950.796875,72.5L950.796875,97.5L859.2477134146342,122.5" marker-end="url(#arrowhead1748)" style="fill:none"></path><defs><marker id="arrowhead1748" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M1336.34375,72.5L1336.34375,97.5L867.50390625,139.41746737537906" marker-end="url(#arrowhead1749)" style="fill:none"></path><defs><marker id="arrowhead1749" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" id="B" transform="translate(763.12109375,148.75)" style="opacity: 1;"><rect rx="0" ry="0" x="-104.3828125" y="-26.25" width="208.765625" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-94.3828125,-16.25)"><foreignObject width="188.76953125" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">DataAccessException</div></foreignObject></g></g></g><g class="node" id="A" transform="translate(763.12109375,251.25)" style="opacity: 1;"><rect rx="0" ry="0" x="-90.1171875" y="-26.25" width="180.234375" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-80.1171875,-16.25)"><foreignObject width="160.234375" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">RuntimeException</div></foreignObject></g></g></g><g class="node" id="C" transform="translate(195.953125,46.25)" style="opacity: 1;"><rect rx="0" ry="0" x="-175.953125" y="-26.25" width="351.90625" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-165.953125,-16.25)"><foreignObject width="331.9140625" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">DataAccessResource FailureException</div></foreignObject></g></g></g><g class="node" id="D" transform="translate(575.4453125,46.25)" style="opacity: 1;"><rect rx="0" ry="0" x="-153.5390625" y="-26.25" width="307.078125" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-143.5390625,-16.25)"><foreignObject width="287.08984375" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">DataIntegrity ViolationException</div></foreignObject></g></g></g><g class="node" id="E" transform="translate(950.796875,46.25)" style="opacity: 1;"><rect rx="0" ry="0" x="-171.8125" y="-26.25" width="343.625" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-161.8125,-16.25)"><foreignObject width="323.6328125" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">CleanupFailure DataAccessException</div></foreignObject></g></g></g><g class="node" id="F" transform="translate(1336.34375,46.25)" style="opacity: 1;"><rect rx="0" ry="0" x="-163.734375" y="-26.25" width="327.46875" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-153.734375,-16.25)"><foreignObject width="307.48046875" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">OptimisticLocking FailureException</div></foreignObject></g></g></g></g></g></g></svg></div>
<h2 id="using-test-databases">Using Test Databases</h2>
<ul>
<li>Embedded Database Builder i.e. HSQL/H2/Derby</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Bean</span>
<span class="token keyword">public</span> DataSource <span class="token function">dataSource</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
EmbeddedDatabaseBuilder builder <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">EmbeddedDatabaseBuilder</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> builder<span class="token punctuation">.</span><span class="token function">setName</span><span class="token punctuation">(</span><span class="token string">"testdb"</span><span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">setType</span><span class="token punctuation">(</span>EmbeddedDatabaseType<span class="token punctuation">.</span>HSQL<span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span><span class="token string">"classpath:/testdb/schema.db"</span><span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">addScript</span><span class="token punctuation">(</span><span class="token string">"classpath:/testdb/test-data.db"</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">build</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>JDBC Namespace Equivalent</li>
</ul>
<ol>
<li><code>&lt;jdbc:embedded-database&gt;</code></li>
<li><code>&lt;jdbc:initialize-database&gt;</code></li>
<li>Java configuration on initializing and existing test DB</li>
</ol>
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
<span class="token punctuation">}</span>
</code></pre>
<h2 id="implementing-caching">Implementing Caching</h2>
<h2 id="nosql-databases">NoSQL databases</h2>
<h2 id="summary">Summary</h2>
<p>– Enables layered architecture principles<br>
• Higher layers should not know about data management<br>
below<br>
– Isolate via Data Access Exceptions<br>
• Hierarchy makes them easier to handle<br>
– Provides consistent transaction management<br>
– Supports most leading data-access technologies<br>
• Relational and non-relational (NoSQL)<br>
– A key component of the core Spring libraries<br>
– Automatic caching facility</p>
<hr>
<h1 id="spring-jdbc">Spring JDBC</h1>
<h2 id="spring’s-jdbctemplate">Spring’s JdbcTemplate</h2>
<ol>
<li>implementing JDBC based repository</li>
</ol>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">JdbcCustomerRepository</span> <span class="token keyword">implements</span> <span class="token class-name">CustomerRepository</span> <span class="token punctuation">{</span>
<span class="token keyword">private</span> JdbcTemplate jdbcTemplate<span class="token punctuation">;</span>
<span class="token keyword">public</span> <span class="token function">JdbcCustomerRepository</span><span class="token punctuation">(</span>DataSource dataSource<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token keyword">this</span><span class="token punctuation">.</span>jdbcTemplate <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">JdbcTemplate</span><span class="token punctuation">(</span>dataSource<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">getCustomerCount</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
String sql <span class="token operator">=</span> <span class="token string">"select count(*) from customer"</span><span class="token punctuation">;</span>
<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">queryForObject</span><span class="token punctuation">(</span>sql<span class="token punctuation">,</span> Integer<span class="token punctuation">.</span><span class="token keyword">class</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ol start="2">
<li>Query execution</li>
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
<li>RowMapper</li>
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
<li>RowCallbackHandler
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
<li>ResultSetExtractor
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
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">insertPerson</span><span class="token punctuation">(</span>Person person<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">return</span> jdbcTemplate<span class="token punctuation">.</span><span class="token function">update</span><span class="token punctuation">(</span>
	“insert into <span class="token function">PERSON</span> <span class="token punctuation">(</span>first_name<span class="token punctuation">,</span> last_name<span class="token punctuation">,</span> age<span class="token punctuation">)</span>” <span class="token operator">+</span>
	“<span class="token function">values</span> <span class="token punctuation">(</span><span class="token operator">?</span><span class="token punctuation">,</span> <span class="token operator">?</span><span class="token punctuation">,</span> <span class="token operator">?</span><span class="token punctuation">)</span>”<span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getFirstName</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getLastName</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
	person<span class="token punctuation">.</span><span class="token function">getAge</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">public</span> <span class="token keyword">int</span> <span class="token function">updateAge</span><span class="token punctuation">(</span>Person person<span class="token punctuation">)</span> <span class="token punctuation">{</span>
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
<th>set of tasks take place as a single transaction</th>
</tr>
</thead>
<tbody>
<tr>
<td>atomic</td>
<td>unit of work is all-or-nothing operation</td>
</tr>
<tr>
<td>consistent</td>
<td>db interity constaints are never voilated</td>
</tr>
<tr>
<td>isolated</td>
<td>isolating transactions from each other</td>
</tr>
<tr>
<td>durable</td>
<td>committed changes are permanent</td>
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
<li><strong>PlatformTransactionManager</strong> implementations
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
<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">DataSourceTransactionManager</span><span class="token punctuation">(</span>ds<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>
<span class="token comment">//or</span>
<span class="token operator">&lt;</span>tx<span class="token operator">:</span>annotation<span class="token operator">-</span>driven<span class="token operator">/</span><span class="token operator">&gt;</span>

<span class="token operator">&lt;</span>bean id<span class="token operator">=</span>“transactionManager” <span class="token keyword">class</span><span class="token operator">=</span>”org<span class="token punctuation">.</span>springframework<span class="token punctuation">.</span>jdbc<span class="token punctuation">.</span>datasource<span class="token punctuation">.</span>DataSourceTransactionManager”<span class="token operator">&gt;</span>
	<span class="token operator">&lt;</span>property name<span class="token operator">=</span>“dataSource” ref<span class="token operator">=</span>“dataSource”<span class="token operator">/</span><span class="token operator">&gt;</span>
<span class="token operator">&lt;</span><span class="token operator">/</span>bean<span class="token operator">&gt;</span>
</code></pre>
<ul>
<li>Target object wrapped in a proxy (Around advice)</li>
</ul>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-NCYXMZyrpjQSWGOM" height="100%" viewBox="0 0 662.40625 260.625" style="max-width:662.40625px;"><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M147.859375,120.3125L284.8203125,120.3125L421.78125,120.3125" marker-end="url(#arrowhead1757)" style="fill:none"></path><defs><marker id="arrowhead1757" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(284.8203125,120.3125)" style="opacity: 1;"><g transform="translate(-111.9609375,-16.25)" class="label"><foreignObject width="223.92578125" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">proxy wraps target object</span></div></foreignObject></g></g></g><g class="nodes"><g class="node" id="A" transform="translate(83.9296875,120.3125)" style="opacity: 1;"><rect rx="0" ry="0" x="-63.9296875" y="-26.25" width="127.859375" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-53.9296875,-16.25)"><foreignObject width="107.87109375" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">spring proxy</div></foreignObject></g></g></g><g class="node" id="B" transform="translate(522.09375,120.3125)" style="opacity: 1;"><circle x="-100.3125" y="-26.25" r="100.3125"></circle><g class="label" transform="translate(0,0)"><g transform="translate(-90.3125,-16.25)"><foreignObject width="180.625" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">RewardNetworkImpl</div></foreignObject></g></g></g></g></g></g></svg></div>
<ul>
<li>@Transactional --class level and method level</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>timeout<span class="token operator">=</span><span class="token number">60</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">RewardNetworkImpl</span> <span class="token keyword">implements</span> <span class="token class-name">RewardNetwork</span> <span class="token punctuation">{</span>
<span class="token keyword">public</span> RewardConfirmation <span class="token function">rewardAccountFor</span><span class="token punctuation">(</span>Dining d<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token comment">// atomic unit-of-work</span>
<span class="token punctuation">}</span>
<span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span>timeout<span class="token operator">=</span><span class="token number">45</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> RewardConfirmation <span class="token function">updateConfirmation</span><span class="token punctuation">(</span>RewardConfirmantion rc<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token comment">// atomic unit-of-work</span>
<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
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
<td>no dirty reads</td>
<td>default for most db</td>
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
</ul>
<h2 id="transaction-propagation">Transaction Propagation</h2>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Transactional</span><span class="token punctuation">(</span> propagation<span class="token operator">=</span>Propagation<span class="token punctuation">.</span>REQUIRES_NEW <span class="token punctuation">)</span>
</code></pre>

<table>
<thead>
<tr>
<th>Propagation</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td>REQUIRED</td>
<td>Execute within a current transaction, create a new one if none exists</td>
</tr>
<tr>
<td>REQUIRES_NEW</td>
<td>Create a new transaction, suspending the current transaction if one exists</td>
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
<h2 id="propagation-options">(6) Propagation Options</h2>
<h2 id="uml-diagrams">UML diagrams</h2>
<p>You can render UML diagrams using <a href="https://mermaidjs.github.io/">Mermaid</a>. For example, this will produce a sequence diagram:</p>
<hr>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-STeovcOrrkFV0dZp" height="100%" width="100%" style="max-width:750px;" viewBox="-50 -10 750 483.59375"><g></g><g><line id="actor84" x1="75" y1="5" x2="75" y2="472.59375" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="0" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="75" dy="0">Alice</tspan></text></g><g><line id="actor85" x1="275" y1="5" x2="275" y2="472.59375" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="200" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="275" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="275" dy="0">Bob</tspan></text></g><g><line id="actor86" x1="475" y1="5" x2="475" y2="472.59375" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="400" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="475" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="475" dy="0">John</tspan></text></g><defs><marker id="arrowhead" refX="5" refY="2" markerWidth="6" markerHeight="4" orient="auto"><path d="M 0,0 V 4 L6,2 Z"></path></marker></defs><defs><marker id="crosshead" markerWidth="15" markerHeight="8" orient="auto" refX="16" refY="4"><path fill="black" stroke="#000000" stroke-width="1px" d="M 9,2 V 6 L16,4 Z" style="stroke-dasharray: 0, 0;"></path><path fill="none" stroke="#000000" stroke-width="1px" d="M 0,1 L 6,7 M 6,1 L 0,7" style="stroke-dasharray: 0, 0;"></path></marker></defs><g><text x="175" y="93" class="messageText" style="text-anchor: middle;">Hello Bob, how are you?</text><line x1="75" y1="100" x2="275" y2="100" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><text x="375" y="128" class="messageText" style="text-anchor: middle;">How about you John?</text><line x1="275" y1="135" x2="475" y2="135" class="messageLine1" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line></g><g><text x="175" y="163" class="messageText" style="text-anchor: middle;">I am good thanks!</text><line x1="275" y1="170" x2="75" y2="170" class="messageLine1" stroke-width="2" stroke="black" marker-end="url(#crosshead)" style="stroke-dasharray: 3, 3; fill: none;"></line></g><g><text x="375" y="198" class="messageText" style="text-anchor: middle;">I am good thanks!</text><line x1="275" y1="205" x2="475" y2="205" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#crosshead)" style="fill: none;"></line></g><g><rect x="500" y="215" fill="#EDF2AE" stroke="#666" width="150" height="102.59375" rx="0" ry="0" class="note"></rect><text x="516" y="245" fill="black" class="noteText"><tspan x="516">Bob thinks a long</tspan><tspan dy="23" x="516">long time, so long</tspan><tspan dy="23" x="516">that the text does</tspan><tspan dy="23" x="516">not fit on a row.</tspan></text></g><g><text x="175" y="345.59375" class="messageText" style="text-anchor: middle;">Checking with John...</text><line x1="275" y1="352.59375" x2="75" y2="352.59375" class="messageLine1" stroke-width="2" stroke="black" style="stroke-dasharray: 3, 3; fill: none;"></line></g><g><text x="275" y="380.59375" class="messageText" style="text-anchor: middle;">Yes... John, how are you?</text><line x1="75" y1="387.59375" x2="475" y2="387.59375" class="messageLine0" stroke-width="2" stroke="black" style="fill: none;"></line></g><g><rect x="0" y="407.59375" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="440.09375" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="75" dy="0">Alice</tspan></text></g><g><rect x="200" y="407.59375" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="275" y="440.09375" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="275" dy="0">Bob</tspan></text></g><g><rect x="400" y="407.59375" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="475" y="440.09375" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="475" dy="0">John</tspan></text></g></svg></div>
<p>And this will produce a flow chart:</p>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-y98pVwnrE8gtkSzc" height="100%" viewBox="0 0 605.8874969482422 232.359375" style="max-width:605.8874969482422px;"><g><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M141.65692249754056,84.26953125L207.890625,54.9296875L296.5625,54.9296875" marker-end="url(#arrowhead1772)" style="fill:none"></path><defs><marker id="arrowhead1772" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M141.65692249754056,136.76953125L207.890625,166.109375L270.984375,166.109375" marker-end="url(#arrowhead1773)" style="fill:none"></path><defs><marker id="arrowhead1773" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M366.421875,54.9296875L417,54.9296875L466.6588202324742,86.86071254340472" marker-end="url(#arrowhead1774)" style="fill:none"></path><defs><marker id="arrowhead1774" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M392,166.109375L417,166.109375L466.65881837093923,135.17835114681816" marker-end="url(#arrowhead1775)" style="fill:none"></path><defs><marker id="arrowhead1775" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" transform="translate(207.890625,54.9296875)" style="opacity: 1;"><g transform="translate(-38.09375,-16.25)" class="label"><foreignObject width="76.19140625" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Link text</span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" transform="" style="opacity: 1;"><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" id="A" transform="translate(82.3984375,110.51953125)" style="opacity: 1;"><rect rx="0" ry="0" x="-62.3984375" y="-26.25" width="124.796875" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-52.3984375,-16.25)"><foreignObject width="104.8046875" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Square Rect</div></foreignObject></g></g></g><g class="node" id="B" transform="translate(331.4921875,54.9296875)" style="opacity: 1;"><circle x="-34.9296875" y="-26.25" r="34.9296875"></circle><g class="label" transform="translate(0,0)"><g transform="translate(-24.9296875,-16.25)"><foreignObject width="49.86328125" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Circle</div></foreignObject></g></g></g><g class="node" id="C" transform="translate(331.4921875,166.109375)" style="opacity: 1;"><rect rx="5" ry="5" x="-60.5078125" y="-26.25" width="121.015625" height="52.5"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-50.5078125,-16.25)"><foreignObject width="101.015625" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Round Rect</div></foreignObject></g></g></g><g class="node" id="D" transform="translate(503.9437484741211,110.51953125)" style="opacity: 1;"><polygon points="61.94375,0 123.8875,-61.94375 61.94375,-123.8875 0,-61.94375" rx="5" ry="5" transform="translate(-61.94375,61.94375)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-41.1796875,-16.25)"><foreignObject width="82.36328125" height="32.5"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Rhombus</div></foreignObject></g></g></g></g></g></g></svg></div>

