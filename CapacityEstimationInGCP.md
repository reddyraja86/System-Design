# **API Capacity Estimation Template**

## **📌 Overview**

Use this template to calculate resource usage for each API in your Java web application. Fill in the details for each API to estimate **CPU, RAM, storage, and network bandwidth** requirements.

---

## **1️⃣ API Details**

| Field                               | Value                 |
| ----------------------------------- | --------------------- |
| **API Name**                        | `<API Name>`          |
| **Endpoint**                        | `<Endpoint>`          |
| **Request Type**                    | `GET/POST/PUT/DELETE` |
| **Expected RPS**                    | `<Number>`            |
| **Concurrent Users**                | `<Number>`            |
| **Average Response Time (ms)**      | `<Number>`            |
| **Data Sent (KB per request)**      | `<Number>`            |
| **Data Received (KB per response)** | `<Number>`            |

---

## **2️⃣ CPU Estimation**

### **Step 1: Measure CPU Time Per Request**

- **Profiling Tool Used**: `<JVisualVM/YourKit/Java Mission Control>`
- **Observed CPU time per request (ms)**: `<Number>`

### **Step 2: Compute Total CPU Needed**

#### **Formula:**

```
Total CPU = RPS × CPU time per request (in seconds)
```

#### **Calculation:**

```
CPU Required = <RPS> × (<CPU Time> / 1000)
```

✅ **Estimated CPU Required:** `<vCPUs>`

---

## **3️⃣ RAM Estimation**

### **Step 1: Measure RAM Usage Per Request**

- **Profiling Tool Used**: `<JProfiler/VisualVM/Eclipse Memory Analyzer>`
- **Observed RAM usage per request (MB)**: `<Number>`

### **Step 2: Compute Total RAM Required**

#### **Formula:**

```
Total RAM = RAM per request × Concurrent requests
```

#### **Calculation:**

```
RAM Required = <RAM Per Request> × <Concurrent Users>
```

✅ **Estimated RAM Required:** `<GB>`

---

## **4️⃣ Storage Estimation**

### **Database Storage (Cloud SQL)**

| Metric                                | Value      |
| ------------------------------------- | ---------- |
| **Average Data Size Per Record (KB)** | `<Number>` |
| **Total Records Stored**              | `<Number>` |
| **Growth Rate (records/month)**       | `<Number>` |
| **Index Overhead Factor**             | `2x`       |

#### **Formula:**

```
Total DB Storage = (Total Records × Data Size Per Record) × Index Overhead
```

✅ **Estimated DB Storage:** `<GB>`

### **Cloud Storage (File Storage)**

| Metric                     | Value      |
| -------------------------- | ---------- |
| **Number of Files**        | `<Number>` |
| **Average File Size (KB)** | `<Number>` |

#### **Formula:**

```
Total File Storage = Number of Files × Average File Size
```

✅ **Estimated File Storage:** `<GB/TB>`

---

## **5️⃣ Network Bandwidth Estimation**

### **Step 1: Compute Data Transfer Per Request**

| Metric                              | Value      |
| ----------------------------------- | ---------- |
| **Data Sent (KB per request)**      | `<Number>` |
| **Data Received (KB per response)** | `<Number>` |
| **Total Data Per Request (KB)**     | `<Sum>`    |

### **Step 2: Compute Bandwidth Needed**

#### **Formula:**

```
Bandwidth Required = RPS × Total Data Per Request
```

#### **Calculation:**

```
Bandwidth Required = <RPS> × <Total Data Per Request>
```

✅ **Estimated Bandwidth:** `<MBps/Gbps>`

---

## **6️⃣ Cost Estimation Using GCP Pricing Calculator**

| Service                  | Estimated Requirement | Estimated Cost |
| ------------------------ | --------------------- | -------------- |
| Compute Engine           | `<vCPUs & RAM>`       | `<Cost>`       |
| Cloud SQL                | `<DB Storage>`        | `<Cost>`       |
| Cloud Storage            | `<File Storage>`      | `<Cost>`       |
| Load Balancer            | `<Throughput>`        | `<Cost>`       |
| Cloud CDN                | `<Data Cached>`       | `<Cost>`       |
| **Total Estimated Cost** | **`<$Amount>`**       |                |

---

## **📌 Summary**

| Resource              | Formula                              | Estimated Requirement |
| --------------------- | ------------------------------------ | --------------------- |
| **CPU**               | `RPS × CPU time per request`         | `<vCPUs>`             |
| **RAM**               | `RAM per request × Concurrent users` | `<GB>`                |
| **Database Storage**  | `Data size × growth rate`            | `<GB>`                |
| **File Storage**      | `Files × size per file`              | `<GB/TB>`             |
| **Network Bandwidth** | `RPS × Data per request`             | `<MBps/Gbps>`         |

---

## **🚀 Final Takeaways**

✅ **Use profiling tools** (`JVisualVM`, `htop`, `pmap`) to measure real usage.
✅ **Use autoscaling** to handle peak loads efficiently.
✅ **Cloud CDN + Load Balancer** optimize network & reduce costs.
✅ **Monitor usage** with Cloud Monitoring & Profiler.
✅ **Optimize queries & caching** to reduce resource consumption.

---

## **Step-by-Step Example** (Spring Boot REST API: `POST /orders`)

### **1. Define API Requirements**

- **Requests Per Second (RPS):** `500`
- **Concurrent Users:** `1000`
- **Response Time:** `200ms`
- **Payload:** `Request: 2 KB, Response: 5 KB`

### **2. Measure CPU & RAM Usage**

- **Load Testing Tool:** `k6`
- **Profiling Tool:** `JVisualVM`
- **Observed CPU per request:** `15ms`
- **Observed RAM per request:** `0.5MB`

### **3. Compute Resources Needed**

- **CPU Required:** `(500 × 15) / 1000 = 7.5 vCPUs → 8 vCPUs`
- **RAM Required:** `0.5 MB × 1000 = 0.5 GB → 1 GB`

### **4. Storage Estimation**

- **DB Storage:** `150 GB`
- **File Storage:** `4 TB`

### **5. Bandwidth Calculation**

- **Bandwidth Required:** `500 × 7 KB = 3.5 MBps ≈ 30 Mbps`

### **6. Autoscaling Setup in GCP**

- **GKE Deployment:** `HorizontalPodAutoscaler (2–10 replicas)`
- **Compute Engine:** `Autoscaler (2–10 instances, 70% CPU target)`

Would you like a **spreadsheet version** for calculations? 🚀



Great! Let's go step by step.

---

## **Step 1: Define API Requirements**
We assume a Spring Boot REST API for processing customer orders.

| Metric | Value |
|--------|------|
| **API Name** | `Create Order API` |
| **Endpoint** | `/orders` |
| **Request Type** | `POST` |
| **Expected RPS (Requests Per Second)** | `500` |
| **Concurrent Users** | `1000` |
| **Average Response Time** | `200 ms` |
| **Payload Size (Request & Response)** | `Request: 2 KB, Response: 5 KB` |

---

## **Step 2: Measure CPU & RAM Usage**
### 2.1 **Run Load Tests**
To simulate traffic, use **Apache JMeter** or **k6** to send 500 RPS.
#### **k6 Load Test Script**
Save this as `load-test.js`:
```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  stages: [
    { duration: '30s', target: 500 }, // Ramp-up to 500 RPS
    { duration: '2m', target: 500 }, // Sustain load for 2 minutes
    { duration: '30s', target: 0 }   // Ramp-down
  ],
};

export default function () {
  let res = http.post('http://localhost:8080/orders', JSON.stringify({ item: "Laptop", quantity: 1 }), {
    headers: { 'Content-Type': 'application/json' },
  });

  check(res, { 'status was 200': (r) => r.status === 200 });
  sleep(1);
}
```
Run the test:
```sh
k6 run load-test.js
```
---

### 2.2 **Measure CPU & RAM Usage**
#### **Use JVisualVM for Profiling**
1. Start the Spring Boot application.
   ```sh
   mvn spring-boot:run
   ```
2. Open **JVisualVM**, attach it to the process.
3. Observe:
    - **CPU usage per request** (ms/request)
    - **Heap Memory usage per request** (MB/request)

#### **Get CPU & Memory Usage in Code**
Modify `application.properties`:
```properties
management.endpoints.web.exposure.include=metrics
management.metrics.export.prometheus.enabled=true
```
Use **Spring Actuator** to check resource consumption:
```sh
curl http://localhost:8080/actuator/metrics/system.cpu.usage
curl http://localhost:8080/actuator/metrics/jvm.memory.used
```

**Example Observed Values:**
- **CPU time per request:** `15 ms`
- **RAM per request:** `0.5 MB`

---

## **Step 3: Calculate Total Resource Needs**
### 3.1 **CPU Requirement**
**Formula:**
```
Total CPU (vCPUs) = (RPS × CPU time per request) / 1000
```
**Calculation:**
```
Total CPU = (500 × 15) / 1000 = 7.5 vCPUs
```
✅ **Estimated CPU Required: 8 vCPUs**

---

### 3.2 **RAM Requirement**
**Formula:**
```
Total RAM (GB) = RAM per request × Concurrent Users
```
**Calculation:**
```
Total RAM = 0.5 MB × 1000 = 500 MB (0.5 GB)
```
✅ **Estimated RAM Required: 1 GB (buffer included)**

---

## **Step 4: Storage Estimation**
### 4.1 **Database Storage (Cloud SQL)**
Assuming **100,000 orders per day**, each **2 KB record**.

**Formula:**
```
DB Storage = Orders per day × Size per order × Retention days × 2 (index overhead)
```
**Calculation:**
```
DB Storage = 100,000 × 2 KB × 365 × 2
           = 146 GB
```
✅ **Estimated DB Storage: 150 GB**

---

### 4.2 **Cloud Storage for File Uploads**
If each order has an **invoice PDF (100 KB)**:
```
Storage Needed = Orders per day × File Size × Retention days
```
```
Storage = 100,000 × 100 KB × 365
        = 3.65 TB
```
✅ **Estimated File Storage: 4 TB**

---

## **Step 5: Network Bandwidth Estimation**
| Metric | Value |
|--------|------|
| **Request Size** | 2 KB |
| **Response Size** | 5 KB |
| **Total Data per Request** | 7 KB |
| **Requests Per Second (RPS)** | 500 |

**Formula:**
```
Bandwidth (MBps) = RPS × Total Data Per Request
```
**Calculation:**
```
Bandwidth = 500 × 7 KB = 3.5 MBps (≈ 28 Mbps)
```
✅ **Estimated Bandwidth: 30 Mbps**

---

## **Step 6: Setup Autoscaling in GCP**
### **6.1 Deploying on GKE (Kubernetes)**
Create a `Deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: order-service
  template:
    metadata:
      labels:
        app: order-service
    spec:
      containers:
      - name: order-service
        image: gcr.io/my-project/order-service:latest
        resources:
          requests:
            cpu: "500m"
            memory: "512Mi"
          limits:
            cpu: "2"
            memory: "2Gi"
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: order-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: order-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```
Apply it:
```sh
kubectl apply -f Deployment.yaml
```
---

### **6.2 Autoscaling on Compute Engine**
Enable **auto-scaler** for an instance group:
```sh
gcloud compute instance-groups managed set-autoscaling my-instance-group \
  --max-num-replicas 10 \
  --min-num-replicas 2 \
  --target-cpu-utilization 0.7 \
  --cool-down-period 90
```

---

## **Final Summary**
| Resource | Estimated Requirement |
|----------|-----------------------|
| **CPU** | 8 vCPUs |
| **RAM** | 1 GB |
| **DB Storage** | 150 GB |
| **File Storage** | 4 TB |
| **Bandwidth** | 30 Mbps |
| **Autoscaling** | Min 2, Max 10 replicas |

---

## **Next Steps**
✅ **Run in production, monitor with GCP Logging & Metrics**  
✅ **Use Cloud CDN & Caching to reduce load**  
✅ **Tune DB queries & optimize indexes**

Would you like a **spreadsheet version** of this calculation? 🚀

# **Capacity Estimation Template for Java Web Applications**

## **📌 Overview**
Use this template to calculate resource usage for your Java web application APIs. Each section includes formulas and an example calculation for clarity.

---
## **1️⃣ API Details**
| Field | Value |
|--------|--------|
| **API Name** | `<API Name>` |
| **Endpoint** | `<Endpoint>` |
| **Request Type** | `GET/POST/PUT/DELETE` |
| **Expected RPS** | `<Number>` |
| **Concurrent Users** | `<Number>` |
| **Average Response Time (ms)** | `<Number>` |
| **Data Sent (KB per request)** | `<Number>` |
| **Data Received (KB per response)** | `<Number>` |

---
## **2️⃣ CPU Estimation**
### **Formula:**
```
Total CPU Required (vCPUs) = (RPS × Avg CPU time per request) / 1000
```
### **Example Calculation:**
- **RPS:** `500`
- **Avg CPU Time Per Request:** `15ms`
```
CPU Required = (500 × 15) / 1000
             = 7.5 → 8 vCPUs
```
✅ **Estimated CPU Required:** `8 vCPUs`

---
## **3️⃣ RAM Estimation**
### **Formula:**
```
Total RAM Required (GB) = RAM per request × Concurrent Users
```
### **Example Calculation:**
- **RAM Usage Per Request:** `0.5 MB`
- **Concurrent Users:** `1000`
```
RAM Required = 0.5 MB × 1000
             = 500 MB → 1 GB
```
✅ **Estimated RAM Required:** `1 GB`

---
## **4️⃣ Database Storage Estimation**
### **Formula:**
```
Total DB Storage = (Total Records × Data Size Per Record) × Index Overhead
```
### **Example Calculation:**
- **Total Records:** `2 Million`
- **Data Size Per Record:** `5 KB`
- **Index Overhead Factor:** `2x`
```
Total DB Storage = (2,000,000 × 5 KB) × 2
                  = 10 GB × 2
                  = 20 GB
```
✅ **Estimated DB Storage:** `20 GB`

---
## **5️⃣ File Storage Estimation**
### **Formula:**
```
Total File Storage = Number of Files × Average File Size (KB)
```
### **Example Calculation:**
- **Number of Files:** `100,000`
- **Average File Size:** `500 KB`
```
Total File Storage = 100,000 × 500 KB
                    = 50 GB
```
✅ **Estimated File Storage:** `50 GB`

---
## **6️⃣ Network Bandwidth Estimation**
### **Formula:**
```
Bandwidth Required (MBps) = RPS × (Data Sent + Data Received)
```
### **Example Calculation:**
- **RPS:** `500`
- **Data Sent per Request:** `2 KB`
- **Data Received per Response:** `5 KB`
```
Bandwidth Required = 500 × (2 KB + 5 KB)
                   = 500 × 7 KB
                   = 3.5 MBps → 30 Mbps
```
✅ **Estimated Bandwidth:** `30 Mbps`

---
## **7️⃣ Connection Pool Size Estimation**
### **Formula:**
```
Max Connections = (RPS × Avg DB Query Time) / Number of Threads
```
### **Example Calculation:**
- **RPS:** `500`
- **Avg DB Query Time:** `50ms (0.05s)`
- **Threads:** `50`
```
Max Connections = (500 × 0.05) / 50
                 = 25 / 50
                 = 12 connections
```
✅ **Estimated Connection Pool Size:** `12 connections`

---
## **8️⃣ Cost Estimation Using GCP Pricing Calculator**
| Service | Estimated Requirement | Estimated Cost |
|---------|-----------------------|---------------|
| Compute Engine | `<vCPUs & RAM>` | `<Cost>` |
| Cloud SQL | `<DB Storage>` | `<Cost>` |
| Cloud Storage | `<File Storage>` | `<Cost>` |
| Load Balancer | `<Throughput>` | `<Cost>` |
| Cloud CDN | `<Data Cached>` | `<Cost>` |
| **Total Estimated Cost** | **`<$Amount>`** |

---
## **📌 Summary**
| Resource | Formula | Estimated Requirement |
|----------|--------|-----------------------|
| **CPU** | `RPS × CPU time per request` | `<vCPUs>` |
| **RAM** | `RAM per request × Concurrent users` | `<GB>` |
| **Database Storage** | `Data size × growth rate` | `<GB>` |
| **File Storage** | `Files × size per file` | `<GB/TB>` |
| **Network Bandwidth** | `RPS × Data per request` | `<MBps/Gbps>` |
| **Connection Pool Size** | `(RPS × DB Query Time) / Threads` | `<Connections>` |

---
## **🚀 Final Takeaways**
✅ **Use profiling tools** (`JVisualVM`, `htop`, `pmap`) to measure real usage.  
✅ **Use autoscaling** to handle peak loads efficiently.  
✅ **Cloud CDN + Load Balancer** optimize network & reduce costs.  
✅ **Monitor usage** with Cloud Monitoring & Profiler.  
✅ **Optimize queries & caching** to reduce resource consumption.

Would you like a working **Spring Boot `POST /orders` API** example with code for testing these calculations? 🚀

