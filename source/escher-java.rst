EscherJava
==========

 * source code: `EscherJava <https://github.com/emartech/escher-java>`_
 * maven package: `com.emarsys.escher <https://mvnrepository.com/artifact/com.emarsys/escher>`_

Installation
------------

Simply add the *com.emarsys.escher* artifact to your *pom.xml*.

.. code-block:: text

  <!-- https://mvnrepository.com/artifact/com.emarsys/escher -->
  <dependency>
    <groupId>com.emarsys</groupId>
    <artifactId>escher</artifactId>
    <version>0.3</version>
  </dependency>

Usage
-----

Once the artifact is downloaded it is ready to be used.

The library has 3 interfaces you can call. You can sign an HTTP request, you can presign a URL
and you can validate a signed HTTP request or presigned URL (with the same method).

Signing a Request
^^^^^^^^^^^^^^^^^

Escher works by calculating a cryptographic signature of your request, and adding it (and other authentication
information) to the request.
Usually you will want to add the authentication information by appending extra headers to it.
Let's say you want to send a signed POST request to http://example.com/ using the apache HttpClient.

.. code-block:: java

   public class ClientTest {

     public static void main(String... args) {
             String url = "http://example.com/";

             HttpClient client = HttpClientBuilder.create().build();
             HttpRequestBase request = new HttpGet(url);
             request.addHeader("host", "example.com");
             request.addHeader("Content-Type", ContentType.APPLICATION_JSON.toString());
             request = signRequest(request);

             HttpResponse response = client.execute(request);
             System.out.println("Response Code : " + response.getStatusLine().getStatusCode());
             System.out.println(fetchResponse(response));
     }


     private static HttpRequestBase signRequest(HttpRequestBase request) throws EscherException {
         MyEscherRequest escherRequest = new MyEscherRequest(request, "");

         Escher escher = new Escher("eu/suite/ems_request")
                 .setAuthHeaderName("X-Ems-Auth")
                 .setDateHeaderName("X-Ems-Date")
                 .setAlgoPrefix("EMS");

         escher.signRequest(escherRequest, "ACCESS_KEY_ID", "SECRET", Arrays.asList("Content-Type", "X-Ems-Date", "host"));

         return escherRequest.getHttpRequest();
     }


     private static String fetchResponse(HttpResponse response) throws IOException {
         try (BufferedReader rd = new BufferedReader(
                 new InputStreamReader(response.getEntity().getContent()))) {
             return rd.lines().collect(Collectors.joining("\n"));
         }
     }
   }


   class MyEscherRequest implements EscherRequest {

     private HttpRequestBase httpRequest;
     private String body;


     public MyEscherRequest(HttpRequestBase httpRequest, String body) {
         this.httpRequest = httpRequest;
         this.body = body;
     }


     @Override
     public String getHttpMethod() {
         return httpRequest.getMethod();
     }


     @Override
     public URI getURI() {
         return httpRequest.getURI();
     }


     @Override
     public List<EscherRequest.Header> getRequestHeaders() {
         return Arrays.asList(httpRequest.getAllHeaders())
                 .stream()
                 .map(header -> new EscherRequest.Header(header.getName(), header.getValue()))
                 .collect(Collectors.toList());
     }


     @Override
     public void addHeader(String fieldName, String fieldValue) {
         httpRequest.addHeader(fieldName, fieldValue);
     }


     @Override
     public String getBody() {
         return body;
     }


     public HttpRequestBase getHttpRequest() {
         return httpRequest;
     }
   }

The full client demo code is available `here <https://github.com/emartech/escher-java/blob/master/src/main/java/com/emarsys/escher/demo/ClientTest.java>`_.

Presigning a URL
^^^^^^^^^^^^^^^^

In some cases you may want to send authenticated requests from a context where you cannot modify the request headers,
e.g. when embedding an API generated iframe.
You can however generate a presigned URL, where the authentication information is added to the query string.

.. code-block:: java

   Escher escher = new Escher("eu/suite/ems_request")
             .setAuthHeaderName("X-Ems-Auth")
             .setDateHeaderName("X-Ems-Date")
             .setAlgoPrefix("EMS");

   escher.presignUrl('http://example.com', "ACCESS_KEY_ID", "SECRET", 60);

The full client demo code is available `here <https://github.com/emartech/escher-java/blob/master/src/main/java/com/emarsys/escher/demo/ClientTest.java>`_.

Validating a request
^^^^^^^^^^^^^^^^^^^^

You can validate a request signed by the methods described above. For that you will need a database of the access keys and secrets of your clients.
Escher accepts a *Map<String, String>* object as the key database.

.. code-block:: java

   public class ServerTest {

     public static void main(String... args) throws Exception {

       HttpServer server = HttpServer.create(new InetSocketAddress(8888), 0);
       server.createContext("/", exchange -> {
          String response;
          try {
               authenticate(exchange);

               response = "Everything is OK\n";

           } catch (EscherException e) {
               response = e.getMessage();
           }

           exchange.sendResponseHeaders(200, response.length());

           try (OutputStream os = exchange.getResponseBody()) {
               os.write(response.getBytes());
           }

         });
         server.setExecutor(null);
         server.start();
         System.out.println("started");
     }


     private static void authenticate(HttpExchange exchange) throws EscherException {
         EscherRequest request = new MyServerEscherRequest(exchange);
         Escher escher = new Escher("eu/suite/ems_request")
                 .setAuthHeaderName("X-Ems-Auth")
                 .setDateHeaderName("X-Ems-Date")
                 .setAlgoPrefix("EMS");

         Map<String, String> keyDb = new HashMap<>();
         keyDb.put("ACCESS_KEY_ID", "SECRET");

         escher.authenticate(request, keyDb, new InetSocketAddress("localhost", 8888));
     }
   }


   class MyServerEscherRequest implements EscherRequest {

     private HttpExchange exchange;

     public MyServerEscherRequest(HttpExchange exchange) {
         this.exchange = exchange;
     }

     @Override
     public String getHttpMethod() {
         return exchange.getRequestMethod();
     }

     @Override
     public URI getURI() {
         try {
             return new URIBuilder(exchange.getRequestURI())
                     .setScheme("http")
                     .build();
         } catch (URISyntaxException e) {
             throw new RuntimeException(e);
         }
     }

     @Override
     public List<Header> getRequestHeaders() {
         List<Header> headers = new ArrayList<>();
         exchange.getRequestHeaders().forEach((fieldName, fieldValues) ->
                 fieldValues.forEach(fieldValue ->
                         headers.add(new Header(fieldName, fieldValue))
                 )
         );
         return headers;
     }

     @Override
     public void addHeader(String fieldName, String fieldValue) {
         throw new RuntimeException("Should not be called");
     }

     @Override
     public String getBody() {
         try (BufferedReader br = new BufferedReader(new InputStreamReader(exchange.getRequestBody()))) {
             return br.lines().collect(Collectors.joining("\n"));
         } catch (IOException e) {
             throw new RuntimeException(e);
         }
     }
   }

The full server demo code is available `here <https://github.com/emartech/escher-java/blob/master/src/main/java/com/emarsys/escher/demo/ServerTest.java>`_.
