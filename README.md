# Secure Multi-Client Chat Application

A C++ client-server chat application implementing TCP socket programming, OpenSSL TLS, concurrent client handling, user authentication, real-time private/broadcast messaging, file-based persistence, and graph-based friend recommendations.

## Features

* **Client-Server Architecture** — Centralized server for managing multiple connected clients and routing messages.
* **TCP Socket Communication** — Reliable, connection-oriented communication using TCP sockets.
* **TLS Security** — Secured client-server communication using OpenSSL TLS with server certificate and private key configuration.
* **Concurrent Client Handling** — Multi-threaded server with a dedicated handler thread for each connected client.
* **Thread Synchronization** — Mutex-based synchronization to protect shared client state across concurrent threads.
* **Real-Time Messaging** — Supports private messaging and broadcast messaging between connected users.
* **User Authentication** — User registration and login with hashed password storage.
* **File-Based Persistence** — Stores user and friendship data in local files.
* **Friend Management** — Add and manage friendships between users.
* **Graph-Based Friend Recommendations** — Represents friendships as an undirected graph and recommends potential connections based on mutual-friend counts.
* **Concurrent Message Reception** — Client uses a separate receiver thread to receive messages while the main thread handles user input.

## System Architecture

```text
                         ┌──────────────────────┐
                         │        SERVER        │
                         │                      │
                         │  TCP Socket : 8888   │
                         │  OpenSSL TLS         │
                         │  Client Management   │
                         │  Mutex Synchronization│
                         └──────────┬───────────┘
                                    │
                  ┌─────────────────┼─────────────────┐
                  │                 │                 │
             Client A           Client B          Client C
                  │                 │                 │
               TLS/TCP           TLS/TCP           TLS/TCP
                  │                 │                 │
             Input +          Input +          Input +
             Receiver         Receiver         Receiver
              Thread           Thread           Thread
```

### Communication Flow

1. The server initializes Winsock and OpenSSL.
2. The server creates a TCP socket, binds it to port `8888`, and starts listening.
3. A client establishes a TCP connection with the server.
4. OpenSSL performs the TLS handshake between the client and server.
5. The server receives the authenticated client's username and registers the connection.
6. A dedicated server thread handles communication for the connected client.
7. The client runs a separate receiver thread so incoming messages can be processed while the user enters messages.
8. The server routes messages as private messages or broadcasts based on the application-level command.

## Messaging

The application uses a simple command-based messaging format.

### Broadcast Message

Any normal message is broadcast to other connected clients.

```text
Hello everyone!
```

### Private Message

Private messages use:

```text
/pm <username> <message>
```

Example:

```text
/pm Bob Hello Bob!
```

The server identifies the recipient and sends the message only through that client's TLS connection.

### Exit

```text
/quit
```

closes the client's chat session and releases the associated network resources.

## Authentication

The application provides:

* User registration
* User login
* Username-based identification
* Hashed password storage
* Local persistence of user information

User authentication data is maintained using local files.

> **Security Note:** The current implementation uses C++ `std::hash` for password transformation. This is suitable for the project implementation but is not intended as production-grade password hashing. A production authentication system should use a dedicated password hashing algorithm such as Argon2 or bcrypt with per-user salts.

## Friend Recommendation System

Friend relationships are represented using an undirected graph:

* Each user is represented as a **node**.
* Each friendship is represented as an **edge**.
* For a given user, the system examines friends-of-friends.
* Existing direct friends and the user themselves are excluded from recommendations.
* Potential connections are ranked according to their number of mutual friends.

For example:

```text
        Bob
       /   \
    Alice   David
       \   /
      Charlie
```

If Alice is connected to Bob and Charlie, while both Bob and Charlie are connected to David, David can be recommended to Alice based on the two mutual connections.

## Data Structures

The project uses:

* `vector` — stores currently connected clients.
* `unordered_map` — stores user and graph-related mappings.
* `unordered_set` — stores friendship relationships and provides efficient membership checks.
* `string` — handles usernames and messages.

## Concurrency

The server creates a separate thread for each connected client.

```text
Client A → Thread A
Client B → Thread B
Client C → Thread C
```

Because these threads access shared client information, a mutex protects the shared client collection.

The implementation uses `std::lock_guard` for scoped mutex management.

On the client side, a separate receiver thread continuously waits for incoming messages while the main thread handles user input.

## Security

The application uses **OpenSSL TLS** to secure communication between each client and the server.

The server:

1. Creates an OpenSSL TLS context.
2. Loads the server certificate.
3. Loads the server private key.
4. Performs `SSL_accept()` after accepting a TCP connection.

The client:

1. Creates an OpenSSL TLS context.
2. Establishes the TCP connection.
3. Performs `SSL_connect()`.
4. Uses `SSL_read()` and `SSL_write()` for encrypted communication.

> The application provides TLS-secured client-server communication. It should not be described as end-to-end encryption because the server terminates the TLS connection and routes messages between clients.

## Technologies Used

* **Language:** C++
* **Networking:** TCP/IP, Windows Sockets (Winsock)
* **Security:** OpenSSL / TLS
* **Concurrency:** C++ Threads, Mutex, `std::lock_guard`
* **Data Structures:** Vector, Hash Map, Hash Set, Graph
* **Build System:** CMake
* **Persistence:** Local text files

## Project Structure

```text
Chat-based-app/
│
├── include/
│   └── ...
│
├── tls_client_p.cpp
├── tls_server_p.cpp
├── auth.cpp
├── auth.hpp
├── CMakeLists.txt
├── users.txt
├── friends.txt
├── hashed_map.txt
├── .gitignore
└── README.md
```

## Building and Running

### Prerequisites

* Windows
* C++ compiler with C++11 or later
* CMake
* OpenSSL

### Build

Create a build directory and configure the project using CMake:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

Make sure OpenSSL is correctly installed and available to the build configuration.

### Run the Server

Start the server executable first.

```bash
./tls_server_p
```

The server listens for incoming TCP connections on port `8888`.

### Run the Client

Start one or more client instances:

```bash
./tls_client_p
```

Register or log in, then connect to the server to begin chatting.

> The exact executable names may depend on the CMake configuration and compiler being used.

## Key Learning Outcomes

This project provided practical experience with:

* TCP socket programming
* Client-server system design
* TLS integration using OpenSSL
* Concurrent programming and thread management
* Mutex-based synchronization
* Blocking network I/O
* C++ STL data structures
* Graph traversal and recommendation logic
* Authentication and file-based persistence
* Resource management for sockets and TLS connections

## Future Improvements

Potential improvements for a production-oriented implementation include:

* Replace `std::hash` with Argon2 or bcrypt for password hashing.
* Add explicit TLS certificate verification on the client.
* Implement message framing using length-prefixed messages or delimiters.
* Replace file-based persistence with a relational database.
* Replace the thread-per-client model with an event-driven or thread-pool architecture for larger numbers of concurrent connections.
* Introduce centralized logging and improved error handling.
* Add automated unit and integration tests.
* Add a more structured group-chat model with group membership and permissions.
