# Secure Web Infrastructure for www.foobar.com

## Components Added

1. **Firewalls**
   - **Purpose**: Protect each server by filtering unauthorized access.
   - **Placement**: One firewall for each server (Web, Application, Database).

2. **SSL Certificate**
   - **Purpose**: Encrypt user-server communication for security.
   - **Implementation**: Terminate SSL at the Load Balancer to reduce overhead on servers.

3. **Monitoring Clients**
   - **Purpose**: Collect performance data and monitor server health.
   - **Implementation**: Use tools like Sumologic to track CPU, memory, and queries.
   - **Example**: Measure Web Server Queries Per Second (QPS) for traffic analysis.

## Infrastructure Design

### Traffic Flow

1. **Load Balancer**
   - Balances incoming requests using a Round Robin algorithm.
   - Terminates SSL to decrypt traffic and forward it internally.

2. **Servers**
   - **Web Servers (Nginx)**: Serve static files and forward dynamic requests.
   - **Application Servers**: Handle application logic and database requests.

3. **Database Cluster**
   - **Primary**: Handles all write operations.
   - **Replica**: Handles read operations to balance load and ensure redundancy.

## Identified Issues

1. **Unencrypted Internal Traffic**
   - SSL termination at the Load Balancer leaves backend traffic exposed.

2. **Single Point of Failure: Database Write Server**
   - If the Primary server fails, write operations halt.

3. **Non-Dedicated Server Roles**
   - Combining Web, Application, and Database functions on one server creates resource conflicts.

4. **Monitoring Coverage**
   - Without proactive monitoring, failures may go undetected.

## Recommendations

1. Extend SSL encryption to backend traffic to secure internal communication.
2. Add a failover mechanism for the Primary Database to prevent write disruptions.
3. Separate server roles to improve specialization and avoid resource contention.
4. Integrate proactive monitoring tools like Prometheus and Grafana for better visibility.

```mermaid
graph TD
    subgraph Load_Balancer_Cluster["Load Balancer Cluster"]
        LB1["Load Balancer 1 (HAProxy)"]
        LB2["Load Balancer 2 (HAProxy)"]
        LB1 -- heartbeat --> LB2
    end

    subgraph Web_Servers["Web Servers"]
        WS1["Web Server 1 (Nginx)"]
        WS2["Web Server 2 (Nginx)"]
    end

    subgraph Application_Servers["Application Servers"]
        App1["Application Server 1"]
        App2["Application Server 2"]
    end

    subgraph Database_Cluster["Database Cluster"]
        DBPrimary["Primary MySQL Server"]
        DBReplica1["Replica MySQL Server 1"]
        DBReplica2["Replica MySQL Server 2"]
    end

    %% Connections
    LB1 --> WS1
    LB1 --> WS2
    LB2 --> WS1
    LB2 --> WS2
    WS1 --> App1
    WS2 --> App2
    App1 --> DBPrimary
    App2 --> DBPrimary
    DBPrimary --> DBReplica1
    DBPrimary --> DBReplica2
```