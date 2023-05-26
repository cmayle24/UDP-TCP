# UDP-TCP Sockets

* Small overview of networking and network models
* Explain TCP and UDP protocols and go over their simularities/differences
* Give an example of both using Qt C++

## Breif Overview of Networking 

## UDP:

UDP is a connectionless protocol, meaning it doesn't establish a direct connection between the sender and receiver before sending data.
It provides a simple and lightweight method of communication, without the overhead of establishing and maintaining a connection.
UDP is often used for applications where speed and efficiency are more important than guaranteed delivery or reliability.
It does not guarantee that the data will reach the destination or arrive in the correct order.
UDP is commonly used for real-time streaming, online gaming, DNS queries, and other applications where slight delays or occasional data loss are acceptable.

### UDP Socket:

A UDP socket is a programming interface that allows applications to send and receive UDP datagrams.
It operates on the UDP protocol and provides an endpoint for communication between devices.
UDP sockets are connectionless, meaning you can send packets to any destination without establishing a connection first.
They are often used for applications that require low latency and can tolerate some data loss, such as real-time streaming or multiplayer gaming.
In programming, UDP sockets can be created using libraries or APIs, like the Berkeley Sockets API in C/C++ or the socket module in Python.

## TCP:

TCP is a connection-oriented protocol, which means it establishes a reliable connection between the sender and receiver before data transmission.
It provides reliable, ordered, and error-checked delivery of data packets.
TCP ensures that data packets are received in the correct order and can retransmit lost or corrupted packets.
It also includes mechanisms for flow control to manage the rate at which data is transmitted.
TCP is typically used for applications that require guaranteed delivery and reliable communication, such as web browsing, file transfers, email, and other applications where data integrity is crucial.
Now, let's dive into sockets, which are the programming interfaces used for network communication using UDP and TCP:

### TCP Socket:

A TCP socket is a programming interface that enables applications to establish a reliable, bidirectional connection and exchange data over a TCP connection.
It operates on the TCP protocol and provides an endpoint for communication.
TCP sockets use a three-way handshake to establish a connection between the sender and receiver before data transmission.
They provide reliable, ordered, and error-checked delivery of data packets, ensuring that data is received correctly and in the correct order.
TCP sockets are commonly used for applications that require guaranteed delivery and data integrity, such as web browsing or file transfers.
Like UDP sockets, TCP sockets can be created using libraries or APIs in different programming languages.

The OSI (Open Systems Interconnection) model and the TCP/IP (Transmission Control Protocol/Internet Protocol) model are both conceptual frameworks that define the functions and protocols used in computer networks. They help in understanding how different network protocols and technologies work together to enable communication between devices. Let's explore each model in more detail:

OSI Model:
The OSI model is a theoretical framework that standardizes network communication into seven layers. Each layer has specific functions and protocols that operate within that layer. The layers, from bottom to top, are as follows:

Physical Layer: This layer deals with the physical transmission of data over the network, including electrical, optical, or radio signals. It defines characteristics such as voltage levels, cable types, and connector specifications.

Data Link Layer: The data link layer manages the reliable transmission of data frames between adjacent nodes over a physical link. It handles error detection and correction, flow control, and data framing. Ethernet is a common protocol used in this layer.

Network Layer: The network layer focuses on the logical addressing and routing of data packets across multiple networks. It determines the best path for data transmission and performs IP addressing and routing. The Internet Protocol (IP) is the primary protocol used in this layer.

Transport Layer: This layer ensures reliable and error-free end-to-end delivery of data between hosts. It manages the segmentation and reassembly of data, as well as flow control and error recovery. TCP and UDP operate in this layer.

Session Layer: The session layer establishes, manages, and terminates communication sessions between applications. It allows synchronization and checkpointing of data exchange, as well as handling session recovery.

Presentation Layer: The presentation layer deals with data representation, encryption, and compression. It ensures that data exchanged between systems is in a format that the receiving system can understand.

Application Layer: The application layer provides a platform for application-specific services and protocols, such as HTTP for web browsing, FTP for file transfer, and SMTP for email. It is the layer closest to the end-user.

TCP/IP Model:
The TCP/IP model is a practical implementation of network protocols used on the internet. It is based on four layers, which are sometimes mapped to the OSI model for comparison:

Network Interface Layer (equivalent to OSI Physical and Data Link Layers): This layer corresponds to the physical and data link layers in the OSI model. It handles the physical transmission of data and manages the communication over the local network. Ethernet, Wi-Fi, and other network-specific protocols operate at this layer.

Internet Layer (equivalent to OSI Network Layer): The internet layer corresponds to the network layer in the OSI model. It handles IP addressing, routing, and the fragmentation and reassembly of data packets. The Internet Protocol (IP) operates at this layer.

Transport Layer (equivalent to OSI Transport Layer): This layer corresponds to the transport layer in the OSI model. It provides reliable and ordered delivery of data between hosts. The Transmission Control Protocol (TCP) ensures reliable and connection-oriented communication, while the User Datagram Protocol (UDP) provides unreliable and connectionless communication.

Application Layer (equivalent to OSI Session, Presentation, and Application Layers): The application layer corresponds to multiple layers in the OSI model. It provides application-specific protocols and services for end-user applications, including HTTP, FTP, SMTP, and many others.

In summary, the OSI model and the TCP/IP model are conceptual frameworks that help understand the layers and protocols involved in computer networks. The OSI model consists of seven layers, while the TCP/IP model comprises four layers, with each layer serving specific functions and protocols. The TCP/IP model is widely used in practical networking, especially on the internet.

UDP (User Datagram Protocol) Examples:

Video Streaming: UDP is frequently used for real-time video streaming applications. Services like YouTube, Netflix, and Twitch utilize UDP for delivering live video streams to users. While UDP does not guarantee the delivery of every packet, the real-time nature of video streaming allows for occasional data loss without significantly impacting the user experience.

VoIP (Voice over IP): UDP is well-suited for VoIP applications, such as voice and video conferencing services like Skype or Zoom. Real-time communication requires low latency, and the loss of a few packets may not be noticeable in a conversation. UDP's speed and simplicity make it a preferred choice for these applications.

DNS (Domain Name System): UDP is used in DNS queries, where a client sends a request to a DNS server to translate domain names into IP addresses. DNS operates on UDP port 53, and UDP's lightweight nature allows for quick resolution of domain names.

TCP (Transmission Control Protocol) Examples:

Web Browsing: When you browse the internet and request a webpage, TCP is used for establishing a reliable connection between your device and the web server. TCP ensures that the webpage data is transmitted accurately, with error detection and retransmission of any lost packets, guaranteeing reliable delivery of web content.

File Transfer: TCP is commonly used for file transfer protocols like FTP (File Transfer Protocol) and SFTP (Secure File Transfer Protocol). These protocols require the transfer of large files with guaranteed integrity, and TCP's reliability and error correction mechanisms ensure that the files are transferred accurately.

Email: When you send or receive emails using protocols like SMTP (Simple Mail Transfer Protocol) or IMAP (Internet Message Access Protocol), TCP is employed for reliable and ordered delivery of email messages. It ensures that email data is transmitted without loss or corruption and arrives in the correct order.

Remote Desktop: Applications like Remote Desktop Protocol (RDP) or Virtual Network Computing (VNC) use TCP for remote access to computers. These protocols need a reliable and accurate transmission of keyboard, mouse, and screen updates between the local and remote machines.

UDP Example:

```c++
#include <QUdpSocket>
#include <QHostAddress>

int main()
{
    QUdpSocket socket;
    
    // Bind the socket to a specific port for receiving UDP datagrams
    socket.bind(QHostAddress::Any, 1234);
    
    // Connect a signal to handle incoming UDP datagrams
    QObject::connect(&socket, &QUdpSocket::readyRead, [&socket]() {
        while (socket.hasPendingDatagrams()) {
            QByteArray datagram;
            datagram.resize(socket.pendingDatagramSize());
            
            QHostAddress senderAddress;
            quint16 senderPort;
            
            socket.readDatagram(datagram.data(), datagram.size(), &senderAddress, &senderPort);
            
            // Process the received UDP datagram
            // ...
        }
    });
    
    // Send a UDP datagram to a specific destination
    QByteArray datagram = "Hello, UDP!";
    QHostAddress receiverAddress = QHostAddress::LocalHost;
    quint16 receiverPort = 5678;
    
    socket.writeDatagram(datagram, receiverAddress, receiverPort);
    
    return 0;
}
```
 
TCP Example:

```c++
#include <QTcpSocket>
#include <QHostAddress>

int main()
{
    QTcpSocket socket;
    
    // Connect to a TCP server
    socket.connectToHost(QHostAddress::LocalHost, 1234);
    
    if (socket.waitForConnected()) {
        // Send data to the server
        QByteArray data = "Hello, TCP!";
        socket.write(data);
        
        // Wait for data to be ready for reading
        if (socket.waitForReadyRead()) {
            // Read the received data from the server
            QByteArray receivedData = socket.readAll();
            
            // Process the received TCP data
            // ...
        }
    }
    
    // Disconnect from the server
    socket.disconnectFromHost();
    
    return 0;
}
```

Unicast:

Unicast is a communication method where data is sent from a single sender to a specific receiver.
In unicast communication, the data is addressed to a specific destination IP address, typically the unique IP address of the intended receiver.
The data is transmitted over the network, and only the intended recipient with the specified IP address receives and processes the data.
Unicast communication is a one-to-one communication model, where each communication session involves one sender and one receiver.
Multicast:

Multicast is a communication method where data is sent from a single sender to a group of recipients.
In multicast communication, the data is addressed to a specific multicast group IP address, which represents a group of receivers.
The sender transmits a single copy of the data over the network, and the network infrastructure replicates and delivers the data to all members of the multicast group.
Multicast communication is a one-to-many communication model, where a single sender can communicate with multiple recipients simultaneously.
Multicast is commonly used for applications such as video streaming, online gaming, and software updates, where the same data needs to be delivered to multiple recipients efficiently.
Broadcast:

Broadcast is a communication method where data is sent from a single sender to all devices within a specific network segment.
In broadcast communication, the data is addressed to a special IP address known as the broadcast address, which represents all devices on the local network.
The data is transmitted over the network, and every device within the network segment receives and processes the data.
Broadcast communication is a one-to-all communication model, where a single sender communicates with all devices on the local network.
Broadcast is typically used for network discovery protocols, such as Address Resolution Protocol (ARP), where a device needs to obtain the MAC address corresponding to a specific IP address within its local network segment.

In summary, unicast is one-to-one communication, multicast is one-to-many communication, and broadcast is one-to-all communication. Each method has its own use cases and considerations based on the specific requirements of the network application or protocol being used.

The three-way handshake is a method used by TCP (Transmission Control Protocol) to establish a reliable connection between a client and a server. It involves a series of steps to synchronize and negotiate parameters before data transmission can begin. Here's an overview of the three-way handshake process:

Step 1: SYN (Synchronize)
The client initiates the connection by sending a TCP segment with the SYN (synchronize) flag set to the server.
This segment contains a randomly generated sequence number (SEQ) to establish the initial sequence number for the connection.
The client also selects an initial value for the receive window, which represents the amount of data it can receive at a time.
Step 2: SYN-ACK (Synchronize-Acknowledge)
Upon receiving the SYN segment, the server responds by sending a TCP segment with both the SYN and ACK (acknowledge) flags set.
The SYN-ACK segment acknowledges the receipt of the client's SYN segment and also synchronizes the sequence numbers.
The server generates its own sequence number (SEQ) and acknowledges the client's sequence number (ACK).
Step 3: ACK (Acknowledge)
Finally, the client acknowledges the receipt of the server's SYN-ACK segment by sending an ACK segment.
The ACK segment has the ACK flag set and acknowledges the server's sequence number.
At this point, the connection is established, and both the client and server can begin exchanging data.
It's important to note that the sequence numbers are crucial for reliable data transmission and ensuring proper ordering of segments. They help detect and recover from any lost or duplicate segments during the communication.

The three-way handshake allows the client and server to agree on initial sequence numbers, confirm each other's ability to receive data, and synchronize their communication parameters. This process establishes a reliable and bidirectional connection before any actual data transmission occurs.
