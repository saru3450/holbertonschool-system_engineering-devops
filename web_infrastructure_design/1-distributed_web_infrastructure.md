# Distributed Web Infrastructure for www.foobar.com

## Infrastructure Diagram  
```mermaid
graph TD
    %% Load Balancer
    LB["Load Balancer (HAProxy)"]

    %% Server 1
    subgraph Server1["Server 1"]
        WS1["Web Server 1 (Nginx)"]
        App1["Application Server 1"]
        DB1["Database 1 (MySQL - Primary)"]
        CodeBase1["Application Files"]
    end

    %% Server 2
    subgraph Server2["Server 2"]
        WS2["Web Server 2 (Nginx)"]
        App2["Application Server 2"]
        DB2["Database 2 (MySQL - Replica)"]
        CodeBase2["Application Files"]
    end

    %% Connections
    LB --> WS1
    LB --> WS2

    WS1 --> App1
    WS2 --> App2

    App1 --> DB1
    App2 --> DB1
    DB1 --> DB2
```

## Components

1. **Load Balancer (HAProxy)**
   - Distributes traffic across two servers using **Round Robin**:
     ```
     ┌──── request1: --> Server 1
     ├──── request2: --> Server 2
     ├──── request3: --> Server 1
     ├──── request4: --> Server 2
     ```
   - Configured in **Active-Active** mode: both servers handle requests simultaneously.

2. **Two Servers**
   - **Web Servers (Nginx)**: Serve static files and proxy requests to the application.
   - **Application Servers**: Process logic and serve dynamic content.

3. **Database Cluster**
   - **Primary Database**: Handles write operations (Insert, Update, Delete).
   - **Replica Database**: Handles read operations (Select), reducing the Primary's load.

## Issues

1. **Single Points of Failure (SPOF)**
   - Load Balancer failure stops traffic distribution.
   - Primary Database crash limits functionality to read-only.

2. **Security**
   - No firewall: Vulnerable to attacks.
   - No HTTPS: Exposes unencrypted traffic.

3. **No Monitoring**
   - Lack of tools for performance tracking and failure detection.
