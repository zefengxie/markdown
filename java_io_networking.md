# Java File I/O and Networking

## 1. File Input and Output (I/O)

### 1.1 Reading a File Using Byte Streams

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ByteStreamExample {
    public static void main(String[] args) {

        try (FileInputStream fis = new FileInputStream("example.txt")) {
            // Create a FileInputStream to read "example.txt". If the file does not exist 
            // or is not in the same directory, an error message will be thrown.

            int content;
            // Declare an integer variable to store the read character

            while ((content = fis.read()) != -1) {
                // Read the file byte by byte until the end (-1 indicates the end of the file)

                System.out.print((char) content);
                // Convert the byte to a character and print it
            }

        } catch (IOException e) { // Catch any file-related exceptions
            e.printStackTrace(); // Print the tracked error details
        }
    }
}
```

## Why do we use `e.printStackTrace();` to print out errors even though Java automatically prints them?

If an exception occurs and we don’t catch it (or don't call `e.printStackTrace();` inside `catch`), Java **stops execution** and prints an error message automatically.

### Example without try-catch (Uncaught Exception)

```java
import java.io.FileInputStream;

public class WithoutTryCatch {
    public static void main(String[] args) {
        FileInputStream fis = new FileInputStream("non_existent_file.txt"); // No try-catch block
    }
}
```

### Output (Default Java Error Message)
```
Exception in thread "main" java.io.FileNotFoundException: non_existent_file.txt (The system cannot find the file specified)
    at java.base/java.io.FileInputStream.open0(Native Method)
    at java.base/java.io.FileInputStream.open(FileInputStream.java:211)
    at java.base/java.io.FileInputStream.<init>(FileInputStream.java:152)
    at WithoutTryCatch.main(WithoutTryCatch.java:5)
```
🔹 The program crashes because the exception is **not handled**.

---

## Using try-catch but without `e.printStackTrace();`

If the exception is caught but we **don't use `e.printStackTrace();`**, the program **will not crash**, but we won’t see any details about the error.

```java
import java.io.FileInputStream;
import java.io.IOException;

public class TryCatchWithoutPrint {
    public static void main(String[] args) {
        try {
            FileInputStream fis = new FileInputStream("non_existent_file.txt");
        } catch (IOException e) {
            System.out.println("An error occurred!"); // Only prints a custom message
        }
    }
}
```

### Output:
```
An error occurred!
```
🔹 The error is **caught**, but we **don’t see any details** about the cause.

---

## Using `e.printStackTrace();`

If we call `e.printStackTrace();` inside `catch`, it prints **detailed error information (stack trace)**, helping us locate the issue.

```java
public class PrintStackTraceExample {
    public static void main(String[] args) {
        System.out.println("Program started");

        try {
            int result = 10 / 0; // This will cause an ArithmeticException
        } catch (ArithmeticException e) {
            e.printStackTrace(); // Prints error but does NOT stop the program
        }

        System.out.println("Program continues..."); // This line still executes
    }
}
```

### Output (The program continues even though an error occurs):
```
Program started
java.lang.ArithmeticException: / by zero
    at PrintStackTraceExample.main(PrintStackTraceExample.java:7)
Program continues...
```

🔹 **Even though an error occurs, the program continues running.**







## 1.2 Writing to a File Using Byte Streams

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamWriteExample {
    public static void main(String[] args) {
        String data = "Hello, World!";
        try (FileOutputStream fos = new FileOutputStream("output.txt")) {
            fos.write(data.getBytes());
 // Convert the string into a byte array and write it to the file
// The getBytes() method converts the string into a byte array, as FileOutputStream only accepts bytes.
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
 // Read the file line by line until there are no more lines (null)

                System.out.println(line); 
            }
        } catch (IOException e) { 
            e.printStackTrace(); 
        }  
    }
}

```

### 1.4 Writing to a File Using Character Streams

```
import java.io.BufferedWriter; 
import java.io.FileWriter;
import java.io.IOException;


public class CharStreamWriteExample {
    public static void main(String[] args) {
        String data = "Hello, World!"; 
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
            bw.write(data);
            bw.newLine();
// Write the string to the file
 // The BufferedWriter does not add a new line automatically, use bw.newLine() if needed

        } catch (IOException e) { 
            e.printStackTrace();
        }
    }
}

```

---

## 2. Java Networking

### 2.1 TCP Programming
#### what is TCP?
TCP (Transmission Control Protocol) is one of the core protocols of the Internet Protocol (IP) suite, commonly referred to as TCP/IP. It is a connection-oriented protocol that provides reliable, ordered, and error-checked delivery of data between applications over a network.

### How TCP Works?
TCP communication follows a three-step process:

**Connection Establishment (Three-Way Handshake)**

**Data Transmission**

**Connection Termination**

### 3-Way Handshake (How a TCP Connection is Established)
Before two devices communicate over TCP, they must establish a connection through a three-way handshake.

**SYN (Synchronize)**
The client sends a SYN message to the server, requesting a connection.

**SYN-ACK (Synchronize-Acknowledge)**
The server responds with a SYN-ACK, acknowledging the request.

**ACK (Acknowledge)**
The client replies with an ACK, and the connection is established.

example
```
Client → Server: SYN (I want to connect)
Server → Client: SYN-ACK (Okay, let's connect)
Client → Server: ACK (Connection established)

```
### How Data is Transmitted in TCP?
Once the connection is established:

1. The sender breaks data into packets.
2. Each packet has a sequence number to ensure the receiver can reorder them.
3. The receiver sends an ACK (Acknowledgment) for each received packet.
4. If a packet is lost, TCP retransmits it.
5. The process continues until all data is delivered.


### Connection Termination (4-Way Handshake)
To close a TCP connection, a 4-way handshake is used:

1. Client sends FIN (Finish) to indicate it wants to close the connection.
2. Server responds with ACK.
3. Server then sends FIN to confirm it is closing as well.
4. Client replies with ACK, and the connection is closed.
example
```
Client → Server: FIN (I want to disconnect)
Server → Client: ACK (Okay)
Server → Client: FIN (I am also disconnecting)
Client → Server: ACK (Goodbye)
```

### TPC programming


#### Server 

```java
import java.io.*;
import java.net.*;
public class TCPServer {
    public static void main(String[] args) {
        // Create a ServerSocket listening on port 5000
        try (ServerSocket serverSocket = new ServerSocket(5000)) {
            System.out.println("Server started, waiting for client connections...");

            while (true) {
          
                try (Socket clientSocket = serverSocket.accept();
// Accept an incoming client connection
                     
                     BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
// Create a BufferedReader to read data from the client
                     
                     PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true)) {
// Create a PrintWriter to send data to the client

                    System.out.println("Client connected: " + clientSocket.getInetAddress());
// Print the client's IP address

                    String message = in.readLine();
// Read the message sent by the client

                    System.out.println("Received message from client: " + message);
                    out.println("Server received the message: " + message);
                }
            }
        } catch (IOException e) {
            e.printStackTrace(); 
        }
    }
}

```

```
 BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
```
1. if we break this to part, we first get **clientSocket.getInputStream()**
it gets the input stream from the client socket and it is a raw byte stream, meaning it reads data as bytes (0101010 format).
It cannot directly understand text.
```
InputStream inputStream = clientSocket.getInputStream();
```
2. then we get **new InputStreamReader(clientSocket.getInputStream())**
it converts the raw byte stream into a character stream (which can read text), it also allows us to read text data from the client.

```
InputStreamReader reader = new InputStreamReader(clientSocket.getInputStream());

```
3. lastly we got **new BufferedReader(new InputStreamReader(clientSocket.getInputStream()))**
the **BufferedReader** wraps **InputStreamReader** to read data efficiently, line by line.
BufferedReader.readLine() will read a complete line instead of one character at a time.
```
BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
```

### Why do we need this conversion?
InputStream (byte-based) → InputStreamReader (character-based) → BufferedReader (efficient text reading)


#### Client

```java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) {
        // Define the server address and port number
        String serverAddress = "localhost";
 // The server is running on the same machine
        int port = 5000;
// Must match the port number of the server

        try (Socket socket = new Socket(serverAddress, port);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
// BufferedReader to read data from the server
             
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {
// PrintWriter to send data to the server
           
            out.println("Hello, Server!");
 // Send a message to the server

            String response = in.readLine();
// Read the server's response
            System.out.println("Server response: " + response);

        } catch (IOException e) {
            e.printStackTrace(); 
        }
    }
}

```
