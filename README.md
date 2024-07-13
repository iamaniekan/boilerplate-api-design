### Database Schema Documentation

#### **Users Table**

- **Table Name**: users
- **Description**: Stores user information.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `unique_id`: `varchar`
  - `first_name`: `varchar`
  - `last_name`: `varchar`
  - `phone`: `varchar`
  - `email`: `varchar` (Unique)
  - `password`: `varchar`
  - `role`: `boolean`
  - `created_at`: `datetime`
  - `updated_at`: `datetime`

#### **Transactions Table**

- **Table Name**: transactions
- **Description**: Stores transaction details.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `user_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `payment_data`: `json`
  - `created_at`: `datetime`

#### **Organisations Table**

- **Table Name**: organisations
- **Description**: Stores organisation details.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `name`: `varchar`
  - `description`: `varchar`
  - `created_at`: `datetime`
  - `updated_at`: `datetime`

#### **User_Organisation Table**

- **Table Name**: user_organisation
- **Description**: Maps users to organisations.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `user_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `organisation_id`: `int` (Foreign Key referencing `organisations.id`)

#### **Notifications Table**

- **Table Name**: notifications
- **Description**: Stores user notifications.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `user_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `message_data`: `json`
  - `read_status`: `varchar`
  - `created_at`: `datetime`

#### **Posts Table**

- **Table Name**: posts
- **Description**: Stores user posts.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `title`: `varchar`
  - `body`: `text`
  - `image`: `varchar`
  - `status`: `varchar`
  - `author_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `created_at`: `datetime`
  - `updated_at`: `datetime`

#### **Messages Table**

- **Table Name**: messages
- **Description**: Stores user messages.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `sender_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `receiver_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `subject`: `varchar`
  - `body`: `text`
  - `sent_at`: `datetime`

#### **Payments Table**

- **Table Name**: payments
- **Description**: Stores payment details.
- **Fields**:
  - `id`: `int` (Primary Key, Auto Increment)
  - `user_id`: `varchar` (Foreign Key referencing `users.unique_id`)
  - `user_data`: `json`
  - `amount`: `decimal`
  - `payment_method`: `varchar`
  - `status`: `varchar`
  - `created_at`: `datetime`

---

### API Design Documentation

#### **Users API**

- **Endpoint**: `/users`
- **Description**: Manage users.
- **Methods**:

  ##### `POST /users`
  - **Description**: Create a new user.
  - **Request Body**:
    ```json
    {
      "unique_id": "string",
      "first_name": "string",
      "last_name": "string",
      "phone": "int",
      "email": "string",
      "password": "string",
      "role": "bool"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "unique_id": "string",
        "first_name": "string",
        "last_name": "string",
        "phone": "int",
        "email": "string",
        "password": "string",
        "role": "bool",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /users`
  - **Description**: Get a list of users.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        }
      ]
      ```

  ##### `GET /users/{id}`
  - **Description**: Get a user by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "unique_id": "string",
        "first_name": "string",
        "last_name": "string",
        "phone": "int",
        "email": "string",
        "password": "string",
        "role": "bool",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "User not found"
      }
      ```

  ##### `PUT /users/{id}`
  - **Description**: Update a user by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Request Body**:
    ```json
    {
      "unique_id": "string",
      "first_name": "string",
      "last_name": "string",
      "phone": "int",
      "email": "string",
      "password": "string",
      "role": "bool"
    }
    ```
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "unique_id": "string",
        "first_name": "string",
        "last_name": "string",
        "phone": "int",
        "email": "string",
        "password": "string",
        "role": "bool",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "User not found"
      }
      ```

  ##### `DELETE /users/{id}`
  - **Description**: Delete a user by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "User not found"
      }
      ```

---

#### **Transactions API**

- **Endpoint**: `/transactions`
- **Description**: Manage transactions.
- **Methods**:

  ##### `POST /transactions`
  - **Description**: Create a new transaction.
  - **Request Body**:
    ```json
    {
      "user_id": "string",
      "payment_data": {
        "id": "int",
        "user_id": "string",
        "user_data": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "amount": "decimal",
        "payment_method": "string",
        "status": "string",
        "created_at": "datetime"
      }
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "user_id": "string",
        "payment_data": {
          "id": "int",
          "user_id": "string",
          "user_data": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "amount": "decimal",
          "payment_method": "string",
          "status": "string",
          "created_at": "datetime"
        },
        "created_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /transactions`
  - **Description**: Get a list of transactions.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "user_id": "string",
          "payment_data": {
            "id": "int",
            "user_id": "string",
            "user_data": {
              "id": "int",
              "unique_id": "string",
              "first_name": "string",
              "last_name": "string",
              "phone": "int",
              "email": "string",
              "password": "string",
              "role": "bool",
              "created_at": "datetime",
              "updated_at": "datetime"
            },
            "amount": "decimal",
            "payment_method": "string",
            "status": "string",
            "created_at": "datetime"
          },
          "created_at": "datetime"
        }
      ]
      ```

  ##### `GET /transactions/{id}`
  - **Description**: Get a transaction by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "user_id": "string",
        "payment_data": {
          "id": "int",
          "user_id": "string",
          "user_data": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "amount": "decimal",
          "payment_method": "string",
          "status": "string",
          "created_at": "datetime"
        },
        "created_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Transaction not found"
      }
      ```

  ##### `DELETE /transactions/{id}`
  - **Description**: Delete a transaction by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "Transaction not found"
      }
      ```

---

#### **Organisations API**

- **Endpoint**: `/organisations`
- **Description**: Manage organisations.
- **Methods**:

  ##### `POST /organisations`
  - **Description**: Create a new organisation.
  - **Request Body**:
    ```json
    {
      "name": "string",
      "description": "string"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "name": "string",
        "description": "string",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /organisations`
  - **Description**: Get a list of organisations.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id

": "int",
          "name": "string",
          "description": "string",
          "created_at": "datetime",
          "updated_at": "datetime"
        }
      ]
      ```

  ##### `GET /organisations/{id}`
  - **Description**: Get an organisation by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "name": "string",
        "description": "string",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Organisation not found"
      }
      ```

  ##### `PUT /organisations/{id}`
  - **Description**: Update an organisation by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Request Body**:
    ```json
    {
      "name": "string",
      "description": "string"
    }
    ```
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "name": "string",
        "description": "string",
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Organisation not found"
      }
      ```

  ##### `DELETE /organisations/{id}`
  - **Description**: Delete an organisation by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "Organisation not found"
      }
      ```

---

#### **User Organisation API**

- **Endpoint**: `/user_organisations`
- **Description**: Manage user organisations.
- **Methods**:

  ##### `POST /user_organisations`
  - **Description**: Create a new user organisation.
  - **Request Body**:
    ```json
    {
      "user_id": "string",
      "organisation_id": "int"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "user_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "organisation_id": {
          "id": "int",
          "name": "string",
          "description": "string",
          "created_at": "datetime",
          "updated_at": "datetime"
        }
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /user_organisations`
  - **Description**: Get a list of user organisations.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "user_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "organisation_id": {
            "id": "int",
            "name": "string",
            "description": "string",
            "created_at": "datetime",
            "updated_at": "datetime"
          }
        }
      ]
      ```

  ##### `GET /user_organisations/{id}`
  - **Description**: Get a user organisation by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "user_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "organisation_id": {
          "id": "int",
          "name": "string",
          "description": "string",
          "created_at": "datetime",
          "updated_at": "datetime"
        }
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "User organisation not found"
      }
      ```

  ##### `DELETE /user_organisations/{id}`
  - **Description**: Delete a user organisation by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "User organisation not found"
      }
      ```

---

#### **Notifications API**

- **Endpoint**: `/notifications`
- **Description**: Manage notifications.
- **Methods**:

  ##### `POST /notifications`
  - **Description**: Create a new notification.
  - **Request Body**:
    ```json
    {
      "user_id": "string",
      "message_data": {
        "id": "int",
        "sender_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "receiver_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "subject": "string",
        "body": "text",
        "sent_at": "datetime"
      },
      "read_status": "string"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "user_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "message_data": {
          "id": "int",
          "sender_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "receiver_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "subject": "string",
          "body": "text",
          "sent_at": "datetime"
        },
        "read_status": "string",
        "created_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /notifications`
  - **Description**: Get a list of notifications.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "user_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "message_data": {
            "id": "int",
            "sender_id": {
              "id": "int",
              "unique_id": "string",
              "first_name": "string",
              "last_name": "string",
              "phone": "int",
              "email": "string",
              "password": "string",
              "role": "bool",
              "created_at": "datetime",
              "updated_at": "datetime"


            },
            "receiver_id": {
              "id": "int",
              "unique_id": "string",
              "first_name": "string",
              "last_name": "string",
              "phone": "int",
              "email": "string",
              "password": "string",
              "role": "bool",
              "created_at": "datetime",
              "updated_at": "datetime"
            },
            "subject": "string",
            "body": "text",
            "sent_at": "datetime"
          },
          "read_status": "string",
          "created_at": "datetime"
        }
      ]
      ```

  ##### `GET /notifications/{id}`
  - **Description**: Get a notification by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "user_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "message_data": {
          "id": "int",
          "sender_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "receiver_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "subject": "string",
          "body": "text",
          "sent_at": "datetime"
        },
        "read_status": "string",
        "created_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Notification not found"
      }
      ```

  ##### `DELETE /notifications/{id}`
  - **Description**: Delete a notification by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "Notification not found"
      }
      ```

---

#### **Posts API**

- **Endpoint**: `/posts`
- **Description**: Manage posts.
- **Methods**:

  ##### `POST /posts`
  - **Description**: Create a new post.
  - **Request Body**:
    ```json
    {
      "title": "string",
      "body": "text",
      "image": "string",
      "status": "string",
      "author_id": "string"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "title": "string",
        "body": "text",
        "image": "string",
        "status": "string",
        "author_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /posts`
  - **Description**: Get a list of posts.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "title": "string",
          "body": "text",
          "image": "string",
          "status": "string",
          "author_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "created_at": "datetime",
          "updated_at": "datetime"
        }
      ]
      ```

  ##### `GET /posts/{id}`
  - **Description**: Get a post by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "title": "string",
        "body": "text",
        "image": "string",
        "status": "string",
        "author_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Post not found"
      }
      ```

  ##### `PUT /posts/{id}`
  - **Description**: Update a post by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Request Body**:
    ```json
    {
      "title": "string",
      "body": "text",
      "image": "string",
      "status": "string",
      "author_id": "string"
    }
    ```
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "title": "string",
        "body": "text",
        "image": "string",
        "status": "string",
        "author_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "created_at": "datetime",
        "updated_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Post not found"
      }
      ```

  ##### `DELETE /posts/{id}`
  - **Description**: Delete a post by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "Post not found"
      }
      ```

---

#### **Messages API**

- **Endpoint**: `/messages`
- **Description**: Manage messages.
- **Methods**:

  ##### `POST /messages`
  - **Description**: Create a new message.
  - **Request Body**:
    ```json
    {
      "sender_id": "int",
      "receiver_id": "int",
      "subject": "string",
      "body": "text"
    }
    ```
  - **Response**:
    - **201 Created**:
      ```json
      {
        "id": "int",
        "sender_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "receiver_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "subject": "string",
        "body": "text",
        "sent_at": "datetime"
      }
      ```
    - **400 Bad Request**:
      ```json
      {
        "error": "Invalid input data"
      }
      ```

  ##### `GET /messages`
  - **Description**: Get a list of messages.
  - **Response**:
    - **200 OK**:
      ```json
      [
        {
          "id": "int",
          "sender_id": {
           

 "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "receiver_id": {
            "id": "int",
            "unique_id": "string",
            "first_name": "string",
            "last_name": "string",
            "phone": "int",
            "email": "string",
            "password": "string",
            "role": "bool",
            "created_at": "datetime",
            "updated_at": "datetime"
          },
          "subject": "string",
          "body": "text",
          "sent_at": "datetime"
        }
      ]
      ```

  ##### `GET /messages/{id}`
  - **Description**: Get a message by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **200 OK**:
      ```json
      {
        "id": "int",
        "sender_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "receiver_id": {
          "id": "int",
          "unique_id": "string",
          "first_name": "string",
          "last_name": "string",
          "phone": "int",
          "email": "string",
          "password": "string",
          "role": "bool",
          "created_at": "datetime",
          "updated_at": "datetime"
        },
        "subject": "string",
        "body": "text",
        "sent_at": "datetime"
      }
      ```
    - **404 Not Found**:
      ```json
      {
        "error": "Message not found"
      }
      ```

  ##### `DELETE /messages/{id}`
  - **Description**: Delete a message by ID.
  - **Parameters**:
    - `id`: `int` (Path Parameter)
  - **Response**:
    - **204 No Content**
    - **404 Not Found**:
      ```json
      {
        "error": "Message not found"
      }
      ```

---
