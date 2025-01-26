This is the analysis of the metrics, pricing, pros. and cons of using AKS Deployment for small scale setup.

### **Key Metrics for AKS Deployment**
1. **Node Costs**:
    - In AKS, you pay for the virtual machines (nodes) in your cluster. For a small-scale deployment, we can use low-cost instances like `Standard_B2s` which is the lowest we can get and `Standard_D8s_v5` for mistral.
    - Example:
        - **1 CPU Node (Rasa)**: Azure Standard_B2s (2 vCPUs, 4 GB RAM).
        - **1 CPU Node (Mistral 7b)**: Azure Standard_D8s_v5 (8 vCPUs, 32 GB RAM).
	        or
	    - **1 GPU NODE (Mistral 7b)**: Azure Standard_NC6s_v3  (1 NVIDIA Tesla V100 GPU, 6 vCPUs, 112 GB RAM) (EXPENSIVE $1,800/month)
	    or
	    - **1 GPU Standard_NC4asT4_v3**: 4vCPUS, 28GB Ram (~$460)
1. **Cluster Management Costs**:
    - In AKS we pay for any additional resources (e.g., virtual network, storage, load balancers).
2. **Load Balancer**:
    - A Standard Load Balancer is required to distribute the load among nodes.
    - **Cost**: ~$0.025/hour + $0.01/GB of outbound data.
3. **Storage**:
    - **Azure Disk**: ~$0.04/GB/month for Standard SSD.
    - **Azure Blob Storage**: ~$0.018/GB/month (for shared storage).
4. **Networking**:
    - Outbound data transfer costs: ~$0.087/GB
5. **Autoscaling Costs (WE  DONT NEED TO USE THIS IN THE SMALL SCALE)**:
    - We ’ll always incur a baseline cost, even with minimal usage because scaling to 0 nodes in not supported by AKS.

### **Small-Scale AKS Metrics without GPU instance** 

| **Component**        | **Configuration**                    | **Cost/Month** |
| -------------------- | ------------------------------------ | -------------- |
| **Node 1 (Rasa)**    | Standard_B2s (2 vCPUs, 4 GB RAM)     | ~$19           |
| **Node 2 (Mistral)** | Standard_D8s_v5 (8 vCPUs, 32 GB RAM) | ~$340          |
| **Load Balancer**    | Standard Load Balancer               | ~$18           |
| **Storage**          | 50 GB Azure Disk                     | ~$2            |
| **Networking**       | 100 GB outbound data                 | ~$9            |

 **Total Cost**: ~$388/month
 
# **With GPU Instance**

| **Component**             | **Details**                                | **Cost/Month** |
| ------------------------- | ------------------------------------------ | -------------- |
| **Node Pool 1 (Rasa)**    | 1 x `Standard_B2s` 2cores, 8gb ram         | ~$36           |
| **Node Pool 2 (Mistral)** | 1 x `Standard_NC4asT4_v3` 4vCPUS, 28GB Ram | ~$ 460         |
| **Storage (Disks)**       | 128 GB Standard SSD                        | ~$9            |
| **Cluster**               | Single Cluster is needed.                  | ~$73           |
**Throughput: ~**: 50-150 Tokens/sec
**Total Cost: ~** $578/month

### **Comparing AKS to Standalone VMs**

|**Metric**|**Standalone VMs**|**AKS**|
|---|---|---|
|**Ease of Management**|Manual setup and scaling.|Managed scaling and orchestration.|
|**Fault Tolerance**|Limited redundancy.|Built-in high availability.|
|**Scalability**|Horizontal scaling is manual.|Autoscaling is integrated.|
|**Cost**|Slightly lower at small scale.|Slightly higher due to load balancer and network overhead.|
|**Operational Overhead**|Higher for updates and maintenance.|Lower due to Azure-managed cluster services.|

#### **Advantages of using Aks in small scale**:
1. **Future-Proofing**:
    - If we plan to grow our user base, AKS provides the foundation for easy scaling without re-architecting the deployment.
2. **Simplified Management**:
    - Azure handles updates, security patches, and node management.
3. **Multi-Service Integration**:
    - Rasa and Mistral can coexist in separate namespaces with independent scaling policies.

#### **Disadvantages**:
 **Higher Base Costs**:
 - The **load balancer** and **networking costs** make AKS slightly more expensive at small scales.

### Order of Magnitude Comparison 

- **NVIDIA T4**:
    - Estimated throughput: 50 to 150 tokens per second.
    - Highly efficient for matrix-heavy computations, great for running large NLP models..

- **Standard_D8s_V5 (8 vCPUs, 32 GB RAM)**:
    - Estimated throughput: 10 to 40 tokens per second.
    - Significantly slower for AI model inference.

### Order of Magnitude in Performance
**GPU (NVIDIA T4)** is approximately **3 to 5 times faster** than **CPU (Standard_D8s_V5)** for inference with large models like Mistral in our case.
