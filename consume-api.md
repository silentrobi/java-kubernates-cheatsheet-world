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

## Consume multiple API parallelly using @Async

1. First define a AsyncClient class. 
```java
@Service
@RequiredArgsConstructor
public class AsyncClient {

    private final RestTemplate restTemplate;

    @Async("threadPoolTaskExecutor") // If not specified then default SimpleAsyncTaskExecutor will be used.
    public <T> CompletableFuture<ResponseDto<T>>   callAsync(String url, ParameterizedTypeReference<T> responseType) {
        ResponseEntity<T> responseEntity = restTemplate.exchange(url, HttpMethod.GET, null, responseType);

        ResponseDto<T> responseDto = new ResponseDto<>(responseEntity.getStatusCode().toString(), responseEntity.getBody());

        final var currentThread = Thread.currentThread();
        System.out.println("Thread name: " + currentThread.getName() + " with id:" + currentThread.getId() + " is doing this task");


        return CompletableFuture.completedFuture(responseDto);
    }
}
```
callAsync is a generic GET method that is asyncronous in nature. By default when @Async method is called Spring uses `SimpleAsyncTaskExecutor` which starts a new thread for each invocation, so no thread pool is used. You can however provide your own Task Executor or use `ThreadPoolTaskExecutor`.

Following code will display 3 threads. One main and rest are from threadpool of size 2.
```java
@SpringBootApplication
@EnableScheduling
@EnableWebMvc
@EnableAsync
public class SpringUsecasesApplication {

    @Bean
    RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
  
    /**Task Executor strategy to use*/
   @Bean
    public Executor threadPoolTaskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(2);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("ThreadPoolExecutor-");
        executor.initialize();
        return executor;
    }

    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(SpringUsecasesApplication.class, args);

        AsyncClient asyncClient = ctx.getBean(AsyncClient.class);

        var currentThread = Thread.currentThread();
        System.out.println("Thread name: " + currentThread.getName() + " with id:" + currentThread.getId() + " is doing this task");
        List<CompletableFuture<ResponseDto<Blog>>> futures = new ArrayList<>();

        for(int i = 0; i < 3 ; i++){
            futures.add(asyncClient.callAsync("https://api.hatchways.io/assessment/blog/posts?tag=history", new ParameterizedTypeReference<>() {
            }));
        }

        //wait for all thread to finish execution.
        futures.forEach(future -> {
            final var result = future.join();
            final var posts = result.getBody().getPosts();
        });

        currentThread = Thread.currentThread();
        System.out.println("Thread name: " + currentThread.getName() + " with id:" + currentThread.getId() + " is doing this task");
    }
}

Otherway to controll the thread pool:
```java
final var threadPool = Executors.newFixedThreadPool(2);
      futures.add(CompletableFuture.supplyAsync(() -> blogClient.callSync(URL, new ParameterizedTypeReference<>(){}), threadPool));
}
```
```
**Sample Output**

```
Thread name: main with id:1 is doing this task
Thread name: ThreadPoolExecutor-1 with id:47 is doing this task
Thread name: ThreadPoolExecutor-2 with id:48 is doing this task
Thread name: ThreadPoolExecutor-2 with id:48 is doing this task
Thread name: main with id:1 is doing this task
```
## Consume multiple API using WebClient (Reactive approach)
// todo....
