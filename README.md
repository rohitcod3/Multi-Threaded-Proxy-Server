# 🧩 Multi-threaded Proxy Server in C

This project is a complete implementation of a **multi-threaded HTTP proxy server** written in C. It listens for client HTTP GET requests, forwards them to the appropriate remote web servers, caches the responses locally using an **LRU (Least Recently Used)** caching mechanism, and then returns the results to the clients. The server supports **concurrent connections** through POSIX threads and demonstrates concepts of low-level network communication, concurrency control, and memory management.

---

## ⚙️ Features

- ✅ Handles multiple clients concurrently using `pthread`
- ✅ Parses and forwards valid HTTP GET requests to target servers
- ✅ Local caching of responses to minimize redundant network calls
- ✅ LRU (Least Recently Used) caching strategy with memory size limits
- ✅ Thread synchronization with mutex locks and semaphores
- ✅ Graceful error handling for bad requests, timeouts, and invalid hosts

---

## 🗂 Project Structure

```
proxy-server/
├── proxy_server_with_cache.c      # Main proxy server with threading and caching logic
├── proxy_parse.c                  # Parser for incoming HTTP GET requests
├── proxy_parse.h                  # Header declarations for the parser module
├── Makefile                       # Compilation instructions
└── README.md                      # This documentation file
```

---

## 🛠 How to Build and Run

### 1. Clone the repository:
```bash
git clone https://github.com/yourusername/proxy-server.git
cd proxy-server
```

### 2. Compile using Makefile:
```bash
make
```

### 3. Run the proxy server:
```bash
./proxy_server_with_cache
```

> By default, the proxy listens on port 8080. You can change the port by modifying the `port_number` variable in `proxy_server_with_cache.c`.

### 4. Test the proxy server:
You can use a browser (set proxy settings to `localhost:8080`) or tools like `curl`:
```bash
curl -x localhost:8080 http://example.com
```

---

## 🧪 How It Works (Flow)

1. Client sends an HTTP GET request to the proxy.
2. The proxy parses the request using a custom parser (`proxy_parse.c`).
3. The server checks if the requested URL is already in the cache.
   - ✅ **Cache Hit:** Return cached response
   - ❌ **Cache Miss:** Connect to the target server
4. Fetch data from the remote server via sockets
5. Save the response in cache if size constraints allow
6. Send the response back to the client

---

## 🧵 Thread Management
- Each new client connection spawns a **dedicated thread** using `pthread_create()`.
- Shared resources (like the cache) are protected using **`pthread_mutex_t`** for mutual exclusion.
- A **semaphore (`sem_t`)** limits the number of active threads to avoid server overload.

---

## 🧠 Concepts Demonstrated

- 📡 **Socket Programming:** Creating TCP connections, reading/writing to sockets
- 🧵 **Multi-threading:** Creating, managing, and synchronizing multiple threads
- 📦 **Caching:** Efficient in-memory data caching using a linked list
- 🔐 **Concurrency Control:** Using mutexes and semaphores to avoid race conditions
- 📑 **HTTP Protocol:** Parsing and understanding basic HTTP/1.0 and 1.1 requests

---

## 🧩 Problems Faced

- 🔄 **Race Conditions:** Early versions of the server had cache corruption due to lack of proper locking.
- 🔍 **Request Parsing:** Building a robust HTTP parser to handle edge cases and malformed requests was tricky.
- 🧵 **Thread Cleanup:** Ensuring that threads terminated cleanly and did not leak memory or file descriptors.
- ⚠️ **Handling Broken Connections:** Managing socket timeouts and failed server responses without hanging threads.

---

## 📚 What I Learned

- How to manage real-world concurrency with mutexes and semaphores
- The flow of HTTP traffic at the socket level
- Efficient use of memory and buffers for network I/O
- How LRU caching works and how to implement it from scratch
- Structuring and debugging multi-threaded C programs with clarity and discipline

---


## 📜 License

This project is licensed under the MIT License. You are free to use, modify, and distribute it for personal and academic purposes.

---

## 🙋‍♂️ Author

Built by Rohit — a hands-on project that deepened my understanding of how proxy servers work under the hood. It allowed me to explore low-level networking, memory management in C, and the complexities of writing safe and efficient multi-threaded applications.
> For questions or suggestions, feel free to reach out or open an issue in the repository.

