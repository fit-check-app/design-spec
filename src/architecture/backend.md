# Back end architecture overview

## Programming language

The choice of programming languages for the back end are as follows:

- [Python](https://www.python.org)
- [Go](https://go.dev)

### Python

Python will be utilized as the interface for the back end. There are many python packages that are useful for creating APIs. Frameworks like FastAPI include utilities for creating automatic interactive documentation. This documentation is sourced directly from the source code with things like Python type hints. This will make it easy to validate requests at the edge. Invalid request will never reach internal services. The edge API will supply idempotence and rate limiting to the back end services. 

### Go

Go will be utilized for the internal logical services that back the edge API. Go implements the remote procedure calls that are made by the edge API. These implementations shall be stateless. The stateless nature will be helpful for scaling the services horizontally. State of the application will be stored elsewhere. 

## API style choice

The API style is RESTful. The edge python service will provide this type of interface to the internal logical and data services. The internal services will be based on the microservices architecture. Dividing the logical parts into deployable service modules will make scaling and service uptime easier to manage. This will likely require the use of container orchestration software like Kubernetes.

