See this thread for information on this repo: http://tomcat.10.x6.nabble.com/Issue-with-WebResource-Caching-td5074759.html

The issue identified by this repo will be fixed in Tomcat 8.0.53+, 8.5.32+ and 9.0.9+.

# Testing

## Tomcat 8.0.38 (Working)
```
$ mvn clean verify org.codehaus.cargo:cargo-maven2-plugin:run \
    -Dcargo.maven.containerId=tomcat8x \
    -Dcargo.maven.containerUrl=http://repo1.maven.org/maven2/org/apache/tomcat/tomcat/8.0.38/tomcat-8.0.38.tar.gz
$ curl -v http://localhost:8080/tomcat8-cache-issue/_resource/dari/profiler.js
14:42 $ curl -v http://localhost:8080/tomcat8-cache-issue/_resource/dari/profiler.js
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /tomcat8-cache-issue/_resource/dari/profiler.js HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: Apache-Coyote/1.1
< Content-Type: text/javascript;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Thu, 17 May 2018 18:42:47 GMT
< 
(function() {
    var scripts = document.getElementsByTagName('script');
    var javaContextPath = scripts[scripts.length - 1].getAttribute('data-java-context-path');
.
.
.
* Connection #0 to host localhost left intact
$ 
```

## Tomcat 8.0.39 (Broken)
```
$ mvn clean verify org.codehaus.cargo:cargo-maven2-plugin:run \
    -Dcargo.maven.containerId=tomcat8x \
    -Dcargo.maven.containerUrl=http://repo1.maven.org/maven2/org/apache/tomcat/tomcat/8.0.39/tomcat-8.0.39.tar.gz
$ curl -v http://localhost:8080/tomcat8-cache-issue/_resource/dari/profiler.js
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /tomcat8-cache-issue/_resource/dari/profiler.js HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 404 Not Found
< Server: Apache-Coyote/1.1
< Content-Type: text/html;charset=utf-8
< Content-Language: en
< Content-Length: 1086
< Date: Thu, 17 May 2018 18:44:17 GMT
< 
* Connection #0 to host localhost left intact
<!DOCTYPE html><html><head><title>Apache Tomcat/8.0.39 - Error report</title><style type="text/css">H1 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:22px;} H2 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:16px;} H3 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:14px;} BODY {font-family:Tahoma,Arial,sans-serif;color:black;background-color:white;} B {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;} P {font-family:Tahoma,Arial,sans-serif;background:white;color:black;font-size:12px;}A {color : black;}A.name {color : black;}.line {height: 1px; background-color: #525D76; border: none;}</style> </head><body><h1>HTTP Status 404 - /tomcat8-cache-issue/_resource/dari/profiler.js</h1><div class="line"></div><p><b>type</b> Status report</p><p><b>message</b> <u>/tomcat8-cache-issue/_resource/dari/profiler.js</u></p><p><b>description</b> <u>The requested resource is not available.</u></p><hr class="line"><h3>Apache Tomcat/8.0.39</h3></body></html>
$
```
