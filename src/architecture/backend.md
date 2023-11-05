# Backend architecture overview

## Programming language

This candidate programming languages for the backend of the system are 

- [Python](https://www.python.org)
- [Rust](https://www.rust-lang.org)

Python has the following advantages in building web services

- Ease of Use:
    - Python is known for its simplicity and readability. It's easy to learn and has a large community of developers.
    - Python's syntax is concise and resembles natural language, which makes it accessible for developers.
- Interpreted Language:
    - Python is an interpreted language, which means it's generally slower than compiled languages like Rust.
    - It may not be the best choice for high-performance applications.
- Dynamic Typing: Python is dynamically typed, which allows for flexible and rapid development but can lead to runtime errors if not careful.
- Large Ecosystem:
    - Python has a rich ecosystem of libraries and frameworks, including Django, Flask, and FastAPI, for building web applications.
    - It's widely used for various tasks, from web development to data analysis and machine learning.
- Concurrency:
    - Python's Global Interpreter Lock (GIL) can limit its ability to perform true multi-threading and take full advantage of modern multi-core processors.
    - Asynchronous programming with libraries like asyncio can help mitigate GIL limitations.
- Community and Support: Python has a massive and active community, making it easy to find help and resources.

Rust has the following advantages in building web services

- Performance:
    - Rust is designed for high performance and memory safety. It's a systems programming language that can compete with C and C++ in terms of speed.
    - It's well-suited for building high-performance backend systems, including web servers and microservices.
- Static Typing:
    - Rust enforces strong static typing, which helps catch a wide range of errors at compile time, resulting in safer code.
    - Ownership and borrowing system ensures memory safety without a garbage collector.
- Concurrency and Parallelism:
    - Rust excels in managing concurrency and parallelism, making it suitable for building scalable and efficient backend systems.
    - It uses threads and asynchronous programming to maximize resource utilization.
- Tooling:
    - Rust has a rapidly growing ecosystem with libraries and tools that make it easier to build and maintain backend services.
    - The cargo package manager simplifies dependency management.
- Safety and Security:
    - Rust's focus on memory safety and zero-cost abstractions reduces the risk of common security vulnerabilities like buffer overflows.
    - It's a preferred choice for building systems where security is critical.
- Learning Curve:
    - Rust has a steeper learning curve compared to Python due to its unique ownership system and strict borrowing rules.
    - Developers need to invest time in mastering these concepts

## API style choices

### Monolith

- Architecture:
    - In a monolithic architecture, the entire application, including the API, is a single, self-contained unit.
    - All components are tightly integrated into a single codebase and runtime.
- Development and Deployment:
    - Development is generally easier because all code is in one place, simplifying code management and debugging.
    - Deployment can be more complex, as you need to redeploy the entire monolith even for small changes.
- Scalability: Scaling a monolithic API can be challenging. You typically need to scale the entire application, even if only one part of it requires more resources.
- Maintenance:
    - Maintenance can become complex as the codebase grows, leading to long build and test cycles.
    - Code changes can potentially introduce regressions or conflicts with other parts of the application.
- Resource Efficiency: Monolithic APIs can be resource-inefficient, as they require resources to run even for less frequently used components.

### Microservices

- Architecture: Microservices break the application into small, loosely coupled services that communicate via APIs. Each service focuses on a specific business capability.
- Development and Deployment:
    - Development is distributed across teams, with each team responsible for one or more microservices.
    - Deployment is more flexible, as you can deploy individual services independently.
- Scalability: Microservices enable fine-grained scalability, allowing you to scale specific services based on their demand.
- Maintenance: Maintenance is often easier because each service has a well-defined scope, and changes to one service are less likely to affect others.
- Resource Efficiency: Microservices can be more resource-efficient as you can allocate resources based on actual usage, resulting in cost savings.

### Serverless

- Architecture:
    - In serverless, you write individual functions or "serverless endpoints" that respond to specific events or HTTP requests.
    - The cloud provider manages the underlying infrastructure, automatically scaling as needed.
- Development and Deployment:
    - Development is focused on writing functions that perform specific tasks, making development and testing modular.
    - Deployment is simplified, as you only deploy functions, and the cloud provider handles scaling automatically.
- Scalability: Serverless is highly scalable by design. The cloud provider manages the scaling, and you pay only for the actual usage.
- Maintenance: Maintenance can be simpler, as each function is isolated from others. Changes to one function do not affect others.
- Resource Efficiency: Serverless is efficient in terms of resource utilization because resources are allocated dynamically, and you pay only for execution time.

## Helpful libraries or frameworks

### Python

- Flask:
    - Flask is a lightweight and flexible micro web framework for Python. It is excellent for building RESTful APIs.
    - It provides a simple and intuitive API for handling HTTP requests, routing, and creating endpoints.
    - Flask-RESTful and Flask-RESTPlus are extensions that simplify the development of RESTful APIs.
- Django:
    - Django is a high-level web framework that includes built-in support for creating APIs.
    - Django REST framework is a powerful and popular extension for building RESTful APIs in Django.
    - It offers features like authentication, serialization, viewsets, and automatic documentation generation.
- FastAPI:
    - FastAPI is a modern web framework that is designed for high-performance APIs with automatic interactive documentation.
    - It is asynchronous and based on Python type hints, which makes it easy to validate requests and generate API documentation.
- Tornado:
    - Tornado is an asynchronous networking library and web framework that can be used to build APIs that handle a large number of concurrent connections.
    - It is well-suited for building real-time and long-polling APIs.
- Pyramid:
    - Pyramid is a flexible and modular web framework that allows you to choose components and libraries for your API's specific needs.
    - Pyramid is highly extensible, making it suitable for various use cases.
- Bottle:
    - Bottle is a micro web framework that is minimalistic and suitable for small and simple APIs.
    - It is a single-file framework, making it easy to get started quickly.
- Falcon:
    - Falcon is a high-performance, minimalist web framework for building APIs. It is designed for speed and is particularly suitable for large-scale applications.
    - It is lightweight and emphasizes efficiency and simplicity.
- CherryPy:
    - CherryPy is a minimalistic web framework that can be used to build web APIs. It is known for its simplicity and ease of use.
    - It provides a simple and easy-to-understand API for developers.
- Sanic:
    - Sanic is an asynchronous web framework that is designed for building high-performance APIs. It uses Python's async/await syntax for asynchronous programming.
- Quart:
    - Quart is an ASGI (Asynchronous Server Gateway Interface) web framework that is compatible with the asynchronous features of Python 3.7+.
    - It is suitable for building asynchronous APIs.

### Rust

1) Actix: Actix is a powerful and high-performance actor framework for building concurrent, scalable, and resilient web services. Actix-Web is its web framework component.
2) Rocket: Rocket is a web framework designed for ease of use and developer productivity. It uses Rust's macros to declare routes and request handlers.
3) Warp: Warp is a modern and composable web framework that is known for its speed and flexibility. It's well-suited for building asynchronous web services.
4) Hyper: Hyper is a low-level HTTP library for Rust that can be used to build custom web servers and clients. It's often used as a foundation for creating web frameworks.
5) Tide: Tide is an asynchronous web framework built on top of async/await syntax. It is designed to be lightweight and easy to use.
6) Iron: Iron is a middleware-based web framework for Rust. It allows you to easily compose reusable middleware components to build web services.
7) Actix-WebSocket: This is an extension of Actix for building WebSocket-based services, which are ideal for real-time applications and interactive web features.
8) Tera: Tera is a template engine for Rust that can be used to generate dynamic HTML, JSON, or other content in your web services.
9) JWT: Rust crates like jsonwebtoken and jsonwebtoken-crate can be used for working with JSON Web Tokens (JWT) for authentication and authorization in web services.
10) R2D2: R2D2 is a connection pool crate that can be used with various database libraries to efficiently manage database connections in your web services.
11) Diesel: Diesel is a comprehensive ORM (Object-Relational Mapping) for Rust that simplifies database interaction, making it suitable for database-driven web services.
12) Serde: Serde is a highly flexible and widely used serialization and deserialization framework for Rust. It can be used to work with JSON, XML, and other data formats in your web services.
13) SQLx: SQLx is an asynchronous SQL toolkit for Rust that combines the productivity of high-level ORMs with the low-level control of SQL. It works well with modern async/await syntax.
14) Warp+Juniper: This combination of crates allows you to build GraphQL APIs using Warp and Juniper, a Rust GraphQL library.
15) Surf: Surf is a simple and lightweight HTTP client for Rust that is often used in web service clients or for making external API requests.
