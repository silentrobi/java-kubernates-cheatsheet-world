# Tomcat Web Server
It is a web server container that contains the web app and manages HTTP requests and responses.

To run Tomcat with a Web API and serve an index.html file (e.g., a front-end UI or static page), you need to structure your project in a way that Tomcat understands—typically as a WAR (Web Application Archive).

Here’s a step-by-step guide for both static HTML and Java-based API (Servlet/Spring Boot) served via Tomcat:

## ✅ OPTION 1: Deploy Static index.html on Tomcat
If you just want to serve an HTML file (like a front-end UI):

📁 Directory Structure
Place your files inside Tomcat’s webapps directory:

```text
apache-tomcat-X.X.X/
└── webapps/
    └── myapp/
        └── index.html
```

🧾 Example index.html

```html
<!-- webapps/myapp/index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>My API UI</title>
</head>
<body>
    <h1>Welcome to My Web API</h1>
</body>
</html>
```

🚀 How to Access
Start Tomcat (bin/startup.sh or startup.bat), then visit:

```
http://localhost:8080/myapp/
```
It will automatically load index.html.

## ✅ OPTION 2: Run Java Web API + index.html on Tomcat
If you want to:

Build a REST API (e.g., using Servlets or Spring Boot)

Serve index.html as the UI

Deploy the app to Tomcat

### 🏗️ A. Create a WAR-based Web App (Using Maven)
1. Maven pom.xml (Servlet Example)
```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>my-webapp</artifactId>
  <version>1.0</version>
  <packaging>war</packaging>

  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
</project>
```
2. Directory Structure
```text
my-webapp/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/HelloServlet.java
│       ├── resources/
│       └── webapp/
│           ├── index.html
│           └── WEB-INF/
│               └── web.xml
```
3. Example HelloServlet.java
```java
@WebServlet("/api/hello")
public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse res)
        throws IOException {
        res.getWriter().write("Hello from Servlet API");
    }
}
```
4. Example web.xml (optional with annotations)
```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee" version="3.0">
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
```
5. Build WAR and Deploy
```bash
mvn clean package
```
Copy the generated target/my-webapp.war to:
```text
TOMCAT_HOME/webapps/
```
Then start Tomcat and access:
- `http://localhost:8080/my-webapp/` → loads index.html
- `http://localhost:8080/my-webapp/api/hello` → returns "Hello from Servlet API"

✅ OPTION 3: Spring Boot (with Embedded Tomcat)
If you’re using Spring Boot, it doesn’t need Tomcat installed, but you can still deploy it as a WAR if you want to use an external Tomcat.

# Servlet


![image](https://github.com/user-attachments/assets/5833fcd1-4f2e-485b-b42f-b65d39f93db5)

# Spring MVC (DispatcherServlet)
Follow the project example that gives you basic understanding how spring works with servlet!
https://www.youtube.com/watch?v=g2b-NbR48Jo&t=1257s
![image](https://github.com/user-attachments/assets/02f818d2-e5dd-4bec-88b2-d8ef6913e822)


