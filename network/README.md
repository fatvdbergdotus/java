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


# Java TCP and UDP communication

Java supports two common network communication protocols: TCP (Transmission Control Protocol) and UDP (User Datagram Protocol). Both allow applications to exchange data over a network, but they differ in reliability, connection handling and data transmission.

## 1. TCP communication using Socket and ServerSocket

TCP is connection-oriented and provides reliable, ordered data transmission. The `ServerSocket` class listens for incoming connections, while the `Socket` class represents an established connection between the client and server.

### TCP server

```java
import java.net.*;
import java.io.*;

public class TCPServer {

    public static void main(String[] args) throws IOException {

        ServerSocket server = new ServerSocket(5000);

        System.out.println("Waiting for client...");

        try (Socket socket = server.accept();
             BufferedReader in = new BufferedReader(
                 new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(
                 socket.getOutputStream(), true)) {

            String message = in.readLine();

            System.out.println("Received: " + message);

            out.println("Hello from server!");
        }

        server.close();
    }
}
```

### TCP client

```java
import java.net.*;
import java.io.*;

public class TCPClient {

    public static void main(String[] args) throws IOException {

        try (Socket socket = new Socket("localhost", 5000);
             PrintWriter out = new PrintWriter(
                 socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(
                 new InputStreamReader(socket.getInputStream()))) {

            out.println("Hello from client");

            System.out.println(in.readLine());
        }
    }
}
```

**Possible output:**

Server:
```text
Waiting for client...
Received: Hello from client
```

Client:
```text
Hello from server!
```

## 2. UDP communication using DatagramSocket and DatagramPacket

UDP is connectionless and does not guarantee delivery or message ordering. The `DatagramSocket` class sends and receives UDP datagrams, while `DatagramPacket` represents the actual message and its addressing information.

### UDP server

```java
import java.net.*;

public class UDPServer {

    public static void main(String[] args) throws Exception {

        DatagramSocket socket = new DatagramSocket(5000);

        byte[] buffer = new byte[1024];

        DatagramPacket packet =
            new DatagramPacket(buffer, buffer.length);

        System.out.println("Waiting for message...");

        socket.receive(packet);

        String message = new String(
            packet.getData(), 0, packet.getLength()
        );

        System.out.println("Received: " + message);

        socket.close();
    }
}
```

### UDP client

```java
import java.net.*;

public class UDPClient {

    public static void main(String[] args) throws Exception {

        DatagramSocket socket = new DatagramSocket();

        String message = "Hello from client";

        byte[] data = message.getBytes();

        DatagramPacket packet = new DatagramPacket(
            data,
            data.length,
            InetAddress.getByName("localhost"),
            5000
        );

        socket.send(packet);

        socket.close();
    }
}
```

**Possible output (server):**

```text
Waiting for message...
Received: Hello from client
```

## 3. TCP versus UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable delivery | No delivery guarantee |
| Ordering | Preserves data order | No ordering guarantee |
| Data transmission | Continuous byte stream | Individual datagrams |
| Error detection | Yes | Checksum |
| Retransmission | Yes | No automatic retransmission |
| Overhead | Higher | Lower |
| Java classes | `Socket`, `ServerSocket` | `DatagramSocket`, `DatagramPacket` |
| Typical applications | HTTP, file transfer, SSH | DNS, streaming, online gaming |

## 4. Key differences

- **TCP:** Establishes a connection before exchanging data. It provides reliable and ordered delivery through acknowledgements and retransmissions.
- **UDP:** Sends independent packets without establishing a connection. It has lower protocol overhead but does not guarantee delivery or ordering.
- **TCP sockets:** Use input and output streams to exchange data.
- **UDP sockets:** Use datagram packets to send and receive individual messages.

**Key takeaway:** Use TCP when reliability and ordered delivery are important. Use UDP when low latency and low communication overhead are more important than guaranteed delivery.
