### **Project Title: Monitoring EC2 CPU Utilization with CloudWatch and Email Alerts**  

---

## **Project Purpose**  
In this project, we will monitor the **CPU utilization** of an **EC2 instance (Ubuntu)** using **AWS CloudWatch Metrics**. If CPU utilization **crosses 70%**, CloudWatch will trigger an **alarm** that sends an **email notification** via **Amazon SNS (Simple Notification Service)**.  

---

## **What Are We Doing in This Project?**  
1. **Set up CloudWatch Metrics** to track CPU utilization of an EC2 instance.  
2. **Create a CloudWatch Alarm** that triggers when CPU usage exceeds 70%.  
3. **Integrate Amazon SNS** to send an email notification when the alarm is triggered.  
4. **Use Python script** to intentionally **spike CPU usage** on EC2 to test our monitoring setup.  

---

## **Use of This Project**  
✅ Helps in **monitoring server performance** and avoiding downtime.  
✅ **Prevents overload issues** by notifying administrators before crashes occur.  
✅ Can be extended to **auto-scale instances** based on CPU utilization.  
✅ Useful for **system administrators, DevOps engineers, and cloud architects** to automate monitoring.  

---

## **Steps to Create the Project**  

### **Step 1: Launch an EC2 Instance (Ubuntu)**  
1. Go to **AWS Management Console** → **EC2** → **Launch Instance**.  
2. Choose **Ubuntu** as the AMI.  
3. Select an instance type (e.g., **t2.micro**).  
4. Allow inbound rules for **SSH (Port 22)** in the security group.  
5. Click **Launch** and connect to the instance via SSH.  

---

### **Step 2: Install CloudWatch Agent (Optional, for Additional Metrics)**  
1. **Connect to your EC2 instance** via SSH:  
   ```bash
   ssh -i your-key.pem ubuntu@your-ec2-instance-ip
   ```  
2. **Install CloudWatch Agent** (if not installed):  
   ```bash
   sudo apt update
   sudo apt install -y amazon-cloudwatch-agent
   ```  
3. **Start the agent (optional, for advanced monitoring):**  
   ```bash
   sudo systemctl start amazon-cloudwatch-agent
   ```  

---

### **Step 3: Create a CloudWatch Alarm for CPU Utilization**  
1. Go to **AWS Console** → **CloudWatch** → **Alarms**.  
2. Click **Create Alarm**.  
3. **Select Metric:**  
   - Choose **EC2 Metrics** → **Per-Instance Metrics**.  
   - Find **CPUUtilization** for your EC2 instance.  
4. **Set Alarm Conditions:**  
   - **Threshold Type:** Static  
   - **Condition:** Greater than **70%**  
5. **Configure Actions:**  
   - Select **"Create new SNS topic"**.  
   - Give it a name (e.g., `CPU-High-Alert`).  
   - Enter your **email address** for notifications.  
   - Click **Create topic**.  
6. **Confirm SNS Subscription:**  
   - Check your email and **confirm the SNS subscription**.  
7. Click **Create Alarm**.  

---

### **Step 4: Run Python Script to Simulate High CPU Usage**  
1. **Install Python** if not already installed:  
   ```bash
   sudo apt install -y python3
   ```  
2. **Create a Python script:**  
   ```bash
   nano cpu_spike.py
   ```  
3. **Copy and paste the following code:**  
   ```python
   import time

   def simulate_cpu_spike(duration=30, cpu_percent=80):
       print(f"Simulating CPU spike at {cpu_percent}%...")
       start_time = time.time()

       target_percent = cpu_percent / 100
       total_iterations = int(target_percent * 5_000_000)  

       for _ in range(total_iterations):
           result = 0
           for i in range(1, 1001):
               result += i

       elapsed_time = time.time() - start_time
       remaining_time = max(0, duration - elapsed_time)
       time.sleep(remaining_time)

       print("CPU spike simulation completed.")

   if __name__ == '__main__':
       simulate_cpu_spike(duration=30, cpu_percent=80)
   ```  
4. **Save the file and exit nano (Ctrl+X → Y → Enter).**  
5. **Run the script:**  
   ```bash
   python3 cpu_spike.py
   ```  
6. **Watch CloudWatch Metrics** in AWS Console.  

---

### **Step 5: Receive Alert Notification**  
1. Once **CPU usage exceeds 70%**, the CloudWatch **alarm will trigger**.  
2. The SNS topic will **send an email notification**.  
3. Check your **email inbox** for the alert.  

---

## **Conclusion**  
✅ Successfully **monitored EC2 CPU usage** with CloudWatch.  
✅ Created an **alarm** to send **email alerts** when CPU exceeded 70%.  
✅ Used a **Python script** to spike CPU and **test the alert system**.  

This project **ensures proactive monitoring** and can be expanded for **auto-scaling** or integrating with **Lambda for automated responses**. 🚀