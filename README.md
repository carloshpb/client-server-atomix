# Server - Client Communication with ATOMIX

Example project to make a simple communication between a Server and a Client with ATOMIX

## Getting Started

### Prerequisites

* [Java 8](https://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html)

### Building the project


1. Open your OS's terminal;
2. Use the following bash command in the project's main folder (**_../client-server-atomix_**) to compile the project: 
   ```bash
   ./mvnw compile
   ```
3. Then use this next bash command to run the tests and download the repositories : 
   ```bash
   ./mvnw test
   ```
4. Finally, the servers and clients can be started. For that, we'll have to run the MessageServer and DistributedMapClient class, which we'll need to pass an argument list :
* MessageServer's argument list : (server's ID) (sequence of servers' address)

* DistributedMapClient's argument list : (client's ID) (server's ID that it'll connect to) (sequence of servers' address)

    For that, we'll have to run the next bash commands to show it working:

    > WARNING: Each command line should be run in a new terminal window

   ```bash
    ./mvnw exec:java -Dexec.mainClass="server_client.server.MessageServer" -Dexec.args="0 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
    
    ./mvnw exec:java -Dexec.mainClass="server_client.server.MessageServer" -Dexec.args="1 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
    
    ./mvnw exec:java -Dexec.mainClass="server_client.server.MessageServer" -Dexec.args="2 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
   ```
    
   ```bash
    ./mvnw exec:java -Dexec.mainClass="server_client.client.DistributedMapClient" -Dexec.args="0 0 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
    
    ./mvnw exec:java -Dexec.mainClass="server_client.client.DistributedMapClient" -Dexec.args="1 1 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
    
    ./mvnw exec:java -Dexec.mainClass="server_client.client.DistributedMapClient" -Dexec.args="2 2 127.0.0.1 5000 127.0.0.1 5001 127.0.0.1 5002"
   ```

## Client Side's functionality:

* The Client (_DistributedMapClient_) will start the thread from its _Main_'s thread, then create a node for itself in the Atomix cluster and make it aware of the servers on it (member-0. member-1, member-2);

* It'll creates a _Serializer_ that will recognize the Message type object and all of its attributes other than _String_ or primitives (in this case _BigInteger_);

* It'll send messages to the server with the Atomix _CommunicationService_ method **_.send()_** and wait for a _CompletableFuture_ from the server to be printed (**_System.out.println_**);
    
## Server Side's functionality

* The server (_MessageServer_) will start the thread from its _Main_'s thread, then create a node for itself in the Atomix cluster and create knowledge of the servers that are on it (member-0. Member-1, member-2), just as it did in Client, but it must have a profile (declared with **_.withProfile()_**);

* It will then start _DistributedMap_ and wait for a message from the server with the _CommunicationService_ **_.subcribe()_** method;

* The message will be validated by _MessageService_ and _MessageRepository_ (validation has been split into two parts to further organize the code and try to follow a pattern);

* Once validated, it will perform its action on _DistributedMap_ and have the result sent back to the Client as _CompletableFuture_ (within **_.subscribe()_**);

## Built With

* [ATOMIX](https://atomix.io/) - The cluster framework for building fault-tolerant distributed systems
* [Maven](https://maven.apache.org/) - Dependency Management

## Authors

**Carlos Humberto de Paula Borges**

* [LinkedIn](https://www.linkedin.com/in/carloshpb/)

* [GitHub](https://github.com/carloshpb)

## Acknowledgments

I thank my teacher [Prof.Lasaro Camargos](https://www.linkedin.com/in/lasaro/) for teaching me everything I know about distributed system development and also for answering all my questions about Threads issues. Thanks also for introducing me to this great ATOMIX framework.
