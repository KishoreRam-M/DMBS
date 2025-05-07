## **1. Centralized Architecture**

### **Definition:**

All data is stored in a **single central database** on one server/system.

### **Architecture:**

```
Clients ---> Central Server (Single DB)
```

### **Impact:**

* **Data Storage:** Stored in one place.
* **Access:** All users connect to that one server.
* **Processing:** All queries are processed by the central server.
* **Pros:** Easy to manage, simple architecture.
* **Cons:** Single point of failure, can be slow with many users.

---

## **2. Client-Server Architecture**

### **Definition:**

The database is stored on a **server**, and **clients** request data or send queries to the server over a network.

### **Architecture:**

```
Clients <--> Database Server
```

### **Impact:**

* **Data Storage:** Still in one place (the server), but accessed remotely.
* **Access:** Clients send requests, server sends responses.
* **Processing:** Server does most of the work (query processing).
* **Pros:** Supports multiple users, better scalability than centralized.
* **Cons:** Still has a bottleneck and single point of failure.

---

## **3. Distributed Database Architecture**

### **Definition:**

The database is **spread across multiple locations** (nodes), but appears to users as a **single database**.

There are 2 major types:

* **Homogeneous:** All nodes use the same DBMS.
* **Heterogeneous:** Nodes can use different DBMSs.

---

### **Two Main Distributed Styles:**

#### A. **Client-Server Distributed**

```
Clients <--> Server A
          <--> Server B
```

* **Storage:** Data is split/shared across multiple servers.
* **Access:** Clients connect to one or more servers.
* **Processing:** Servers coordinate to process queries.
* **Example:** A bank with branches each having local DB, but all connected.

#### B. **Peer-to-Peer Distributed (P2P)**

```
Node A <--> Node B <--> Node C
```

* **All nodes are equal (peers)**: No master, every node can store and process data.
* **Storage:** Data may be fully or partially replicated across peers.
* **Access:** Any node can be contacted.
* **Processing:** Every node can handle queries or forward them.
* **Example:** Blockchain, BitTorrent.

---

### **Comparison Table:**

| Feature          | Centralized    | Client-Server     | Distributed (P2P or Multi-server) |
| ---------------- | -------------- | ----------------- | --------------------------------- |
| Data Location    | Single machine | Single server     | Multiple locations                |
| Fault Tolerance  | Low            | Moderate          | High                              |
| Scalability      | Low            | Moderate          | High                              |
| Access Speed     | Fast (locally) | Network dependent | Faster due to multiple nodes      |
| Maintenance      | Simple         | Medium            | Complex                           |
| Query Processing | Single point   | Server only       | Shared among nodes                |

---

### **Summary:**

* **Centralized** = Simple but fragile.
* **Client-Server** = Better user handling, but server is a bottleneck.
* **Distributed** = High availability, fault tolerance, and speed, but more complex to manage.
