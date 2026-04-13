# Chat-based-app
End-to-end encrypted C++ chat application with SSL/TLS, real-time messaging, and graph-based friend recommendations.
# Secure Chat Application with TLS & Social Graph

## Overview

This project is a **secure, real-time chat application** built in C++ that enables private and group communication between multiple users. It incorporates **SSL/TLS encryption** to ensure secure data transmission and implements a **graph-based friend recommendation system** for enhanced social connectivity.

The project demonstrates concepts from **computer networks, concurrent programming, and graph algorithms**.

---

## Key Features

* End-to-end encrypted communication using SSL/TLS
* Real-time private and group messaging
* Multi-client handling using concurrent socket programming
* Thread-safe architecture for reliable communication
* Graph-based friend recommendation system
* Efficient handling of multiple connections

---

## System Architecture

* **Client-Server Model**

  * Server manages multiple client connections
  * Clients communicate securely via encrypted channels

* **Security Layer**

  * Encryption using SSL/TLS protocols
  * Secure key exchange and data transmission

* **Concurrency**

  * Multi-threaded server to handle multiple users simultaneously

* **Recommendation Engine**

  * Graph-based approach to suggest new connections

---

## Security Implementation

* Integrated OpenSSL for SSL/TLS encryption
* Ensures:

  * Data confidentiality
  * Integrity of messages
  * Protection against eavesdropping

---

## Project Structure

```
secure-chat-app/
│── client/              # Client-side implementation
│── server/              # Server-side implementation
│── include/             # Header files
│── src/                 # Core logic (socket handling, encryption)
│── certs/               # SSL certificates (self-signed)
│── build/               # Compiled files (ignored)
│── README.md
```

---

## Installation & Setup

### 1. Clone the repository

```
git clone https://github.com/your-username/secure-chat-app.git
cd secure-chat-app
```

### 2. Install dependencies

Make sure you have:

* C++ compiler (g++)
* OpenSSL installed

### 3. Compile the project

```
g++ server.cpp -o server -lssl -lcrypto -lpthread
g++ client.cpp -o client -lssl -lcrypto -lpthread
```

### 4. Run the server

```
./server
```

### 5. Run clients (in different terminals)

```
./client
```

---

## How It Works

1. Server starts and listens for incoming connections
2. Clients connect securely using SSL/TLS
3. Messages are encrypted before transmission
4. Server handles multiple clients concurrently
5. Friend recommendation system suggests connections using graph logic

---

## Key Concepts Used

* Socket Programming
* SSL/TLS Encryption
* Multi-threading
* Graph Algorithms
* Client-Server Architecture

---

## Future Improvements

* Add GUI for better user experience
* Implement authentication (login/signup system)
* Use database for persistent chat storage
* Improve recommendation system using advanced algorithms
* Deploy on cloud for global accessibility

---

## Contributing

Contributions are welcome! Feel free to fork and improve the project.

---

## Disclaimer

This project is for educational purposes and demonstrates secure communication concepts.

---

## Contact

For queries or collaboration, feel free to connect via GitHub or LinkedIn.

---
