# Web Infrastructure Design

This directory contains a series of web infrastructure design tasks, each illustrated by a diagram (see images provided for each task). Each task builds on the previous one, increasing in complexity, scalability, security, and monitoring. All explanations are in English to enhance technical communication skills.

---

## 0. Simple Web Stack

**Scenario:**
A user wants to access the website at `www.foobar.com`.

**Infrastructure:**
- 1 domain name: `foobar.com` with a `www` record pointing to server IP `8.8.8.8`
- 1 server containing:
	- Web server (Nginx)
	- Application server
	- Application files (code base)
	- Database (MySQL)

**Key Concepts:**
- **Server:** A physical or virtual machine that provides services or resources to clients over a network.
- **Domain Name:** Human-readable address (e.g., foobar.com) that maps to the server's IP.
- **DNS Record:** The `www` in `www.foobar.com` is a subdomain, typically an A or CNAME record in DNS.
- **Web Server:** Handles HTTP requests from users and serves static content or forwards requests to the application server.
- **Application Server:** Runs the application code, processes business logic, and generates dynamic content.
- **Database:** Stores and manages data for the application.
- **Communication:** The server communicates with the user's computer via HTTP/HTTPS protocols.

**Issues:**
- **SPOF (Single Point of Failure):** If the server fails, the website is down.
- **Downtime for Maintenance:** Deployments or restarts cause unavailability.
- **Scalability:** Cannot handle high traffic; limited to one server.

---

## 1. Distributed Web Infrastructure

**Scenario:**
A more robust setup for `www.foobar.com` using multiple servers and a load balancer.

**Infrastructure:**
- 1 load balancer (HAProxy)
- 2 servers, each with:
	- Web server (Nginx)
	- Application server
	- Application files
	- Database (MySQL)

**Key Concepts:**
- **Load Balancer:** Distributes incoming traffic across multiple servers. Common algorithms include round-robin (each server gets requests in turn).
- **Active-Active vs. Active-Passive:**
	- *Active-Active:* All servers handle traffic simultaneously.
	- *Active-Passive:* One server is on standby, only used if the active one fails.
- **Database Primary-Replica (Master-Slave) Cluster:**
	- *Primary (Master):* Handles all writes.
	- *Replica (Slave):* Handles reads and replicates data from the primary.

**Issues:**
- **SPOF:** The load balancer or database master can be a single point of failure.
- **Security:** No firewall or HTTPS.
- **Monitoring:** Not implemented.

---

## 2. Secured and Monitored Web Infrastructure

**Scenario:**
A secure, encrypted, and monitored infrastructure for `www.foobar.com`.

**Infrastructure:**
- 3 firewalls (one per server)
- 1 SSL certificate (HTTPS)
- 3 monitoring clients (e.g., Sumologic agents)

**Key Concepts:**
- **Firewalls:** Restrict unauthorized access and protect servers.
- **HTTPS/SSL:** Encrypts traffic between users and the website, ensuring privacy and security.
- **Monitoring:** Collects metrics (e.g., QPS - queries per second) and alerts on issues. Monitoring tools gather data via agents installed on each server.
- **Monitoring QPS:** Configure the monitoring tool to track web server request rates.

**Issues:**
- **SSL Termination at Load Balancer:** If SSL ends at the load balancer, internal traffic is unencrypted.
- **Single MySQL Writer:** Only one MySQL server can accept writes, which can be a bottleneck.
- **Identical Server Components:** All servers having the same components can lead to inefficiency and security risks.

---

## 3. Scale Up

**Scenario:**
Scaling the infrastructure for higher availability and performance.

**Infrastructure:**
- 1 additional server
- 1 additional load balancer (HAProxy) in a cluster
- Split components:
	- Dedicated web server
	- Dedicated application server
	- Dedicated database server

**Key Concepts:**
- **Component Separation:** Improves scalability, performance, and security by isolating roles.
- **Load Balancer Cluster:** Increases availability by removing the load balancer as a single point of failure.

---

## Application Server vs Web Server
- **Web Server (Nginx):** Handles HTTP requests, serves static files, and forwards dynamic requests to the application server.
- **Application Server:** Runs the application code, processes business logic, and interacts with the database.

---

For each task, refer to the corresponding diagram image for a visual representation of the described infrastructure.
