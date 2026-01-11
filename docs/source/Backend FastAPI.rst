14. Backend development with FastAPI
====================================

We now begin the section on full stack development, which is the process of creating software with a user interface (frontend) and a component that
handles logic, data processing and storage, and application workflows (backend).

Here, we focus on the backend, which is the behind the scenes magic that makes an application work. There are many languages used for backend
development, such as Ruby, PHP, and Java. We will focus on using Python with the FastAPI framework. Its key features are speed (high performance),
fast to code, has fewer bugs, is easy to use, and is robust (useable for production code).

API stands for Application Programming Interface i.e. a language that allows different system components to communicate with each other smoothly.
It is a set of rules that define how one componment requests data from another, instead of having to collect and process that data itself. APIs
follow a cycle of requests and responses. The frontend makes a request to the backend API, which processes the request and sends back a response.

JSON, which stands for JavaScript Object Notation, is a lightweight data format that is often used for data exchange between a server and a web 
application. It is human-readable and easy to parse.

ASGI, which stands for Asynchronous Server Gateway Interface, provides a standard interface between asynchronous Python web servers, frameworks,
and applications. It is designed to handle long-lived connections and concurrency, making it suitable for modern web applications.

Uvicorn is a fast ASGI server implementation, designed for high speed and implementing a minimal application interface.

A port is a virtual point where network connections start and end. Ports are used to differentiate between different data types and services on a 
computer.

FastAPI and Uvicorn are both installed via pip:

.. code-block:: bash

   pip install "fastapi>=0.115" "uvicorn[standard]>=0.30"

Once we have a working app (see below) defined in the file ``main.py``, we can run it with Uvicorn:

.. code-block:: bash

   uvicorn main:app --reload

The port can be specified with the ``--port`` flag (default is 8000). The ``--reload`` flag makes the server restart after code changes, which is 
useful for development. A common error is when the port is already in use. You can view which ports are in use with the ``lsof`` command which means
"list open files":

.. code-block:: bash

   lsof -iTCP -sTCP:LISTEN -n -P

You can check if a specific port is busy using:

.. code-block:: bash

   lsof -i :8000

Once the server is running, you can access the API documentation at ``http://localhost:8000/docs``

1. Declaration
-----------------

There are two important dependencies to import when creating a FastAPI app:

.. code-block:: python

   # main.py
   from fastapi import FastAPI
   from pydantic import BaseModel

    app = FastAPI()

- ``FastAPI`` is the main class for creating the app that we instaniate with ``app = FastAPI()``.
- ``BaseModel`` is a class from the Pydantic library that is used to define data models with type hints and validation.

2. Shape of data
-------------------

The Pydantic ``BaseModel`` class is used to define the format and structure of data that the API will receive and send. This allows for type
validation (making sure the data is of the correct Python type) and conversion of compatible types. For example, say we want to define a model
for tabular data with three columns: name (string), age (integer), and height (float). We can create a class that inherits from ``BaseModel``:

.. code-block:: python

   class Person(BaseModel):
       name: str
       age: int
       height: float

This class defines a data model called ``Person`` with three attributes: ``name``, ``age``, and ``height``, along with their respective types.

3. Endpoints
---------------

An API endpoint is a specific URL that allows the API to receive requests and send responses. An endpoint is made of a resource path and an HTTP
method that describes an action, that together form a complete instruction. They are defined in Python using decorators:

.. code-block:: python

   @app.post("/person/")
   async def create_person(person: Person):
       return person

In this example, we define a POST endpoint at the path ``/person/``. The function ``create_person`` takes a parameter ``person`` of type ``Person``,
which is the data model we defined earlier. The function simply returns the received ``person`` data. If we send a POST request to ``/person/`` with a 
JSON payload that matches the ``Person`` model, FastAPI will validate the data and return it in the response. If the data does not match the model
we have defined, FastAPI will return an 422 error with details.

4. Ports
-----------

A port is made of four parts:

- Protocol: The communication protocol used (e.g. HTTP, HTTPS).
- Domain: The domain name or IP address of the server (e.g. localhost, 127.0.0.1 are both loopback addresses meaning "this computer").
- Port: The port number on which the server is listening (e.g. 8000, which is the default for Uvicorn).
- Path: The specific endpoint or resource path (e.g. /person/).

5. Extras
------------

FastAPI provides several additional features that enhance the functionality of the API:
- Automatic interactive API documentation using the Swagger UI and ReDoc.
- Dependency injection: Automatically handles dependency hierarchies.
- Security and authentication defined in OpenAPI, including HTTP Basic, OAuth2, and API keys.
- WebSocket support from Starlette.

6. Endpoint types
--------------------

FastAPI supports eight main HTTP "methods" for endpoints:

- GET: Requests a representation of a specified resource.
- POST: Submits an entity to the specified resource, often resulting in a change of state or side effects on the server.
- PUT: Replaces all current representations of the target resource with the request content.
- DELETE: Deletes the specified resource.
- PATCH: Applies partial modifications to a resource.
- HEAD: Asks for a response identical to a GET request, but without the response body.
- OPTIONS: Describes the communication options for the target resource.
- TRACE: Performs message loop-back test along the path to the target resource.

Idempotent means that an operation can be applied multiple times without changing the result beyond the initial application. In HTTP methods,
this means that making the same request multiple times will have the same effect as making it once. From the above list, only POST and PATCH are
not idempotent.

7. Status codes
---------------

HTTP status codes are three digit numbers that indicate whether a specific HTTP request has been successfully completed. There are five classes:

- 1xx (Informational): Request received, continuing process.
- 2xx (Successful): The request was successfully received, understood, and accepted.
- 3xx (Redirection): Further action needs to be taken in order to complete the request.
- 4xx (Client Error): The request contains bad syntax or cannot be fulfilled.
- 5xx (Server Error): The server failed to fulfil an apparently valid request.

FastAPI automatically handles the following status codes:

- 200 OK: The request has succeeded.
- 201 Created: The request has been fulfilled and resulted in a new resource being created.
- 204 No Content: The server successfully processed the request, but is not returning any content.
- 400 Bad Request: The server cannot or will not process the request due to a client error.
- 401 Unauthorized: The request requires user authentication.
- 403 Forbidden: The server understood the request, but refuses to authorize it.
- 404 Not Found: The server has not found anything matching the Request-URI.
- 422 Unprocessable Entity: The server understands the content type of the request entity, and the syntax of the request entity is correct, but was unable to process the contained instructions.
- 500 Internal Server Error: The server encountered an unexpected condition which prevented it from fulfilling the request.