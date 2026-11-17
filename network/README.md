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

# Java ServerSocket class

The `ServerSocket` class is part of the `java.net` package and is used to create server-side applications that listen for incoming TCP connections from clients. It waits for client requests on a specified port and establishes a connection using the `accept()` method, which returns a `Socket` object for communication with the client.

## Example: Simple TCP server

```java
import java.net.*;
import java.io.*;

public class ServerExample {

    public static void main(String[] args) throws IOException {

        ServerSocket server = new ServerSocket(5000);

        System.out.println("Waiting for client...");

        Socket socket = server.accept();

        PrintWriter out = new PrintWriter(
            socket.getOutputStream(), true
        );

        out.println("Hello from server!");

        socket.close();
        server.close();
    }
}
```

**Possible output:**

```text
Waiting for client...
```

## Example: Simple TCP client

```java
import java.net.*;
import java.io.*;

public class ClientExample {

    public static void main(String[] args) throws IOException {

        Socket socket = new Socket("localhost", 5000);

        BufferedReader in = new BufferedReader(
            new InputStreamReader(socket.getInputStream())
        );

        System.out.println(in.readLine());

        socket.close();
    }
}
```

**Possible output:**

```text
Hello from server!
```

## Important methods

| Method | Description |
|---|---|
| `ServerSocket(port)` | Creates a server listening on a port. |
| `accept()` | Waits for an incoming client connection. |
| `getLocalPort()` | Returns the server's listening port. |
| `close()` | Closes the server socket. |
| `isClosed()` | Checks whether the server socket is closed. |

## Key concepts

- `ServerSocket` listens for incoming TCP connections.
- `Socket` represents an established connection between a client and server.
- `accept()` blocks execution until a client connects.
- A server can handle multiple clients by accepting connections in a loop and processing them using separate threads.


