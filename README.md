# Text Chat Application

## 1. Objectives

Develop the client and server components of a text chat application, consisting of one chat server and multiple chat clients over TCP connections.

## 2. Running my program

program will take 2 command line parameters:

The first parameter (s/c) indicates whether your program instance should run as a server or a client. The second parameter (number) is the port number on which your process will listen for incoming connections. In the rest of this document, this port is referred to as the listening port.

E.g., if your executable is named assignment1:

To run as a server listening on port 4322
$ ./assignment1 s 4322

To run as a client listening on port 4322
$ ./assignment1 c 4322

## 3. Detailed description

The chat application follows a typical client-server model, whereby we will have one server instance and two or more client instances. 
The clients, when launched, can log in to the server, identify themselves and obtain the list of other clients that are connected to the server. Clients can either send a unicast message to any one of the other clients or broadcast a message to all the other clients.
Note that the clients maintain an active connection only with the server and not with any other clients. Consequently, all messages exchanged between the clients must flow through the server. Clients never exchange messages directly with each other. The server exists to facilitate the exchange of messages between the clients. The server can exchange control messages with the clients. Among other things, it maintains a list of all clients that are connected to it, and their related information (IP address, port number, etc. Further, the server stores/buffers any messages destined to clients that are not logged in at the time of the receipt of the message at the server from the sender, to be delivered at a later time when the client logs in to the server.

## 4. Network and SHELL Dual Functionality

When launched (either as server or client), application works like a UNIX shell accepting specific commands (described below), in addition to performing network operations required for the chat application to work. This uses the select() system call which allows us to provide a user interface and perform network functions at the same time (simultaneously).

## 5. Server/Client SHELL Command Description

This set of commands will work irrespective of whether the application is started as a server or a client.

**IP**

Prints the IP address of this process.

**PORT**

Print the port number this process is listening on.

**LIST**

Displays a numbered list of all the currently logged-in clients. The output displays the hostname, IP address, and the listening port numbers sorted by their listening port numbers, in increasing order. E.g.,

                      1     stones.cse.buffalo.edu          4       0       logged-in
                      2     embankment.cse.buffalo.edu      3       67      logged-out
                      3     highgate.cse.buffalo.edu        7       14      logged-in
                      4     euston.cse.buffalo.edu          11      23      logged-in


## 6. Server SHELL Command/Event Description

This set of commands will work only when the application is started as a server.

**STATISTICS**

Displays a numbered list of all the clients that have ever logged in to the server (but have never executed the EXIT command) and statistics about each one. The output will display the hostname, #messages-sent, #messages-received, and the current status: logged-in/logged-out depending on whether the client is currently logged-in or not, sorted by their listening port numbers, in increasing order.

**BLOCKED <client-ip>**

Displays a numbered list of all the clients blocked by the client with IP address: <client-ip>. The output will display the hostname, IP address, and the listening port numbers, sorted by their listening port numbers, in increasing order. The output format will be identical to that of the LIST command.

## 7. Client SHELL Command/Event Description

This set of commands will work only when the application is started as a client.

**LOGIN <server-ip> <server-port>**

This command is used by a client to log in to the server located at IP address: <server-ip> listening on port: <server-port>. The LOGIN command takes 2 arguments. The first argument is the IP address of the server and the second argument is the listening port of the server.

**REFRESH**

Get an updated list of currently logged-in clients from the server.

**SEND <client-ip> <msg>**

To send message: <msg> to client with IP address: <client-ip>. <msg> can have a maximum length of 256 bytes and will consist of valid ASCII characters.

**BROADCAST <msg>**

To send message: <msg> to all logged-in clients. <msg> can have a maximum length of 256 bytes and will consist of valid ASCII characters.

**BLOCK <client-ip>**

To block all incoming messages (unicast and broadcast) from the client with IP address: <client-ip>. The client implementation should notify the server about this blocking. The server should not relay or store/buffer any messages from a blocked sender destined for the blocking client. The blocked sender, however, will be unaware of this blocking and should execute the SEND command without any error.

**UNBLOCK <client-ip>**

To unblock a previously blocked client with IP address: <client-ip>. The client will notify the server about the unblocking.

**LOGOUT**

To logout from the server. However, application will not exit and continue to accept LOGIN, EXIT, IP, PORT, and AUTHOR commands. In general, on LOGOUT all state related to this client is maintained on both the client and the server.

**EXIT**

Logout from the server (if logged-in) and terminate the application with exit code 0. This should delete all the states for this client on the server.




