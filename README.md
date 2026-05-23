# tuyul-engine

`tuyul-engine` is a lightweight, low-latency, and high-performance asynchronous networking engine written from scratch in **Modern C++ (C++17)**. 

The main goal here is to build a rock-solid networking foundation that compiles with absolutely *zero external dependencies*. Inspired from `boost` but stripped down to be ultra-modular and easy to grasp.

---

## 🗺️ System Design

The infrastructure relies on **Event-Driven Reactor Pattern**. To get maximum performance on Linux, directly hook into the kernel's native **`epoll`** mechanism, combined with a strict zero-copy buffer strategy prevent hog RAM or CPU cycles.

```mermaid
graph TD
    %% Styling
    classDef appClass fill:#f9f,stroke:#333,stroke-width:2px;
    classDef coreClass fill:#bbf,stroke:#333,stroke-width:2px;
    classDef roadmapClass fill:#eee,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5;
    classDef osClass fill:#ddd,stroke:#333,stroke-width:2px;

    subgraph tuyul_example [tuyul-example: Living Examples Catalog]
        app[Your Application / Examples]
    end

    subgraph tuyul_engine [tuyul-engine: Core Framework]
        subgraph roadmap [Application Layer: Future Roadmap]
            http[http_server]
            ws[websocket]
            mqtt[mqtt]
            sftp[sftp]
            grpc[gRPC]
        end
        
        subgraph transport [Transport Layer: Current Focus]
            tcp[tuyul-tcp]
            udp[tuyul-udp]
        end
    end

    subgraph os_layer [Operating System Layer]
        kernel[Linux Kernel: Epoll Engine & Sockets]
    end

    %% Apply Styles
    class app appClass;
    class tcp,udp coreClass;
    class http,ws,mqtt,sftp,grpc roadmapClass;
    class kernel osClass;

    %% Data Flow
    app -->|Uses High-Level API| http
    app -->|Uses Bare TCP Callbacks| tcp

    http -->|Processes Raw Buffers| tcp
    ws -->|Upgrades Connection| http
    mqtt -->|Decodes Binary PDUs| tcp
    sftp -->|Handles Secure Streams| tcp
    grpc -->|Serializes Protobuf| http

    tcp -->|I/O Multiplexing epoll_wait| kernel
    udp -->|Connectionless Datagrams| kernel
```
---