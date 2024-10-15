For this assignment we would be choosing the Real-time chat application.

### Section 1: REST and GraphQL for Data Requests and Updates

**REST API** 
The chat application would consist of many tables including at least, there may be more tables as the application is developed further: 
- users: this table will contain all user data that registers to the chat application. 
- agents: all the agents that can be available to chat with the users.
- messages: this will contain the messages and have a sender_id field to indicate the sender corresponding to the user or agent.
- conversations: this will have the message_id, user_id and agent_id.
- attachments: this table will have the attachments such as pictures or videos.

With the tables defined, there will be many REST API endpoints. All the endpoints start with root for the url:
- GET root/ : this will return the content of main page of the application.
- GET root/conversations/<agent_id> : this will return the list of all conversations that the agent has been in.
- GET root/conversations/<user_id> : returns the list of conversations that the user has done.
- GET root/messages/<id> : returns the message that has the specific id.
- GET root/messages/<user_id> : returns the list of messages that the user has.
- POST root/conversations/ : this will contain a body in the request, with the message text, the user_id or agent_id that sent the message. This will create the conversation with a new unique conversation_id
- POST root/users/ : this will contain a body, with the user data. This will create the user with a new unique user_id
- POST root/agents/ : this will contain a body, with the agent data. This will create the agent with a new unique agent_id
- POST root/attachments/ : this will contain a body, with the attachment data. This will create the attachment with a new unique attachment_id
- PUT root/conversations/<conversation_id> : this will contain the body with the fields that the system would edit (for example to edit the message there would be a "message" field in the body)
- PUT root/users/<user_id> : this will contain the body with the fields that the system would edit
- PUT root/agents/<agent_id> : this will contain the body with the fields that the system would edit
- PUT root/messages/<message_id> : this will contain the body with the fields that the system would edit
- PUT root/conversations/<conversation_id> : this will contain the body with the fields that the system would edit
- DEL root/users/<user_id> : this will delete the user with the user_id
- DEL root/agents/<agent_id> : this will delete the agent with the agent_id
- DEL root/attachments/<attachment_id_id> : this will delete the attachment with the attachment_id
- DEL root/conversations/<conversation_id> : this will delete the user with the user_id. This will also delete any message associated with it.
- DEL root/messages/<message_id> : this will delete the message with the message_id. This will also update the conversation that the message has been deleted.

**GraphQL**:
## Entities (Tables in GraphQL Schema)

- **Users**: Contains all user data that registers to the chat application.
- **Agents**: Contains data for agents that can chat with users.
- **Messages**: Contains the messages, with a `sender_id` field to indicate the sender corresponding to the user or agent.
- **Conversations**: Contains `message_id`, `user_id`, and `agent_id` to define chat conversations.
- **Attachments**: Contains file attachments (such as pictures or videos).

## Queries

GraphQL allows fetching of specific data with a single endpoint. Below are queries that can be used in place of your REST endpoints:

### Query: Get Main Page Content
```graphql
query {
  getMainPage {
    title
    description
  }
}
```

**Comparison**:
- In GraphQL, unlike REST API where each resource typically has its own endpoint, all operations (queries, mutations, and subscriptions) are handled through a single endpoint. This means that clients send queries or mutations to this single endpoint, and the server interprets the query and fetches exactly what is requested.

**REST API**
Pros:
- Easy to manage the endpoints with all the different endpoint options that we have designed and can be changed easily or upgrade to fit more functionalities in.
- Easy to document since it is devided into endpoints and we would be using a tool called OpenAPI that we can define the endpoints, the body of the request as well as expected result. It will also generate a page that lets us test the API by simply inputting all the information required for the request.
- REST API has stateless design making it easy to scale the chat application horizontally by replicating servers without worrying about session state.
- There are many tools and supported library that will help us in developing and maintaining the REST API for the application.

Cons:
- Inefficient when dealing with complex data fetching because it sometimes fetches more data than needed.
- May require many requests to fetch all the information for related data so we must design the endpoints to accommodate as the functionalities are required.
- Maintaining different versions of APIs can become complex.
- REST API is not designed for real-time updates, we would require polling the server continuously to check for new messages.

- **GraphQL**
Pros:
- The client can specify exactly what data it needs, preventing over-fetching which minimizes over fetching like mentioned with REST API.
- GraphQL allows you to fetch related data in a single request instead of making multiple API calls.
- It provides strong schema definition which is self-documenting, making it easier to manage, understand, and debug API requests.
- With GraphQL subscriptions the chat application can get real-time updates with notification of new messages or changes to conversations without needing to poll the server.


Cons:
- More complex to implement compared to REST as it requires more complex setup and configuration for defining the schema, resolvers, and handling requests
- Clients can query different fields and combinations of data so it becomes harder to cache GraphQL queries effectively
- Steeper learning curve for developers, and over-querying can lead to performance issues if not optimized.

---

### Section 2: WebSockets for Real-time Communication

- In a real-time chat application, WebSockets can be used for handling continuous communication server and the client. 
- When a client (user or agent) connects to the chat server a WebSocket connection is established. 
- This connection remains open allowing both the server and the client to send and receive messages instantly without the need for repeated HTTP requests.
- When a user sends a message, the WebSocket connection pushes the message to the server which then broadcasts it to other participants in the conversation (e.g., another user or agent).
- This real-time delivery is crucial for a smooth chat experience. 
- Additionally, WebSockets can handle other real-time events like notifications of a new user joining a conversation ensuring users stay informed of the latest activity in the chat.

**How WebSockets Differ from REST and GraphQL for Real-time Data Flow**
- WebSockets allow continuous communication making them more efficient for real-time applications like chat. REST would require constant polling to check for new messages.
- Lower latency than REST or GraphQL because once the connection is established, messages can be sent instantly.
- While GraphQL has subscriptions to enable real-time updates it still relies on WebSockets under the hood for real-time capabilities. Native WebSocket implementations is simpler and more focused on real-time communication.

### Section 3: Technology Recommendation and Justification
## Recommendation
**REST API**: 
- We can use REST API for data management and connection to the database. 
- It can be used for operations like user management, retrieving chat data and handling attachments.
- REST APIs can be easily structured with CRUD operations (GET, POST, PUT, DELETE) for these non-real-time actions.

**GraphQL**:
- GraphQL can be used in the case of needing flexibility in fetching specific data since it can reduce overfetching and only fetch the data that is required.
- It can query for exactly what is needed in a single request would reduce the number of round trips and payload size.

**WebSockets**:
- WebSocket can be used for the messaging functionality because it provides persistent low-latency connection between the client and the server.
- Messages can be delivered instantly to all participants without the overhead of repeated requests.

**Performance**:
- WebSockets handle the continuous flow of messages in real-time, reducing latency significantly compared to REST or GraphQL for messaging. 
- REST (or GraphQL) handles non-real-time tasks efficiently, separating the workload between data storage and real-time messaging.
  
**Scalability**: 
- Both WebSockets and REST scale well with the support they have, with REST we can write more endpoints as more functionalities are required. 
- WebSockets can handle open connections simultaneously so it can provide scalability.

**Development**:
- **REST** is easy to implement and well-documented, making it a good choice for developers familiar with traditional API development. 
- **GraphQL** is more flexible but introduces some complexity. If the data requirements are simple, REST remains the easier option, but GraphQL could add value when querying complex, nested data.

### Citation and Disclosure of AI Tools
For this assignment since I have had a background in technology as a back-end developer I have had hands-on development experience and REST API specificially, which is how I incorporated my development knowledge with what I have learned in the class CST8916 (Remote Data and RT Applications) and CST8915 (Full-stack Cloud-native Development) at Algonquin College. The use of AI (ChatGPT) was employed as is permitted by the professor Ramy Mohammed, and it was used to make the format for the markdown file (.md) and brainstorm some ideas for the relevant application of GraphQL and WebSocket on the requirement of the real time online chat application, which has been used only for generating the rough draft for the understanding on the subject. I have gone through to double check everything, write in my own words and understanding as well as adding my own knowledge on the subject matter.