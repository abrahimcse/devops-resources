# Amazon CloudFront: Complete Guide

## **1. What is Amazon CloudFront?**
Amazon CloudFront is a **Content Delivery Network (CDN)** that accelerates the delivery of static and dynamic content (e.g., websites, APIs, videos) using AWS's global network of **edge locations**.

### **Key Features**
- **Low Latency**: 310+ edge locations worldwide.
- **Integration**: Works with S3, EC2, ALB, and custom origins.
- **Security**: DDoS protection, AWS WAF, HTTPS support.
- **Cost-Effective**: Pay-as-you-go pricing.

---

## **2. Core Concepts**
| Component          | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **Distribution**   | Configuration for content delivery (Web or RTMP).                           |
| **Origin**         | Source of content (S3, EC2, or custom HTTP server).                         |
| **Edge Locations** | AWS servers that cache content closer to users.                             |
| **Cache Behavior** | Rules for how CloudFront caches and serves requests (e.g., TTL settings).   |

---

## **3. Setup & Configuration**
### **Prerequisites**
- AWS account with IAM permissions.
- Origin (e.g., S3 bucket or web server).

### **Steps to Create a Distribution**
1. **Navigate to CloudFront** in the AWS Console.
2. Click **Create Distribution**.
3. Choose **Web** (for HTTP/S) or **RTMP** (for streaming).
4. **Configure Origin**:
   - Enter origin domain (e.g., `my-bucket.s3.amazonaws.com`).
   - Enable **Origin Access Control (OAC)** for S3.
5. **Cache Behavior**:
   - Set default TTL (e.g., 24 hours).
   - Restrict HTTP methods (e.g., `GET, HEAD`).
6. **Distribution Settings**:
   - Enable HTTPS (using AWS ACM).
   - Add custom domain (`cdn.example.com`).
7. Deploy (takes ~15–20 minutes).

---

## **4. Advanced Features**
### **Lambda@Edge**
- Run serverless functions at edge locations to modify requests/responses.
- Use cases: A/B testing, authentication, header modification.

### **Cache Optimization**
- **Invalidation**: Manually clear cached content via AWS Console or CLI.
- **Cache Policies**: Predefined or custom rules for TTL and query strings.

### **Security**
- **Signed URLs/Cookies**: Restrict access to private content.
- **AWS WAF Integration**: Block malicious traffic.
- **Geo-Restriction**: Allow/block countries.

---

## **5. Use Cases**
- **Static Websites**: Hosted on S3 + CloudFront.
- **Dynamic Content**: APIs with Lambda@Edge.
- **Video Streaming**: HLS/DASH for on-demand videos.

---

## **6. Troubleshooting**
| Issue               | Solution                                      |
|---------------------|-----------------------------------------------|
| High latency        | Check edge location coverage; enable HTTP/2. |
| 403 Forbidden       | Verify OAC/S3 bucket policies.               |
| SSL errors          | Ensure ACM certificate is in `us-east-1`.    |

---

## **7. Pricing**
- **Data Transfer Out**: $0.085–0.25/GB (varies by region).
- **HTTP Requests**: $0.0075–0.02 per 10,000 requests.
- **Free Tier**: 1TB/month data transfer.

> Refer to [AWS CloudFront Docs](https://docs.aws.amazon.com/cloudfront/) for updates.
