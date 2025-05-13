# Web Infrastructure Design: Single Server Hosting www.foobar.com

## 1. Introduction
This exercise explains the design of a simple one-server web infrastructure to host a website reachable via `www.foobar.com`.

---

## 2. Components of the Infrastructure

### **1. What is a Server?**
A **server** is a computer that provides information and resources to other computers (clients) in a network.  
In this design, the server hosts:
- **A web server (Nginx):** Handles incoming HTTP(S) requests.
- **An application server:** Runs the application logic.
- **The application codebase:** The source files of the website.
- **A database (MySQL):** Manages and stores persistent application data.

---

### **2. The Role of the Domain Name**
The **domain name** (e.g., `www.foobar.com`) provides a **human-readable address** that is mapped to an IP address.  
- Example: `www.foobar.com` is mapped to `8.8.8.8`.
- `www.foobar.com` uses a **Type A DNS record** (Address Record), which links the domain to the server's IP.

---

### **3. The Role of Each Component**
- **Web Server (Nginx):**
  - Acts as the **entry point** for all HTTP(S) requests.
  - Serves **static files** (images, CSS, etc.).
  - Forwards **dynamic requests** to the application server.

- **Application Server:**
  - Processes **dynamic content** like user logins or database queries.
  - Communicates with the database for retrieving or storing data.
  - Sends processed responses back to the web server.

- **Database (MySQL):**
  - Stores and manages persistent data for the application, such as user information, passwords, and content.

- **Communication Protocol:**
  - The server communicates with the user’s browser via the **HTTP or HTTPS protocol**.

---

## 3. Issues with this Infrastructure

### **1. Single Point of Failure (SPOF)**
- If the server fails, the **entire website becomes unavailable**.

### **2. Downtime During Maintenance**
- Updates or restarts (e.g., for the web server or application server) result in **temporary unavailability** of the website.

### **3. Scalability Issues**
- A single server **cannot handle high traffic volumes**.  
  Excessive requests may slow down or crash the website.

---

## 4. Design Diagram (Simplified Flow)

```plaintext
User (Browser) -> DNS resolves www.foobar.com -> Server IP (8.8.8.8)
   -> Web Server (Nginx)
      -> Application Server
         -> Application Code + Database (MySQL)
