# Plain Java
## General Idea

If you are consuming a RESTful API, you can do it with any flavour of your choice:

If you don't want to use external libraries, you can use `java.net.HttpURLConnection` or `javax.net.ssl.HttpsURLConnection` (for SSL), but that is call encapsulated in a Factory type pattern in `java.net.URLConnection`. To receive the result, you will have to `connection.getInputStream()`
which returns you an `InputStream`. You will then have to convert your input stream to string and parse the string into it's representative object (e.g. XML, JSON, etc).

Alternatively, [Apache HttpClient](https://hc.apache.org/downloads.cgi) (version 4 is the latest). It's more stable and robust than java's default `URLConnection` and it supports most (if not all) HTTP protocol (as well as it can be set to Strict mode). Your response will still be in InputStream and you can use it as mentioned above.
> StackOverflow link [here](https://stackoverflow.com/questions/3913502/restful-call-in-java)


# Spring Boot
Rest template + jackson object mapper
Rest template uses Jackson internally.
- https://www.baeldung.com/rest-template 
- https://www.baeldung.com/jackson-object-mapper-tutorial
- https://www.baeldung.com/jackson-annotations
- https://stackoverflow.com/questions/47611062/using-resttemplate-to-map-json-to-object
