# HTTP Proxy

A high-performance HTTP proxy written in C++ that leverages modern C++23 features. This proxy improves web performance through DNS prefetching, HTTP response caching, and HTML content filtering. This is going to be a work in progress for a while.

## Features

- **Asynchronous Networking:**  
  Utilizes Boost.Asio for non-blocking I/O and concurrent client handling.
- **HTTP Message Handling:**  
  Uses Boost.Beast for parsing and generating HTTP requests/responses.
- **DNS Prefetching & Caching:**  
  Resolves and caches domain names to reduce lookup latency.
- **HTTP Response Caching:**  
  Caches responses to serve frequent requests more quickly.
- **Content Filtering:**  
  Parses and filters HTML content with the Gumbo Parser.
- **Logging (Optional):**  
  Implements fast and thread-safe logging using spdlog.
- **Unit Testing:**  
  Uses GoogleTest to ensure that individual modules and the overall proxy function correctly.

## Directory Structure (Planned Structure)
```
http-proxy/
├── CMakeLists.txt            # Build configuration
├── README.md                 # Project documentation (this file)
├── docs/                     # Detailed design and contribution guidelines
│   └── design.md             # Detailed design document
├── include/                  # Public header files
│   └── proxy/
│       ├── ProxyServer.h         # Main proxy server class
│       ├── HTTPRequestHandler.h  # Interface for processing HTTP requests
│       ├── HTTPResponseHandler.h # Interface for processing HTTP responses
│       ├── DNSCache.h            # DNS prefetching and caching functionality
│       └── ContentFilter.h       # HTML content filtering interface
├── src/                      # Source files for implementation
│   ├── main.cpp              # Application entry point
│   ├── ProxyServer.cpp       # Implementation of the proxy server
│   ├── HTTPRequestHandler.cpp# Implementation of HTTP request processing
│   ├── HTTPResponseHandler.cpp # Implementation of HTTP response handling
│   ├── DNSCache.cpp          # Implementation of DNS prefetching and caching
│   └── ContentFilter.cpp     # Implementation of HTML content filtering
├── third_party/              # Third-party libraries (Boost, Gumbo Parser, etc.)
└── tests/                    # Unit and integration tests
├── test_dns_cache.cpp    # Tests for DNS prefetching and caching
├── test_http_requests.cpp# Tests for HTTP handling
└── test_content_filter.cpp # Tests for HTML content filtering
```

## Prerequisites

- **CMake:** Version 3.10 or higher
- **C++ Compiler:** Support for C++23 is required
- **Boost Libraries:**  
  - Boost.Asio (for asynchronous networking)  
  - Boost.Beast (for HTTP handling)  
  - Boost.System and Boost.Filesystem
- **Gumbo Parser:** For HTML parsing and filtering
- **spdlog (Optional):** For logging
- **GoogleTest:** For unit testing (integrated via CMake)

## Build Instructions (W.I.P)

1. **Clone the Repository:**

    ```bash
    git clone https://github.com/siezedeath/http-proxy.git
    cd http-proxy
    ```

2. **Building Directory:**
    ```bash
    mkdir build 
    cd build
    ```

3. **Configure Using CMake:**
    ```bash
    cmake ..
    ```

4. **Building the Project**
    ```bash
    cmake --build .
    ```

5. **Running the Proxy**
    ```bash
    ./http-proxy
    ```

6. **Run Tests**
    ```bash
    ctest
    ```