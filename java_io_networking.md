# Java File I/O and Networking

## 1. File Input and Output (I/O)

### 1.1 Reading a File Using Byte Streams

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ByteStreamExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("example.txt")) {
            int content;
            while ((content = fis.read()) != -1) {
                System.out.print((char) content);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 1.2 Writing to a File Using Byte Streams

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamWriteExample {
    public static void main(String[] args) {
        String data = "Hello, World!";
        try (FileOutputStream fos = new FileOutputStream("output.txt")) {
            fos.write(data.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 1.3 Reading a File Using Character Streams

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class CharStreamExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 1.4 Writing to a File Using Character Streams

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class CharStreamWriteExample {
    public static void main(String[] args) {
        String data = "Hello, World!";
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
            bw.write(data);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 2. Java Networking

### 2.1 TCP Programming

#### Server

```java
import java.io.*;
import java.net.*;

public class TCPServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(5000)) {
            System.out.println("Server started, waiting for client connection...");
            while (true) {
                try (Socket clientSocket = serverSocket.accept();
                     BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
                     PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true)) {
                    System.out.println("Client connected: " + clientSocket.getInetAddress());
                    String message = in.readLine();
                    System.out.println("Received from client: " + message);
                    out.println("Server received: " + message);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Client

```java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) {
        String serverAddress = "localhost";
        int port = 5000;
        try (Socket socket = new Socket(serverAddress, port);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {
            out.println("Hello, Server!");
            String response = in.readLine();
            System.out.println("Server response: " + response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 2.2 UDP Programming

#### Server

```java
import java.net.*;

public class UDPServer {
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket(5000)) {
            byte[] buffer = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            System.out.println("UDP Server started, waiting for client messages...");
            while (true) {
                socket.receive(packet);
                String received = new String(packet.getData(), 0, packet.getLength());
                System.out.println("Received from client: " + received);
                String response = "Server received: " + received;
                byte[] responseData = response.getBytes();
                DatagramPacket responsePacket = new DatagramPacket(
                        responseData, responseData.length, packet.getAddress(), packet.getPort());
                socket.send(responsePacket);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Client

```java
import java.net.*;

public class UDPClient {
    public static void main(String[] args) {
        String serverAddress = "localhost";
        int port = 5000;
        try (DatagramSocket socket = new DatagramSocket()) {
            String message = "Hello, Server!";
            byte[] buffer = message.getBytes();
            InetAddress address = InetAddress.getByName(serverAddress);
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, port);
            socket.send(packet);
            byte[] responseBuffer = new byte[1024];
            DatagramPacket responsePacket = new DatagramPacket(responseBuffer, responseBuffer.length);
            socket.receive(responsePacket);
            String response = new String(responsePacket.getData(), 0, responsePacket.getLength());
            System.out.println("Server response: " + response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

