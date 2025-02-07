### **Project: Deploy a Static Website Using Amazon S3 and CloudFront**  

#### **Project Purpose:**  
In this project, we will host a **static website** on **Amazon S3** and use **Amazon CloudFront** as a Content Delivery Network (CDN) to distribute content efficiently and securely. This setup improves website performance, reduces latency, and enhances security.  

---

## **Project Steps**  

### **Step 1: Create an S3 Bucket for Static Website Hosting**  
1. **Go to AWS Console** → Open **S3**  
2. Click **Create bucket**  
   - **Bucket name:** `my-cloudfront-website` (must be unique)  
   - **Region:** Choose a region (e.g., `us-east-1`)  
   - **Uncheck** "Block all public access" (to allow website access)  
   - Click **Create bucket**  

---

### **Step 2: Upload Website Files to S3**  
1. Open the S3 bucket **my-cloudfront-website**  
2. Click **Upload** → Add your `index.html` and any other files  
3. Click **Upload**  

📌 **Example `index.html` file:**  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CloudFront Website</title>
</head>
<body>
    <h1>Welcome to My CloudFront Website</h1>
    <p>This website is served via Amazon S3 and distributed using CloudFront.</p>
</body>
</html>
```

---

### **Step 3: Enable Static Website Hosting in S3**  
1. Go to **S3 Console** → Select your **bucket**  
2. Click on **Properties** → Find **Static website hosting**  
3. Click **Edit** → Select **Enable**  
4. Set the **Index document** as `index.html`  
5. Click **Save changes**  

✅ Now, you will get an **S3 Website Endpoint** like:  
```
http://my-cloudfront-website.s3-website-us-east-1.amazonaws.com
```
Try opening this URL in your browser to test if the site is accessible.

---

### **Step 4: Update Bucket Policy for Public Read Access**  
1. Go to **S3 Console** → Select **Bucket**  
2. Click on **Permissions** → Scroll to **Bucket Policy**  
3. Add the following policy to allow public access:  

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-cloudfront-website/*"
        }
    ]
}
```
4. Click **Save changes**  

---

### **Step 5: Create a CloudFront Distribution**  
1. Go to **AWS CloudFront Console**  
2. Click **Create Distribution**  
3. In the **Origin Settings**:  
   - **Origin Domain:** Enter your **S3 Website Endpoint**  
   - **Origin Protocol Policy:** Select `HTTP Only`  
4. In the **Default Cache Behavior**:  
   - **Viewer Protocol Policy:** Select `Redirect HTTP to HTTPS`  
5. **Leave other settings as default**  
6. Click **Create Distribution**  

✅ After a few minutes, you will get a **CloudFront Distribution Domain Name**, like:  
```
https://d123456789abcdef.cloudfront.net
```
Try accessing this URL in your browser. It should serve the website from CloudFront.

---

### **Step 6: Test the CloudFront Distribution**  
- Open `https://d123456789abcdef.cloudfront.net` in your browser.  
- The website should load from **CloudFront’s edge locations** instead of the original S3 bucket.  

---

### **Step 7: (Optional) Invalidate Cache for Updates**  
If you update your website files but CloudFront still shows the old version, you need to **clear the cache**:  
1. **Go to CloudFront Console**  
2. Click on your **distribution**  
3. Go to **Invalidations** → Click **Create Invalidation**  
4. Enter `/*` → Click **Invalidate**  

---

### **Benefits of Using CloudFront with S3**  
✅ **Faster Website Load Times** – Delivers content from nearest edge location  
✅ **Reduced Latency** – Users don’t need to fetch files from a distant S3 bucket  
✅ **Better Security** – Can integrate with AWS WAF for additional protection  
✅ **Cost Optimization** – Reduces bandwidth costs by caching content  

---

### **Conclusion**  
Now, you have successfully deployed a static website on **Amazon S3** and accelerated it using **Amazon CloudFront**! 🚀