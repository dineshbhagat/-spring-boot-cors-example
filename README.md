# spring-boot-cors-example [![forthebadge](https://forthebadge.com/images/badges/made-with-java.svg)](https://forthebadge.com) [![forthebadge](https://forthebadge.com/images/badges/made-with-javascript.svg)](https://forthebadge.com)

CORS stands for Cross-Origin Resource Sharing

CORS is a security mechanism built into (all) modern web-browsers (yes! into your web browser! That’s why your curl calls works fine). It basically blocks all the http requests from your front end to any API that is not in the same “Origin” (domain, protocol, and port—which is the case most of the time).

CORS is a mechanism built into web browser. It’s not a UI code issue
To fix CORS problems, you need to make changes on the API side.

UI domain: http://localhost:9000/  

Backend Domain: http://localhost:8080/ --> Backend enables all the requests from UI domain.  

UI is trying to connect to backend service hosted at http://localhost:8080/greeting.    

So for browser, request is made to the domain different(i.e. localhost:8080/greeting) from that of its own domain (i.e. http://localhost:9000/)   
So browser protects itself from such requests by blocking it.




Steps:
1. Open command Terminal 1(or run in IDE): 
```shell
$./gradlew clean build -x test && java -jar build/libs/gs-rest-service-cors-0.1.0.jar 
```
2. On Terminal 2: 
```shell 
$./mvnw clean install -DskipTest test; mvn spring-boot:run -Dserver.port=9000
```
3. open http://localhost:9000/
4. open debugger window at "http://localhost:9000/" and execute following js 

```javascript
var createCORSRequest = function(method, url) {
  var xhr = new XMLHttpRequest();
  if ("withCredentials" in xhr) {
    // Most browsers.
    xhr.open(method, url, true);
    xhr.setRequestHeader('Content-Type', 'application/json');
  } else if (typeof XDomainRequest != "undefined") {
    // IE8 & IE9
    xhr = new XDomainRequest();
    xhr.open(method, url);
  } else {
    // CORS not supported.
    xhr = null;
  }
  return xhr;
};

var url = 'http://localhost:8080/greeting';
var method = 'GET';
var xhr = createCORSRequest(method, url);

xhr.onload = function() {
	var v = xhr.responseText;
	console.log(xhr.responseText);
	$('.greeting-id').append(v.id);
       $('.greeting-content').append(v.content);
  console.log("success");
};

xhr.onerror = function() {
  console.log("error");
};

xhr.send();
```

OR 
POST call

```javascript
var createCORSRequest = function(method, url) {
  var xhr = new XMLHttpRequest();
  if ("withCredentials" in xhr) {
    // Most browsers.
    xhr.open(method, url, true);
    xhr.setRequestHeader('Content-Type', 'application/json');
  } else if (typeof XDomainRequest != "undefined") {
    // IE8 & IE9
    xhr = new XDomainRequest();
    xhr.open(method, url);
  } else {
    // CORS not supported.
    xhr = null;
  }
  return xhr;
};

var url = 'http://localhost:8080/add';
var method = 'POST';
var xhr = createCORSRequest(method, url);

xhr.onload = function() {
  var v = xhr.responseText;
  console.log(xhr.responseText);
  $('.greeting-id').append(v.id);
  $('.greeting-content').append(v.content);
  console.log("success");
};

xhr.onerror = function() {
  console.log("error");
};

var data=JSON.stringify({"a":"b"})
//add body data here
xhr.send(data);
```
5. Check network tab on debugger, we can see OPTIONS and GET calls
6. step 2-5 can be done for any other port(here 9000)

<img width="1668" alt="screen shot 2018-10-09 at 4 51 08 pm" src="https://media.git.target.com/user/12476/files/da9d4904-cbe3-11e8-8f01-153199c5904d">
<img width="1675" alt="screen shot 2018-10-09 at 4 51 20 pm" src="https://media.git.target.com/user/12476/files/d4b68ec4-cbe3-11e8-8372-c92f8e0f5a78">

Post call
<img width="1676" alt="Screenshot 2021-04-16 at 2 27 29 PM" src="https://user-images.githubusercontent.com/3823705/115002022-e4d56880-9ec1-11eb-8316-1f9ff8fefe24.png">

OR
We can test this using postman by passing header "Origin"

Origin: http://localhost:9000

Then send the request OPTION/GET/POST

This will result in `Invalid CORS request`

```shell
curl -X GET \
  http://localhost:8080/greeting \
  -H 'Accept: */*' \
  -H 'Access-Control-Request-Method: GET' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: localhost:8080' \
  -H 'Origin: http://localhost:30840' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: '
```  
  
But this will result in success:

```shell
curl -X GET \
  http://localhost:8080/greeting \
  -H 'Accept: */*' \
  -H 'Access-Control-Request-Method: GET' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: localhost:8080' \
  -H 'Origin: http://localhost:9000' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: '
```  
  
  
Refs:

https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

https://spring.io/guides/gs/rest-service-cors/#initial

https://blog.container-solutions.com/a-guide-to-solving-those-mystifying-cors-issues

