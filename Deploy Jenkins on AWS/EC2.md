### **What is EC2 and Why is it Important?**

**EC2 (Elastic Compute Cloud)** is a service from AWS that allows you to run virtual servers, called **instances**, in the cloud. These instances provide the **compute power** (CPU), **memory (RAM)**, and **storage** (disk) needed to run applications.

- **Scalability**: You can scale up or down based on your needs. If your application needs more resources, you can easily increase them, and if it needs less, you can reduce them. This flexibility helps save costs and ensures your application performs well.
  
- **Security**: EC2 is designed with security in mind. It uses the **AWS Nitro System** to provide a secure environment for your applications.

- **Performance and Cost Optimization**: EC2 offers flexible pricing options like **Spot Instances** (which let you bid on unused capacity) and **Savings Plans**, which help you reduce costs while optimizing performance.

---

### **EC2 Use Cases**

- **Web Servers**: You can use EC2 to host websites or web applications.
  
- **High-Performance Computing (HPC)**: EC2 can be used for compute-intensive tasks like scientific modeling or simulation.
  
- **Machine Learning**: EC2 provides optimized resources to run ML workloads, benefiting from AWS’s high-speed networking and scalable storage.

- **On-Demand Infrastructure**: If you need extra compute capacity for short periods, EC2 gives you the flexibility to quickly scale as per your demand, paying only for what you use.

# EC2 Instance Types

Recommended to follow [this](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) page for very detailed and updated information.

- General purpose


General Purpose instances are designed to deliver a balance of compute, memory, and network resources. They are suitable for a wide range of applications, including web servers,
small databases, development and test environments, and more.


- Compute optimized


Compute Optimized instances provide a higher ratio of compute power to memory. They excel in workloads that require high-performance processing such as batch processing, 
scientific modeling, gaming servers, and high-performance web servers.


- Memory optimized


Memory Optimized instances are designed to handle memory-intensive workloads. They are suitable for applications that require large amounts of memory, such as in-memory databases,
real-time big data analytics, and high-performance computing.


- Storage optimized


Storage Optimized instances are optimized for applications that require high, sequential read and write access to large datasets. 
They are ideal for tasks like data warehousing, log processing, and distributed file systems.


- Accelerated computing


Accelerated Computing Instances typically come with one or more types of accelerators, such as Graphics Processing Units (GPUs),
Field Programmable Gate Arrays (FPGAs), or custom Application Specific Integrated Circuits (ASICs). 
These accelerators offload computationally intensive tasks from the main CPU, enabling faster and more efficient processing for specific workloads.

# Instance families

    C – Compute

    D – Dense storage

    F – FPGA

    G – GPU

    Hpc – High performance computing

    I – I/O

    Inf – AWS Inferentia

    M – Most scenarios

    P – GPU

    R – Random access memory

    T – Turbo

    Trn – AWS Tranium

    U – Ultra-high memory

    VT – Video transcoding

    X – Extra-large memory

# Additional capabilities

    a – AMD processors

    g – AWS Graviton processors

    i – Intel processors

    d – Instance store volumes

    n – Network and EBS optimized

    e – Extra storage or memory

    z – High performance
                                                         