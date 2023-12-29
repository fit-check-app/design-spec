# Backend Workflows

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

Application components communicate with a message broker. A component can produce messages and consume messages. Message producers create messages that delegate out of scope work to other components. Message consumers receive messages in a first-in-first-out (FIFO) manner and act upon then accordingly. To facilitate asynchronous work, components are often both producers and consumers.

#### API Gateway

This component is a producer and a consumer. It produces messages from the HTTP requests that it validates and delegates to an appropriate logical component. It then awaits the response from the logical component. Individual requests are tracked with a tracing ID that is part of the message it sends and will match against the same tracing ID to obtain the correct response.

Messages sent by the API gateway look like:

```yaml
request-id: <tracing ID>
request-for: <Logic Service Name>
request-from: API Gateway
metadata:
  request-timestamp: <timestamp>
  source-ip: IP Address
  authorization-required: <bool>
payload:
  requestHeaders: # 0 or more allowed
    <http-header-name>: <http-header-value>
  urlParams: # 0 or more allowed
    <urlParamName>: <urlParamValue>
  queryParams: # 0 or more allowed
    <queryParamName>: <queryParamValue>
  body: <body string>
```

Messages received by the API gateway look like:

```yaml
request-id: <tracing ID>
request-for: <API Gateway>
request-from: <Logic Service Name>
metadata:
  status: Success | Failure
  message: null | Failure reason
payload:
  responseHeaders: # 0 or more allowed
    <http-header-name>: <http-header-value>
  body: <body string>
```

#### Service Pods

These components are consumers and producers. They consume delegation requests from the API gateway and produce responses for the API gateway to answer with. They may also produce and consume intermediate messages for data access. For interactions with API gateway, the messages are identical to those above.

Messages sent to a data management pod look like:

```yaml
request-id: <tracing ID>
request-for: <Data Service Name>
request-from: <Logic Service Name>
action: <A String describing the data action>
payload: <payload string required by action>
```

- To query a resource, specify the resource to query in the `action` field and any parameters (such as filter) in the `payload` field
- To write a file to the media server, specify the action as `FileWrite` and include the encoded file data in the `payload` field

Messages received from data management pods look like:

```yaml
request-id: <tracing ID>
request-for: <Logic Service Name>
request-from: <Data Service Name>
metadata:
  status: Success | Failure
  message: null | Failure reason
dataRequested: <serialized data string>
```

- For a DB query, `dataRequested` would be the query results
- For a file retrieval, `dataRequested` would be the encoded file contents

#### Data Management Pods

These components are producers and consumers. The messages that they utilize are identical to the ones above. These pods do **not** produce or consume intermediate messages. Instead, they communicate directly with the data source that they manage.

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


