# Back end Workflows

## Service workflows

![Service Workflow](./figures/service-workflow.drawio.svg)

### HTTP requests

1) Receive API request
2) Check if requested resource exist
    - If not, respond with `NOT FOUND`
3) Check if request payload is valid
    - If not, respond with `UNPROCESSIBLE ENTITY`
4) Check if authorization is required
    - If no, proceed
    - If yes, verify authorization. Respond with `UNAUTHORIZED` if verification fails
5) Create delegation message
6) Publish delegation message and await response
    - Timeout limited to 60 seconds. Respond with `BAD GATEWAY` if timeout exceeded
7) Delegated response is received and serialized for response

### Messages

Application components communicate via [protocol buffers](https://protobuf.dev). Each message is serialized and sent to the appropriate remote component. The receiving component de-serializes the message and acts upon it. Messages are defined using the `proto3` syntax. 

#### API Gateway

This component is the edge of the back end application. It receives HTTP requests and forwards them to the appropriate service pod. When the service pod responds to the remote procedure call, the API gateway sends the response back to the client.

#### Service Pods

Service pods are the logical components of the application. They receive messages from the API gateway, process them, and return a response. Service pods may also send messages to other service pods or data management pods.

#### Data Management Pods

These components are the interface between the application and the data store. Data management pods receive messages from service pods and interact with the data store to retrieve or store data. This type of pod may seem redundant, but it is necessary to abstract the data store from the service pods. This abstraction keeps the service pods stateless and allows for manageable scaling of the data stores if needed in the future. Additionally, should data stores need to be migrated, the data management pods can be updated without affecting the service pods.

- A database interface pod would communicate directly with a database using the specific database wrapper
- A media server interface pod would communicate directly with the file store using a supported protocol

## Services

### Web Controllers

Web controllers provide an index of the available resources from the API gateway. Controllers oversee the life cycle of an API request (from reception and validation to confirmation of work done and response writing). 

#### User controller

![User Controller](./figures/app-tooling-user-controller.drawio.svg)

#### Clothing controller

![Clothing Controller](./figures/app-tooling-clothing-controller.drawio.svg)

### Logical services

![Logical Services](./figures/app-tooling-service-logic.drawio.svg)

### Data interfaces

![Data Interfaces](./figures/app-tooling-data-interface.drawio.svg)


