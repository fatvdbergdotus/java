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

