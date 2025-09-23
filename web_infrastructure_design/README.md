# Web Infrastructure Designs

This repository contains four progressive web infrastructure designs for hosting `www.foobar.com`, from basic single-server setups to advanced distributed architectures.

## Infrastructure Evolution

### 1. [Single Server Infrastructure](./single-server-infrastructure.md)
Basic LAMP stack on one server.

**Components:**
- 1 Server with Nginx, Application Server, Application Files, MySQL
- DNS resolution to single IP address

**Use Case:** Small websites, development environments, proof of concepts

**Key Issues:** Single point of failure, no scalability, maintenance downtime

### 2. [Three Server Infrastructure](./three-server-infrastructure.md) 
Load-balanced setup with database replication.

**Components:**
- HAProxy Load Balancer
- 2 Servers (each with full stack)
- Primary-Replica database setup

**Improvements:** Load distribution, basic redundancy, improved performance

**Remaining Issues:** Load balancer SPOF, security gaps, no monitoring

### 3. [Secured Infrastructure](./secured-infrastructure.md)
Enhanced security and monitoring capabilities.

**Components:**
- 3 Firewalls for layered protection
- SSL certificate for HTTPS encryption
- 3 Monitoring clients (Sumologic agents)
- Same server architecture as design #2

**Improvements:** Data encryption, network security, operational visibility

**Remaining Issues:** SSL termination gaps, single write database, monolithic servers

### 4. [Separated Components](./separated-components-infrastructure.md)
Specialized server roles with clustered load balancers.

**Components:**
- 2 Clustered HAProxy Load Balancers
- Dedicated Web Server (Nginx only)
- Dedicated Application Server (business logic only)  
- Dedicated Database Server (MySQL only)

**Improvements:** Component specialization, better resource utilization, independent scaling

## Key Concepts Explained

### Web Server vs Application Server
- **Web Server:** Serves static files (HTML, CSS, images)
- **Application Server:** Runs dynamic business logic and code

### Load Balancing
- **Round Robin:** Distributes requests sequentially across servers
- **Active-Active:** All servers handle traffic simultaneously
- **Active-Passive:** One server active, others on standby

### Database Replication
- **Primary:** Handles all writes, source of truth
- **Replica:** Handles reads, synchronized copy of primary

### Security Layers
- **Firewalls:** Control network traffic, block attacks
- **HTTPS/SSL:** Encrypt data in transit
- **Monitoring:** Track performance and detect issues

## Infrastructure Progression

```
Single Server → Load Balanced → Secured → Separated Components
     ↓              ↓             ↓              ↓
   Simple        Redundancy    Security      Specialization
```

Each design builds upon the previous one, addressing limitations while introducing new complexities and capabilities.

## Common Issues Across Designs

### Single Points of Failure (SPOF)
- Load balancers without clustering
- Single primary database for writes
- Network connections and DNS

### Security Considerations
- Unencrypted internal traffic
- Missing firewalls and access controls
- SSL termination points

### Operational Challenges
- Monitoring and alerting gaps
- Manual failover procedures
- Configuration management complexity

## Getting Started

1. Start with the single server design to understand basic concepts
2. Progress through each design to see how complexity and capabilities evolve
3. Each document includes implementation details and issue analysis
4. Use the diagrams to visualize architecture and traffic flow

## File Structure

```
├── README.md
├── single-server-infrastructure.md
├── three-server-infrastructure.md  
├── secured-infrastructure.md
└── separated-components-infrastructure.md
```

Each document contains Mermaid diagrams that render automatically on GitHub, showing the infrastructure layout and request flow for that specific design.