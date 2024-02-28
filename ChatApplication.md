# Real-Time Chat Application Guide
![image](https://github.com/Academixedu/Projects/assets/43459668/591510bc-19b1-4c36-81a9-f99bcefc7d61)


This document outlines the development of a real-time chat application using Java (Spring Boot) for the backend, React for the frontend, and MongoDB for data storage. The application facilitates real-time messaging among users.

## Backend (Spring Boot)

### Project Setup
- Use [Spring Initializr](https://start.spring.io/) to generate a Spring Boot project.
- Dependencies: Spring Web, Spring Data MongoDB, and Spring WebSocket.

### MongoDB Configuration
- Configure MongoDB in `src/main/resources/application.properties`.
spring.data.mongodb.uri=mongodb://localhost:27017/chat

vbnet
Copy code

### Model Creation
- Define a `Message` model to store user messages.
```java
@Document
public class Message {
    @Id
    private String id;
    private String sender;
    private String content;
    private Date timestamp;
    // Getters and Setters
}
Repository Interface
Create a repository interface for the Message model.
java
Copy code
public interface MessageRepository extends MongoRepository<Message, String> {
}
WebSocket Configuration
Configure WebSocket in Spring Boot to enable real-time communication.
java
Copy code
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/chat").withSockJS();
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic");
        registry.setApplicationDestinationPrefixes("/app");
    }
}
Message Handling
Implement a controller to handle sending and receiving messages.
java
Copy code
@Controller
public class ChatController {
    @MessageMapping("/message")
    @SendTo("/topic/messages")
    public Message sendMessage(Message message) {
        return message;
    }
}
Frontend (React)
Project Setup
Create a new React app.
lua
Copy code
npx create-react-app chat-frontend
Implementing Chat UI
Develop a simple chat interface to display and send messages.
jsx
Copy code
import React, { useState, useEffect } from 'react';
import SockJS from 'sockjs-client';
import { Stomp } from '@stomp/stompjs';

function Chat() {
    const [messages, setMessages] = useState([]);
    const [newMessage, setNewMessage] = useState("");

    useEffect(() => {
        const socket = new SockJS('/chat');
        const stompClient = Stomp.over(socket);

        stompClient.connect({}, function(frame) {
            stompClient.subscribe('/topic/messages', function(message) {
                setMessages(prevMessages => [...prevMessages, JSON.parse(message.body)]);
            });
        });

        return () => {
            stompClient.disconnect();
        };
    }, []);

    const sendMessage = () => {
        const message = { sender: "User", content: newMessage };
        stompClient.send("/app/message", {}, JSON.stringify(message));
        setNewMessage("");
    };

    return (
        <div>
            <input type="text" value={newMessage} onChange={e => setNewMessage(e.target.value)} />
            <button onClick={sendMessage}>Send</button>
            {messages.map((msg, index) => (
                <div key={index}>{msg.sender}: {msg.content}</div>
            ))}
        </div>
    );
}
Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app.
sql
Copy code
npm start
