# Multi-Threaded Proxy Server with LRU Cache

A **multi-threaded HTTP Proxy Server** implemented in **C** using socket programming, POSIX threads, semaphores, mutex locks, HTTP request parsing, and an **LRU Cache**.

## Index

* [Project Theory](#project-theory)
* [How to Run](#how-to-run)
* [Demo](#demo)
* [Contributing](#contributing)

---

# Project Theory

## Introduction

This project demonstrates how a proxy server handles client requests, forwards them to the destination server, and returns the response. It uses multithreading to handle multiple clients concurrently and an **LRU Cache** to store frequently requested responses.

### OS Concepts Used

* Threading
* Locks and Mutex
* Semaphores
* Socket Programming
* LRU Cache

### Basic Working Flow

```text
Client
   │
   ▼
Proxy Server
   │
   ▼
Parse HTTP Request
   │
   ▼
Check Cache
   │
 ┌─┴───────────┐
 ▼             ▼
Cache Hit    Cache Miss
 ▼             ▼
Response    Web Server
   │             │
   └──────┬──────┘
          ▼
        Client
```

### Project Structure

```text
MultiThreadedProxyServerClient/
├── proxy_server_with_cache.c
├── proxy_server_without_cache.c
├── proxy_parse.c
├── proxy_parse.h
├── Makefile
├── README.md
└── pics/
    └── UML.JPG
```

### Limitations

* Fixed-size cache elements.
* Large or complex websites may not be fully cached.
* Limited HTTP request support.
* Not a complete production-grade HTTPS proxy.

### Future Improvements

* POST request support
* HTTPS support
* Website filtering
* Dynamic cache management
* Multiprocessing implementation

---

# How to Run

### Clone the Repository

```bash
git clone https://github.com/70Purushottam/MultiThreaded_ProxyServerClient.git
cd MultiThreaded_ProxyServerClient
```

### Build the Project

```bash
make all
```

### Run the Proxy Server

```bash
./proxy <port-number>
```

Example:

```bash
./proxy 8080
```

Open:

```text
http://localhost:8080/https://www.cs.princeton.edu/
```

> **Note:** Run the project on Linux. Disable browser cache or use Incognito mode while testing.

---

# Demo

### First Request

```text
Cache Miss
      ↓
Fetch data from Web Server
      ↓
Store Response in Cache
      ↓
Send Response to Client
```

### Repeated Request

```text
Cache Hit
      ↓
Data Retrieved from Cache
      ↓
Send Response to Client
```

---

# Contributing

Feel free to fork this repository and contribute improvements such as:

* Better cache management
* Multiprocessing
* Additional HTTP methods
* HTTPS support
* Improved performance and error handling

---

## Technologies

`C` • `Socket Programming` • `POSIX Threads` • `Semaphore` • `Mutex` • `HTTP Parsing` • `LRU Cache`

## Author

**Purushottam Kumar**

GitHub: https://github.com/70Purushottam

### Enjoy Coding! 🚀
