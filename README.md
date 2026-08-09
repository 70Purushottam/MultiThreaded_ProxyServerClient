# Multi-Threaded Proxy Server with Cache

This project is implemented using `C` and HTTP request parsing is referred from the Proxy Server implementation.

## Index

* [Project Theory](https://github.com/70Purushottam/MultiThreadedProxyServerClient#project-theory)
* [How to Run](https://github.com/70Purushottam/MultiThreadedProxyServerClient#how-to-run)
* [Demo](https://github.com/70Purushottam/MultiThreadedProxyServerClient#demo)
* [Contributing](https://github.com/70Purushottam/MultiThreadedProxyServerClient#contributing)

---

# Project Theory

[Back to top](https://github.com/70Purushottam/MultiThreadedProxyServerClient#index)

## Introduction

A proxy server acts as an intermediary between a client and a web server. Instead of the client communicating directly with the destination server, the request is first sent to the proxy server. The proxy processes the request, checks its cache, and either returns cached data or forwards the request to the destination server.

This project implements a **Multi-Threaded Proxy Server in C** using:

* Socket Programming
* POSIX Threads
* Semaphores
* Mutex Locks
* HTTP Request Parsing
* LRU Cache

The main objective of this project is to demonstrate important **Operating System concepts**, including concurrency, synchronization, thread management, and shared resource handling.

---

## Basic Working Flow of the Proxy Server

```text
Client
   |
   | HTTP Request
   v
+-------------------+
|   Proxy Server    |
+-------------------+
          |
          v
    Parse Request
          |
          v
    Check LRU Cache
       /       \
      /         \
Cache Hit      Cache Miss
    |              |
    v              v
Return Cached    Connect to
Response         Web Server
    |              |
    |              v
    |         Get Response
    |              |
    |              v
    |         Store in Cache
    |              |
    +--------------+
           |
           v
      Send Response
       to Client
```

### Detailed Flow

1. The proxy server starts and listens on a specified port.
2. A client sends an HTTP request to the proxy server.
3. The server accepts the client connection.
4. A new thread is created to handle the client request.
5. The HTTP request is parsed to extract the required information.
6. The proxy checks whether the requested response is available in the cache.
7. If the data is found, it is returned directly to the client.
8. If the data is not found, the proxy connects to the original web server.
9. The proxy receives the response from the web server.
10. The response is stored in the LRU cache.
11. The response is sent back to the client.
12. The client connection is closed after processing.

---

## How Did We Implement Multi-threading?

The project uses **POSIX Threads (`pthread`)** to handle multiple client requests concurrently.

When a new client connects:

```text
Client Connection
        |
        v
     accept()
        |
        v
Create New Thread
        |
        v
Thread Handles Client Request
```

Multiple clients can therefore be handled simultaneously:

```text
                 Proxy Server
                      |
         +------------+------------+
         |            |            |
         v            v            v
      Thread 1      Thread 2      Thread 3
         |            |            |
      Client 1      Client 2      Client 3
```

The project uses:

* `pthread_create()` for creating threads.
* Semaphores for controlling concurrent operations.
* Locks for protecting shared resources such as the cache.
* `sem_wait()` to acquire a semaphore.
* `sem_post()` to release a semaphore.

A semaphore is used instead of relying on `pthread_join()` because the proxy server should continuously accept and process new client connections without waiting for each individual client thread to finish.

---

## Motivation / Need of the Project

The main purpose of this project is to understand:

* How requests travel from a local computer to a web server.
* How a proxy server works as an intermediary.
* How multiple client requests can be handled concurrently.
* The use of threads in a server application.
* Synchronization and locking procedures for concurrent access.
* The concept of caching.
* The implementation of the LRU cache replacement algorithm.
* How caching can reduce repeated communication with a web server.

### Proxy Server Benefits

A proxy server can:

* Speed up repeated requests using cached responses.
* Reduce traffic and load on the destination server.
* Control or restrict access to specific websites.
* Hide the client's IP address from the destination server in certain proxy configurations.
* Be extended to support encryption and additional security mechanisms.

---

## Operating System Components Used

This project demonstrates the following Operating System concepts:

### 1. Threading

Multiple client requests are handled concurrently using POSIX threads.

### 2. Locks

Locks are used to protect shared resources and prevent race conditions during concurrent cache access.

### 3. Semaphores

Semaphores are used for synchronization and to control concurrent operations.

### 4. Cache

The project stores previously requested responses to reduce repeated communication with the original web server.

### 5. LRU Cache Algorithm

LRU stands for **Least Recently Used**.

When the cache becomes full, the response that has not been used for the longest time is removed.

Example:

```text
Cache Capacity = 3

Request A
[A]

Request B
[B, A]

Request C
[C, B, A]

Access A
[A, C, B]

Request D
[D, A, C]
```

In the final step, `B` is removed because it is the least recently used item.

---

## Project Structure

```text
MultiThreadedProxyServerClient/
│
├── proxy_server_with_cache.c
├── proxy_server_without_cache.c
├── proxy_parse.c
├── proxy_parse.h
├── Makefile
├── README.md
└── pics/
    └── UML.JPG
```

### Important Files

#### `proxy_server_with_cache.c`

Contains the main implementation of the multi-threaded proxy server with LRU caching.

#### `proxy_server_without_cache.c`

Contains the proxy server implementation without caching.

#### `proxy_parse.c`

Contains the HTTP request parsing implementation.

#### `proxy_parse.h`

Contains declarations and structures required by the HTTP parser.

#### `Makefile`

Automates the compilation and building process.

---

# Limitations

* If a URL opens multiple resources, the cache may store individual responses separately.
* During retrieval, only a chunk of a large response may be sent in some cases.
* Large websites may not be completely stored because the cache element size is fixed.
* The project has limited HTTP request support.
* It is not a complete production-grade HTTPS proxy implementation.
* Modern websites may behave differently because they use complex HTTPS, JavaScript, and multiple resource requests.

---

# How This Project Can Be Extended

This project can be improved in several ways:

### 1. Multiprocessing

The server can be implemented using multiple processes to achieve parallelism.

### 2. Website Filtering

A blacklist or whitelist can be implemented to control which websites are allowed or blocked.

### 3. POST Request Support

The proxy can be extended to support HTTP methods such as:

```text
GET
POST
PUT
DELETE
```

### 4. Dynamic Cache Size

The fixed-size cache can be replaced with a dynamically managed cache.

### 5. HTTPS Support

Support for the `CONNECT` method and TLS tunneling can be added.

### 6. Better Cache Management

Cache expiration, cache validation, and more efficient data structures can be implemented.

---

# How to Run

## Prerequisites

This project is designed to run on a Linux environment.

Install the required tools:

```bash
sudo apt update
sudo apt install build-essential git curl
```

For Windows users, it is recommended to use **WSL with Ubuntu**.

---

## Clone the Repository

```bash
git clone https://github.com/70Purushottam/MultiThreadedProxyServerClient.git
```

Move into the project directory:

```bash
cd MultiThreadedProxyServerClient
```

---

## Build the Project

Compile the project:

```bash
make all
```

If required, clean the previous build first:

```bash
make clean
make all
```

---

## Run the Proxy Server

Run the executable by providing a port number:

```bash
./proxy <port-number>
```

For example:

```bash
./proxy 8080
```

The proxy server will now listen on port `8080`.

---

## Open a Website Through the Proxy

According to the project implementation, open the following URL format:

```text
http://localhost:<port-number>/<website-url>
```

Example:

```text
http://localhost:8080/https://www.cs.princeton.edu/
```

> **Note:** Disable your browser cache or use a private/incognito window while testing the cache functionality.

---

# Testing the Cache

## First Request — Cache Miss

When a website is opened for the first time:

```text
Client
   |
   v
Proxy Server
   |
   v
URL Not Found in Cache
   |
   v
Cache Miss
   |
   v
Fetch Data from Web Server
   |
   v
Store Response in Cache
   |
   v
Send Response to Client
```

The proxy fetches the response from the original server and stores it in the cache.

---

## Second Request — Cache Hit

When the same website is requested again:

```text
Client
   |
   v
Proxy Server
   |
   v
URL Found in Cache
   |
   v
Cache Hit
   |
   v
Data Retrieved from Cache
   |
   v
Send Response to Client
```

The proxy terminal should print a message indicating that:

```text
Data is retrieved from the cache
```

This demonstrates the working of the LRU cache.

---

# Running Without Cache

To run the proxy server without caching, modify the `Makefile` to use:

```text
proxy_server_without_cache.c
```

instead of:

```text
proxy_server_with_cache.c
```

Then rebuild the project:

```bash
make clean
make all
```

Run the server:

```bash
./proxy 8080
```

In this version, every client request is forwarded to the original server.

---

# Demo

The expected behavior of the project is:

### First Request

```text
URL not found
Cache Miss
Fetching data from the server
Response stored in cache
```

### Repeated Request

```text
Data is retrieved from the cache
```

The second request can be faster because the proxy does not need to contact the original web server again if a valid cached response is available.

---

# Contributing

Contributions and improvements are welcome.

You can improve the project by implementing:

* Multiprocessing.
* Website filtering.
* Support for POST requests.
* Better cache management.
* Dynamic cache allocation.
* HTTPS support.
* Improved error handling.
* Performance optimizations.

Feel free to fork the repository, make changes, and submit improvements.

---

## GitHub Repository

[MultiThreadedProxyServerClient](https://github.com/70Purushottam/MultiThreadedProxyServerClient)

## GitHub Profile

[70Purushottam](https://github.com/70Purushottam)

---

#### Enjoy Coding! Contributions and pull requests are highly appreciated.
