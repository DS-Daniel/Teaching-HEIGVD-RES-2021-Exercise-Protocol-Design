# Protocol Specification for a simple calculator

Specification definition of exchanges between clients and server for simple calculation requests.
A client can ask for an opertation between two numbers and the server should give back the result of the operation.

## What transport protocol do we use?
TCP protocol.

## How does the client find the server (addresses and ports)?
He need to know the client IP address and can use a port that is not in use (not a common port). 

## Who speaks first?
The client ask for a connection to the server.

## What is the sequence of messages exchanged by the client and the server? (flow)
```
Client   |   Server
-------------------
Hello   ->
        <- Op list
Request -> 
        <- Response 
...
Bye     ->
        <- ACK END
```

## What happens when a message is received from the other party? (semantics)
**_Server :_**

Message read (line by line) until the end
Message analysed and controled. If it's a valid request, send :
* ERROR msg if request not valid
* list of operations availables if client say Hello
* response with OK number if operation request
* ACK END if end of connection is asked


**_Client:_**

Message read (line by line) until the end
Message analysed and displayed

## What is the syntax of the messages? How we generate and parse them? (syntax)
In the calcator contexte, simple message are used, with space as token separator for parsing.
Here is the syntax used for communication between client and server : 

```
Request syntax  : [Operation] [number] [number] | [Hello] | [Bye]
Response syntax : [OK] [number] | [ERROR] [msg not valid] | [ACK END]

Exchange exemple:

ADD 22 4  ->
          <- OK 26
SUB 6 2   ->          
          <- OK 4
Life 42   ->          
          <- Error not valid
```         

## Who closes the connection and when?
The client send a close connection message (Bye) to the server when he has finished what he wants to do.
Then the serveur will return in standy mode.
