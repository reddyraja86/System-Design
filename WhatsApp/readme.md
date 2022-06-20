## Whats App System Design 

### Functional Requirements

- User Registration
- One on one chatting
- Group chat
- Message status on App

### Non functional Requirements

- Availability
- Scalable
- Latency
- Fault tolerent
- No single point of failyre



![alt text](WhatsApp-SystemDesign.png?raw=true)

### Execution flow

- User A registers with whats app application by sending a http request to user registration microaervice behind the loadbalancer and api gateway.
- User Details like mobile number,email Id,password are validated and saved in database.
- once the user is registered,Send the message on to the user regisrtration topic.
- 

### Message format

### Apis required

### Database tables 
