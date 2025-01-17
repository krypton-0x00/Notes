## Resource Cost and Arch Document  for Deploying Mistral 7B alongside Rasa

### **Intention**
Deploying the Mistral 7B model for fast responses alongside rasa involves significant hardware and infrastructure considerations. This documnet outlines resource requirements, associated costs, and recommendations to achieve optimal performance.

#### **Infrastructure Options**
1. **Azure Kubernetes Service with GPU Nodes**
    - Rasa And Mistral will be deploy as a microservice.
    - Scales dynamically based on traffic.
    - **Setup Complexity**: ==High.==
2. **Azure Machine Learning**
    - Managed endpoints for model deployment.
    - Simplified scaling and monitoring.
    - **Setup Complexity**: ==Medium==.
3. **Azure Virtual Machines with GPUs**
    - Direct deployment on GPU-backed VMs.
    - Suitable for low-traffic applications.
    - **Setup Complexity**: ==Low.==

---

### **Cost Breakdown**
#### **Estimated Costs by Instance Type**

| **Instance Type**         | **Purpose**       | **Cost**    | **Throughput**     | **Latency**               |
| ------------------------- | ----------------- | ----------- | ------------------ | ------------------------- |
| Standard_B2s (CPU)        | Rasa Deployment   | $19/month   |                    |                           |
| Standard_D32s_v5 (CPU)    | Rasa Deployment   | $1.87/hour  | ~10 tokens/second  | ~1-2 seconds per response |
| Standard_ND96asr_v4 (GPU) | Mistral Inference | $15.85/hour | ~400 tokens/second | ~50-200ms per response    |
| Standard_NC24rs_v3 (GPU)  | Mistral Inference | $8.10/hour  | ~200 tokens/second | ~200-400ms per response   |
|                           |                   |             |                    |                           |
**Pricing**
-> Standard_D32s_v5 https://instances.vantage.sh/azure/vm/d32s-v5
-> Standard_ND96asr_v4 https://instances.vantage.sh/azure/vm/nd96asr
-> Standard_NC24rs_v3 https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/
#### **Deployment Scenarios**

|             |                   |                             |                           |                    |                             |
| ----------- | ----------------- | --------------------------- | ------------------------- | ------------------ | --------------------------- |
| Terms       | **Traffic Level** | **Recommended Deployment**  | **Estimated Hourly Cost** | **Benchmarks**     | **Minimum Time to Respond** |
| Short Term  | Low Traffic       | Azure VM with GPU           | ~$19/month + GPU          | ~10 tokens/second  | ~1-2 seconds                |
| Medium Term | Medium Traffic    | AKS with GPU nodes          | ~$5-10/hour               | ~200 tokens/second | ~200-400ms                  |
| Longer Run  | High Traffic      | Azure ML with GPU endpoints | ~$15/hour                 | ~400 tokens/second | ~50-200ms                   |

# Recommended Hardware:

## 1. Short Term (0-50 Users)
#### a. Rasa
	Instance: Azure Standard_B2s  (CPU)
	CPU:  2 vCPUS
	RAM: 4GB RAM.
	cost: ~ $19/month
#### b. Mistral 7b
	Instance: Azure Standard_D8s_v5 (CPU)
	CPU: 8 vCPUs
	RAM: 32 GB RAM.
	cost: ~ $340 / month
#### c. Storage (optional)
	Type: Azure Blob storage
	cost: ~ $0.018 / GB

#Note: we can make this much more better if you go with the providers which provide Mistral 7b as a service to reduce the costs and at the same time get faster responses, Once we have some users we can add a gpu instance to the infra.   
![[Pasted image 20250111175042.png]]

## 2. Mid Term (50-1000 Users)
### Vertical Scaling
#### a. Rasa
	Instance: Azure Standard_D4s_v5 (CPU)
	 CPU: 4 vCPUs
	 RAM: 16 GB RAM
	 Cost: ~ **$140/month**
#### b. Mistral 7B
	Instance: Azure Standard_NC6s_v3 (GPU)
	GPU: NVIDIA Tesla V100
	CPU: 6 vCPUs
	RAM: 112 GB RAM
	Cost: ~ **$1,800/month
#### c. Storage (Optional)
	Type: Azure Blob storage
	Cost: ~$0.018/GB
### Horizontal Scaling
#### a. Rasa
	Instance: Azure Standard_B2s (CPU)
	Scaling: 3 instances (2 vCPUs, 4 GB RAM each)
	Cost: ~$57/month (3 instances × $19 each)
#### b. Mistral 7B
	Instance: Azure Standard_NC6s_v3 (GPU)
	Scaling: 2 instances (6 vCPUs, 112 GB RAM, 1 NVIDIA Tesla V100 each)
	Cost: ~ **$3,600/month (2 instances × $1,800 each)
#### c. Storage (Optional)
	Type: Azure Blob storage
	Cost: ~$0.018/GB

![[Pasted image 20250111175918.png]]

# 3. Large Scale (1000+ Users)
#Note: we cannot go with vertical scaling due to hardware limitations.
### Horizontal Scaling
#### a. Rasa
	Instance: Azure Standard_D4s_v5 (CPU)
	Scaling: 4 instances (4 vCPUs, 16 GB RAM each)
	Cost: ~ $560/month (4 instances × $140 each)
#### b. Mistral 7B
	Instance: Azure Standard_NC12s_v3 (GPU)
	Scaling: 4 instances (12 vCPUs, 224 GB RAM, 2 NVIDIA Tesla V100s each)
	Cost: ~ **$7,200/month** (4 instances × $1,800 each)
#### c. Storage (Optional)
	Type: Azure Blob storage
	Cost: ~$0.018/GB

![[Pasted image 20250111175815.png]]


ORDER OF MAG 
