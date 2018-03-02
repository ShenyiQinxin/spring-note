# Spring MVC for RESTful Web Services
## REST introduction
-  An  architectural style
-  REpresentational State **Transfer**
-  A best practice to transfer **web services** over **HTTP** in a form of representainal states
-  Expose resources through URIs using nouns
- Operations are perfomed by a set of HTTP methods : GET, PUT, POST, DELETE
i.e. update an order
```js
    PUT to /orders/123
    don't POST to /order/edit?id=123
```
-  Emphasizes  **scalability**
- Resources can support multiple representations:  HTML, XML, JSON
- Representations can link to other resources:    HATEOAS (Hypermedia As The Engine of Application State). RESTful responses contain the links you need, just like HTML pages do
- Stateless architecture  
    – No HttpSession usage  
    – GETs can be cached on URL  
    – Requires clients to keep track of state  
    – Part of what makes it scalable  
    – Looser coupling between client and server
-  HTTP headers and status codes communicate result to clients
## Spring MVC support for RESTful applications
### Request/Response Processing
#### @RequestMapping/@GetMapping
>same URL to be mapped to multiple methods
```java
@RequestMapping(path="/orders", method=RequestMethod.GET) public void listOrders( ... ) {

 // find all Orders and add them to the model 

}

@RequestMapping(path="/orders", method=RequestMethod.POST) public void createOrder( ... ) {

 // process the order data from the request 

}
```
>Alternatives
```java
**@RequestMapping**(path="/accounts”, method=GET)
or @GetMapping("/accounts");

@GetMapping  
@PostMapping  
@PutMapping
@DeleteMapping
@PatchMapping
```
>HTTP Status Code Support
```
    – Success: 200 OK  
    – Redirect: 30x for Redirects  
    – Client Error: 404 Not Found 
    – Server Error: 500 (such as unhandled Exceptions)
    
   RESTful applications use many additional codes
    
    – Created Successfully: 201  
    – HTTP method not supported: 405  
    – Cannot generate response body requested: 406 
    – Request body not supported: 415
```
#### @ResponseStatus
>- **@ResponseStatus** on void methods, Method returns a response with empty body (no-content )
>- @ResponseStatus can be at class level, applying on all methods of the class
```java
@RequestMapping(path="/orders/{id}", method=RequestMethod.PUT)
@ResponseStatus(HttpStatus.NO_CONTENT) // 204  
public void updateOrder(HttpServletRequest request) {

 Order order = getOrder(request);// Extract from request 

 orderService.updateOrder(order);
}
```

#### HttpMessageConverter
>Converts HTTP request/response body data
> Automatic with Spring Boot  
> `@EnableWebMvc or <mvc:annotation-driven/> ` 
> Or define explicitly (allows you to register extra convertors)
> Using `WebMvcConfigurer or <mvc/>`
- @RequestBody
>converts the incoming data from XML/JSON 
```java
@RequestMapping(path="/orders/{id}", method=RequestMethod.PUT) @ResponseStatus(HttpStatus.NO_CONTENT) // 204  

//@RequestBody in method param
public void updateOrder(@RequestBody Order updatedOrder,
@PathVariable("id") long id) {
 // process updated order data and return empty response 

 orderManager.updateOrder(id, updatedOrder);
}
```
- @ResponseBody
```java
@RequestMapping(path="/orders/{id}", method=RequestMethod.GET) @ResponseStatus(HttpStatus.OK) // 200  

//@ResponseBody in method return type
public @ResponseBody Order getOrder(@PathVariable("id") long id) {
// Order class is annotated with JAXB2's @XmlRootElement 

 Order order= orderService.findOrderById(id); 

// results in XML response containing marshalled order: 

 return order;
}
```
- Return format is defined by Accept Header
```
GET /store/orders/123
Host: shop.spring.io 
*Accept*: application/xml
=>
HTTP/1.1 200 OK
Date: ...  
Content-Length: 1456 
*Content-Type*: application/xml
<order id=”123”> 
...  
</order>
```
- @RestController
> no @ResponseBody specified in @Controller
```java
@Controller
public class OrderController {  
@RequestMapping(path="/orders/{id}", method=RequestMethod.GET) 
public @ResponseBody Order getOrder(@PathVariable("id") long id) {

 return orderService.findOrderById(id);
  } 
... 
}

@RestController 
public class OrderController { @RequestMapping(path="/orders/{id}", method=RequestMethod.GET) public Order getOrder(@PathVariable("id") long id) {

 return orderService.findOrderById(id);
  } 
...
}
```
### Accessing Request/Response Data 
#### HttpEntity & ResponseEntity
```java
// Want to return a String as the response-body 
HttpHeaders headers = new HttpHeaders(); headers.setContentType(MediaType.TEXT_PLAIN); 
HttpEntity<String> entity = new HttpEntity<String>("Hello Spring", headers);
//======================
// ResponseEntity (since Spring 4.1) supports a “fluent” API ResponseEntity<String> response =
ResponseEntity.ok()
               .contentType(MediaType.TEXT_PLAIN) 
               .body("Hello Spring");
//======================
@RequestMapping(path="/orders/{id}", method=RequestMethod.GET) 
public HttpEntity<Order> getOrder(@RequestParam("id") long id){

 String order = orderService.find(id);
 HttpHeaders responseHeaders = new HttpHeaders();
 responseHeaders.set("Content-Type", "text/xml"); 
 return new HttpEntity<Order>(order, responseHeaders);

}
```
#### Building URIs: UriComponentsBuilder
```java
String templateUrl =
 "http://store.spring.io/orders/{orderId}/items/{itemId}"; 

URI location = UriComponentsBuilder
				.fromHttpUrl(templateUrl)
				.buildAndExpand("123456","item A")
				.toUri();
```
### Putting it all together
```java
@ResponseStatus  
HTTP Message Converters:  
@RequestBody, @ResponseBody
HttpEntity
ResponseEntity
UriComponentsBuilder
```
>Controllers for GET, POST, PUT and DELETE
>Retrieving a Representation: GET
```java
@GetMapping(path="/orders/{id}")  
public @ResponseBody Order getOrder(@PathVariable("id") long id){
	return orderService.findOrderById(id); 
}
```
>Creating a new Resource: POST
>`ServletUriComponentsBuilder.fromCurrentRequestUri `– Returns a `UriComponentsBuilder` initialized to URL of current Controller method  
>- Useful as our new resource is a sub-path of the POST URL
```java
@PostMapping(path="/orders/{id}/items") @ResponseStatus(HttpStatus.CREATED) // 201  
public ResponseEntity<Void> createItem(@PathVariable("id") long id, @RequestBody Item newItem) {

 // Add the new item to the order 
orderService.findOrderById(id).addItem(newItem); 

 // Build the location URI of the new item 
 URI location = ServletUriComponentsBuilder
 .fromCurrentRequestUri()
 .path("items/{itemId}")
 .buildAndExpand(newItem.getId()).toUri();

return ResponseEntity.created(location).build(); }
```
>Updating existing Resource: PUT
```java
@PutMapping(path="/orders/{orderId}/items/{itemId}") @ResponseStatus(HttpStatus.NO_CONTENT) // 204  
public void updateItem(@PathVariable("orderId") long orderId,
@PathVariable("itemId") String itemId, 
@RequestBody Item item) {
   orderService.findOrderById(orderId.updateItem(itemId, item); 

}
```
>Deleting a Resource: DELETE
```java
@DeleteMapping(path="/orders/{orderId}/items/{itemId}") @ResponseStatus(HttpStatus.NO_CONTENT) // 204  
public void deleteItem(@PathVariable("orderId") long orderId,
@PathVariable("itemId") String itemId) {
 orderService.findOrderById(orderId).deleteItem(itemId); 

}
```
## RESTful Clients with the RestTemplate
>Provides access to RESTful services
![enter image description here](https://user-images.githubusercontent.com/32177380/36919242-a82cde24-1e2a-11e8-895d-a94b96704cf6.png)
```java
```
