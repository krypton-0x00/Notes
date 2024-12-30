
### **Reliability**

- **Definition**: The ability of a system to perform its intended function consistently and accurately over time without failures.
- **Key Characteristics**:
    - **Availability**: The proportion of time a system is operational and accessible.

				a = up time / total time

		
    - **Durability**: Data integrity over time, ensuring no loss or corruption.
    - **Consistency**: Ensuring outputs meet expected behaviors under defined inputs.
- **Techniques to Improve Reliability**:
    1. **Redundancy**: Duplicating critical components to prevent single points of failure.
    2. **Monitoring and Alerts**: Implementing observability tools to detect anomalies.
    3. **Load Balancing**: Distributing workloads to prevent overload and ensure uptime.
    4. **Error Handling**: Implementing retry mechanisms and fallback strategies.
    5. **Testing**: Performing unit tests, integration tests, and chaos engineering.

---

### **Fault Tolerance**

- **Definition**: The ability of a system to continue operating correctly in the presence of faults or failures.
- **Fault Types**:
    - **Transient Faults**: Temporary errors that resolve on their own.
    - **Intermittent Faults**: Errors that occur sporadically.
    - **Permanent Faults**: Persistent failures requiring intervention.
- **Key Principles**:
    - **Failover**: Switching to a backup system or resource upon detecting a failure.
    - **Graceful Degradation**: Reducing functionality while still providing core services.
    - **Self-Healing**: Automatically recovering from faults without manual intervention.
- **Implementation Strategies**:
    1. **Replication**: Maintaining copies of data or services to replace failed components.
    2. **State Management**: Using quorum-based systems (e.g., Raft, Paxos) for consensus.
    3. **Circuit Breakers**: Preventing cascading failures by isolating faults.
    4. **Idempotency**: Ensuring operations can be safely retried.

---

### **Redundancy**

- **Definition**: The duplication of critical components or functions to increase system reliability and fault tolerance.
- **Types of Redundancy**:
    1. **Hardware Redundancy**:
        - Duplicate servers, network links, or storage devices.
        - Examples: RAID (Redundant Array of Independent Disks) for storage.
    2. **Software Redundancy**:
        - Multiple instances of applications or services (e.g., active-passive setups).
    3. **Data Redundancy**:
        - Replicating data across locations or systems.
        - Examples: Master-slave databases, leader-follower architectures.
    4. **Geographical Redundancy**:
        - Deploying resources in multiple geographic locations to handle regional failures.
- **Drawbacks**:
    - Increased cost: Maintaining extra resources.
    - Complexity: More components to manage and synchronize.

---

### **How They Work Together**

1. **Reliability uses redundancy** to ensure that the failure of one component doesn’t bring the entire system down.
2. **Fault tolerance ensures the system can operate despite faults**, using techniques like redundancy and self-healing mechanisms.
3. **Redundancy acts as a foundation** for building reliable and fault-tolerant systems by eliminating single points of failure.

---

### **Examples in Practice**

- **Cloud Services**:
    - AWS, GCP, and Azure offer auto-scaling, load balancing, and multi-zone deployments.
- **Distributed Databases**:
    - Systems like Cassandra and MongoDB replicate data across nodes for fault tolerance.
- **Content Delivery Networks (CDNs)**:
    - Use geographically redundant edge servers to ensure high availability and fault tolerance.

By understanding and implementing these principles effectively, you can design systems that are resilient, highly available, and robust even under adverse conditions.


# Through Put
t = amount of data OR amount of request / some period of time
# Latency 
Some period of time