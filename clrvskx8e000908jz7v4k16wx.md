---
title: "What are WebSockets? How they are different from HTTP?"
seoTitle: "What are WebSockets? How they are different from HTTP?"
seoDescription: "The blog delves into HTTP's limitations for real-time applications, introducing WebSockets as a solution."
datePublished: Sat Jan 27 2024 08:10:25 GMT+0000 (Coordinated Universal Time)
cuid: clrvskx8e000908jz7v4k16wx
slug: what-are-websockets-how-they-are-different-from-http
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706341900457/5da5f72f-2f56-47db-9d6a-d4fedcdea036.jpeg
tags: websockets, http, web-development

---

Whenever you engage with a website, send an email, or simply chat with a friend, some protocols facilitate all these activities.

Let's dive into a very famous, frequently heard protocol named **HTTP.**

## HTTP (HyperText Transfer Protocol)

In simple words in this protocol, you send a request to a server and the server responds to you with a response.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706331026106/bd59a3b1-94c2-4089-9b07-169240c0a613.png align="center")

It may seem simple at first, but it's a bit more complex. Before the exchange of data, a connection pool is established between the server and the client. In technical terms, a series of **handshakes** occur between both parties. Only after these handshakes, the exchange of data is possible.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706331930480/2f44dc88-3bad-4ddb-8d7c-388529c6d065.png align="center")

After the response is sent to the client the connection is terminated. HTTP is a uni-directional and stateless protocol.

* ***Unidirectional***: It means that data flows in one direction only, from the client to the server or from the server to the client.
    
* ***Stateless***: It means that each request from a client to a server is treated as a separate, independent interaction. The server doesn't remember past requests.
    

Imagine you click on one of my blogs to read. In the background, a TCP connection is established between your browser and the server hosting the blog. Subsequently, the server receives the request, and in response, it provides the blog content. After reading the blog, if you wish to explore another one, the entire process repeats. The majority of websites use this protocol for operation.

However, the issue comes up when we want to facilitate data in real-time; a chat application is one example of this. If you were to utilize HTTP in this situation, you would have to repeatedly request the messages because each time you send or request a message, a new connection is made and we receive a response only after we make the request. You want your buddy's message to reach you right away, without requiring you to keep refreshing to see if your friend has sent you anything.

### Pooling

HTTP offers a feature called polling that helps to somewhat address this problem. In polling, the client repeatedly sends requests to the server at predetermined intervals to see if the server has responded. It is divided into two categories: **short pooling** and **long pooling**, depending on the time interval.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706333042122/f73be9aa-bbf9-474a-bb9b-b329be091b1e.jpeg align="center")

Once more using a chat application as an example, we repeatedly ask for new messages after a certain amount of time, and we continue to receive blank replies if the sender doesn't submit any new messages. This will result in the wastage of resources.

### WebSockets

Now that we've examined the features and drawbacks of HTTP, we can delve into understanding WebSockets and the benefits and features it offers over HTTP.

WebSocket is a ***persistent***, ***bidirectional***, and ***full-duplex*** connection. Don't worry, let's dissect these terms one by one.

1. ***Persistent***: A persistent connection means that once established, the connection remains open for an extended period. HTTP connections are often short-lived; they open to request data and close after receiving a response. In contrast, WebSocket allows for a persistent connection, enabling real-time communication between the client and the server.
    
2. ***Bidirectional***: It means that data can be sent in both directions— from the client to the server and from the server to the client. HTTP requests are unidirectional in the sense that the client takes the initiative to request data, and the server responds accordingly. The server does not independently send data or updates to the client without a prior request. In contrast with this, in WebSockets, once the connection is established, both the client and server can send messages independently of each other, allowing for a more interactive and real-time exchange of data.
    
3. ***Full-Duplex***: It means that data can flow in both directions simultaneously. In HTTP, communication is typically half-duplex (one way at a time). With full-duplex WebSocket, the client and server can send messages independently and concurrently without waiting for each other to finish.
    

Because of the above-mentioned functionalities WebSockets are termed to be **stateful**.

Now let's understand step by step how WebSockets work:

1. **Client Initiation:** The process begins with the client sending an HTTP request that includes an additional header. This header serves as an indication that the client proposes switching to a different protocol.
    
2. **Server Acknowledgment:** Upon receiving the client's request, if the server agrees to the protocol switch, it responds with an HTTP status code ***101***, signifying a "***Switching Protocol Response***".
    
3. **Handshake Completion:** The moment the server sends this response, the handshake process concludes, officially establishing a WebSocket connection. This connection allows both the client and server to seamlessly send and receive data over a singular connection concurrently.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706340921290/50eb7eac-7862-4e91-8348-8942b3b098c0.jpeg align="center")

Now, the purpose behind understanding the WebSocket process becomes evident, illustrating why they are particularly advantageous in real-time applications such as chat applications. Once a WebSocket connection is established between you and your friend, seamless communication becomes possible.

In the upcoming blog, we'll guide you through the process of constructing a simple chat application in Node.js using WebSockets.