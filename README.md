# spring-boot-cors-example [![forthebadge](https://forthebadge.com/images/badges/made-with-java.svg)](https://forthebadge.com) [![forthebadge](https://forthebadge.com/images/badges/made-with-javascript.svg)](https://forthebadge.com)

Your domain is http://localhost:9000/, you are trying yo connect to another service hosted at http://localhost:8080/greeting.  
So for [service:8080](http://localhost:8080/greeting) request originated from another domain, so this is the case of CORS.



Steps:
1. Open command Terminal 1: 
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
5. Check network tab on debugger, we can see OPTIONS and GET calls
6. step 2-5 can be done for any other port(here 9000)

<img width="1675" alt="screen shot 2018-10-09 at 4 51 20 pm" src="https://media.git.target.com/user/12476/files/d4b68ec4-cbe3-11e8-8372-c92f8e0f5a78">
<img width="1668" alt="screen shot 2018-10-09 at 4 51 08 pm" src="https://media.git.target.com/user/12476/files/da9d4904-cbe3-11e8-8f01-153199c5904d">


Refs:

https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

https://spring.io/guides/gs/rest-service-cors/#initial
