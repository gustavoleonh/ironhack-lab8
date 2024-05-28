# ironhack-lab8

### Introduction to the Scenario:
Participants are tasked with designing an API for an e-commerce system. The system’s requirements include managing user accounts, processing orders, and handling customer interactions efficiently.

### Tools and Resources:
Participants will use SwaggerHub, an integrated API design and documentation platform, to create their API specifications. They will be provided with access to SwaggerHub and any necessary guides on its usage.

#### API Desing

``` yml
openapi: 3.0.1
info:
  title: E-commerce API for ironhack lab 8
  description: API for managing user accounts, processing orders, and handling customer interactions.
  version: 1.0.0
servers:
  # Added by API Auto Mocking Plugin
  - description: SwaggerHub API Auto Mocking
    url: https://virtserver.swaggerhub.com/GUSTAVOLEON_1/eCommerceIronHack/1.0.0
  - url: https://api.example.com/v1
    description: Main (production) server
  - url: https://sandbox.api.example.com/v1
    description: Sandbox server for testing

paths:
  /users:
    get:
      summary: Get list of users
      description: Retrieve a list of all registered users.
      responses:
        '200':
          description: A list of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
        '500':
          description: Internal server error
    post:
      summary: Create a new user
      description: Register a new user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/NewUser'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Invalid input
        '500':
          description: Internal server error

  /users/{userId}:
    get:
      summary: Get a user by ID
      description: Retrieve details of a specific user by their ID.
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: User details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found
        '500':
          description: Internal server error
    put:
      summary: Update a user by ID
      description: Update the details of a specific user.
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateUser'
      responses:
        '200':
          description: User updated successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Invalid input
        '404':
          description: User not found
        '500':
          description: Internal server error
    delete:
      summary: Delete a user by ID
      description: Remove a user from the system by their ID.
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: User deleted successfully
        '404':
          description: User not found
        '500':
          description: Internal server error

  /login:
    post:
      summary: User login
      description: Authenticate a user and return a token.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Login successful
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
        '401':
          description: Invalid credentials
        '500':
          description: Internal server error

  /orders:
    get:
      summary: Get list of orders
      description: Retrieve a list of all orders.
      responses:
        '200':
          description: A list of orders
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Order'
        '500':
          description: Internal server error
    post:
      summary: Create a new order
      description: Place a new order.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/NewOrder'
      responses:
        '201':
          description: Order created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          description: Invalid input
        '500':
          description: Internal server error

  /orders/{orderId}:
    get:
      summary: Get an order by ID
      description: Retrieve details of a specific order by its ID.
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Order details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '404':
          description: Order not found
        '500':
          description: Internal server error
    put:
      summary: Update an order by ID
      description: Update the details of a specific order.
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateOrder'
      responses:
        '200':
          description: Order updated successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          description: Invalid input
        '404':
          description: Order not found
        '500':
          description: Internal server error
    delete:
      summary: Cancel an order by ID
      description: Cancel a specific order by its ID.
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Order cancelled successfully
        '404':
          description: Order not found
        '500':
          description: Internal server error

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          description: Unique identifier for the user
        username:
          type: string
          description: Username for the user
        email:
          type: string
          description: Email address of the user
        createdAt:
          type: string
          format: date-time
          description: Date and time when the user was created
    NewUser:
      type: object
      properties:
        username:
          type: string
          description: Username for the new user
        email:
          type: string
          description: Email address of the new user
        password:
          type: string
          description: Password for the new user
    UpdateUser:
      type: object
      properties:
        email:
          type: string
          description: Updated email address of the user
        password:
          type: string
          description: Updated password for the user
    LoginRequest:
      type: object
      properties:
        username:
          type: string
          description: Username for login
        password:
          type: string
          description: Password for login
    LoginResponse:
      type: object
      properties:
        token:
          type: string
          description: Authentication token
        user:
          $ref: '#/components/schemas/User'
          description: Authenticated user details
    Order:
      type: object
      properties:
        id:
          type: string
          description: Unique identifier for the order
        userId:
          type: string
          description: Identifier of the user who placed the order
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
          description: List of items in the order
        totalAmount:
          type: number
          format: float
          description: Total amount for the order
        status:
          type: string
          description: Current status of the order
        createdAt:
          type: string
          format: date-time
          description: Date and time when the order was created
    NewOrder:
      type: object
      properties:
        userId:
          type: string
          description: Identifier of the user placing the order
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
          description: List of items to be ordered
    UpdateOrder:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
          description: Updated list of items in the order
        status:
          type: string
          description: Updated status of the order
    OrderItem:
      type: object
      properties:
        productId:
          type: string
          description: Identifier of the product
        quantity:
          type: integer
          description: Quantity of the product
        price:
          type: number
          format: float
          description: Price of the product
```

#### Rational for desing:

##### RESTful Design:
Resource-Based: The API design revolves around resources (users, orders, and interactions) and uses standard HTTP methods to perform CRUD operations.
Statelessness: Each API request from a client contains all the necessary information for the server to fulfill that request, ensuring stateless interactions.

#####  Adherence to OpenAPI 3.0:
Using OpenAPI 3.0 ensures a standardized format for defining the API, which enhances readability, maintainability, and compatibility with tools for API documentation, testing, and client generation.

##### Separation of Concerns:
User Management: Endpoints for creating, updating, retrieving, and deleting user accounts.
Order Management: Endpoints for creating, updating, retrieving, and canceling orders.
Customer Interactions: Specific endpoints for user login and order placement to handle interactions that trigger complex workflows.

##### Clear and Comprehensive Documentation:
Each endpoint is documented with summaries, descriptions, parameters, request bodies, response codes, and corresponding schemas to ensure clarity.
Detailed error codes and descriptions help developers understand and handle different scenarios effectively.

