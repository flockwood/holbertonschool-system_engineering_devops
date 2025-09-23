# One Server Web Infrastructure Design

## Infrastructure Overview

'''mermaid
graph TD
    A[User's Computer<br/>Browser] -->|1. Types www.foobar.com| B[DNS Server]
    B -->|2. Returns IP 8.8.8.8| A
    A -->|3. HTTP Request<br/>TCP/IP Protocol| C[Server IP: 8.8.8.8]
    
    subgraph "Single Server Infrastructure"
        C --> D[Nginx Web Server<br/>Port 80/443]
        D -->|Static Files| D
        D -->|Dynamic Requests| E[Application Server<br/>Business Logic]
        E --> F[Application Files<br/>Code Base]
        E -->|Query/Update| G[MySQL Database<br/>Data Storage]
        G -->|Data Response| E
        E -->|Generated Content| D
    end
    
    D -->|4. HTTP Response| A
    A -->|5. Renders Page| H[Website Display]
    
'''

## Step-by-Step Process: User Accessing www.foobar.com

### 1. User Types URL
User enters `www.foobar.com` in their browser

### 2. DNS Resolution
- Browser queries DNS servers to resolve `www.foobar.com`
- DNS returns IP address `8.8.8.8`
- Browser now knows where to send the request

### 3. HTTP Request
- Browser sends HTTP request to server at `8.8.8.8`
- Request travels through the internet using TCP/IP protocol

### 4. Web Server Processing
- **Nginx** receives the HTTP request on port 80/443
- Nginx handles static content directly (CSS, images, JS files)
- For dynamic content, Nginx forwards request to application server

### 5. Application Processing
- **Application Server** processes business logic
- Reads **Application Files** (your code base)
- May query **MySQL Database** for data

### 6. Response Generation
- Application server generates response
- Sends response back through Nginx
- Nginx returns HTTP response to user's browser

### 7. Page Rendering
- Browser receives HTML/CSS/JS
- Renders the webpage for the user

---

## Infrastructure Components Explained

### What is a Server?
A **server** is a computer system that provides services, resources, or data to other computers (clients) over a network. In our case, it's a physical or virtual machine hosting our entire web application stack.

### Role of the Domain Name
The **domain name** (`foobar.com`) serves as a human-readable address that maps to an IP address. It allows users to access websites using memorable names instead of numeric IP addresses like `8.8.8.8`.

### DNS Record Type for "www"
The **www** in `www.foobar.com` is a **CNAME record** (Canonical Name) or an **A record**:
- **A record**: Directly maps `www.foobar.com` to IP `8.8.8.8`
- **CNAME record**: Maps `www.foobar.com` to `foobar.com`, which then resolves to `8.8.8.8`

### Role of the Web Server (Nginx)
The **web server** handles HTTP requests and responses:
- Serves static files (HTML, CSS, JavaScript, images)
- Acts as reverse proxy to application server
- Handles SSL termination and security
- Manages load balancing (in multi-server setups)
- Provides caching capabilities

### Role of the Application Server
The **application server** executes the dynamic business logic:
- Processes user requests requiring computation
- Runs the application code (PHP, Python, Ruby, etc.)
- Handles user authentication and sessions
- Connects to database for data operations
- Generates dynamic HTML content

### Role of the Database (MySQL)
The **database** stores and manages data:
- Stores user information, content, configuration
- Provides data persistence and reliability
- Handles complex queries and data relationships
- Ensures data integrity and consistency

### Communication Protocol
The server communicates with users' computers using:
- **HTTP/HTTPS** protocol for web requests
- **TCP/IP** for reliable data transmission
- **DNS** protocol for domain name resolution

---

## Infrastructure Issues

### 1. Single Point of Failure (SPOF)
**Problem**: If the single server fails, the entire website becomes unavailable.
- Hardware failure = complete outage
- Software crash = service interruption
- Network issues = site unreachable

**Impact**: 100% downtime when server fails

### 2. Downtime During Maintenance
**Problem**: Maintenance activities require server restart or service interruption.
- Code deployments require application restart
- System updates need server reboot
- Database maintenance blocks access
- Security patches require downtime

**Impact**: Planned downtime affects all users during maintenance windows

### 3. Scalability Limitations
**Problem**: Single server cannot handle unlimited traffic growth.
- CPU/RAM limits restrict concurrent users
- Database becomes bottleneck under heavy load
- Network bandwidth limitations
- Storage capacity constraints

**Impact**: Performance degradation or crashes during traffic spikes

### Additional Concerns:
- **No redundancy**: No backup if components fail
- **No geographic distribution**: Users far from server experience latency
- **Security vulnerability**: Single target for attacks
- **Resource contention**: All services compete for same server resources

---

## Summary
This simple one-server infrastructure works well for small websites with moderate traffic, but has significant limitations in terms of reliability, maintenance flexibility, and scalability. As traffic and requirements grow, moving to a multi-server, distributed architecture becomes necessary.