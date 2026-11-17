### HttpURLConnection – GET request

The `HttpURLConnection` class from `java.net` is used to send HTTP requests and receive responses from a web server.

```java
import java.net.*;
import java.io.*;

public class HttpExample {

    public static void main(String[] args) throws Exception {

        URL url = new URL("https://example.com");

        HttpURLConnection con =
            (HttpURLConnection) url.openConnection();

        con.setRequestMethod("GET");

        System.out.println("Response: " + con.getResponseCode());

        try (BufferedReader reader = new BufferedReader(
                new InputStreamReader(con.getInputStream()))) {

            System.out.println(reader.readLine());
        }

        con.disconnect();
    }
}
```

**Possible output:**
```text
Response: 200
<!doctype html>
```

- `openConnection()` – establishes the connection.
- `setRequestMethod("GET")` – specifies the HTTP method.
- `getResponseCode()` – retrieves the HTTP status code.
- `getInputStream()` – reads the server response.
- `disconnect()` – closes the connection.

### HttpURLConnection – POST request

A POST request sends data to a web server. The `setDoOutput(true)` method enables sending data through the connection's output stream.

```java
import java.net.*;
import java.io.*;

public class HttpPostExample {

    public static void main(String[] args) throws Exception {

        URL url = new URL("https://jsonplaceholder.typicode.com/posts");

        HttpURLConnection con =
            (HttpURLConnection) url.openConnection();

        con.setRequestMethod("POST");
        con.setRequestProperty("Content-Type", "application/json");
        con.setDoOutput(true);

        String json = """
            {"title":"Java","body":"Hello World","userId":1}
            """;

        try (OutputStream os = con.getOutputStream()) {
            os.write(json.getBytes("UTF-8"));
        }

        System.out.println("Response: " + con.getResponseCode());

        con.disconnect();
    }
}
```

**Possible output:**
```text
Response: 201
```

- `setRequestMethod("POST")` – specifies a POST request.
- `setRequestProperty()` – sets the content type.
- `setDoOutput(true)` – enables sending data.
- `getOutputStream()` – writes data to the server.
- `getResponseCode()` – retrieves the HTTP response status.

