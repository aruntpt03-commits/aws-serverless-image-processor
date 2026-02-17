AWS Serverless Image Processor

An end-to-end **event-driven serverless image processing pipeline** built using AWS Lambda, Amazon S3, Docker, and Terraform.
The system automatically processes images uploaded to S3 and generates multiple optimized variants.

Project Overview

This project demonstrates a production-style serverless architecture where image uploads automatically trigger processing workflows.

Workflow:
- User uploads image → Source S3 bucket
- S3 event triggers Lambda
- Lambda processes image using Pillow
- Multiple optimized formats are generated
- Processed images stored in destination bucket
- Logs and metrics sent to CloudWatch

✅ Fully automated
✅ Infrastructure as Code (Terraform)
✅ Docker-based Lambda layer build
✅ Secure and production-ready design

Architecture Diagram

![Architecture]

<img width="1536" height="1024" alt="architecture" src="https://github.com/user-attachments/assets/81e1015a-0737-43c9-a45a-0e5444c84fcb" />


Tech Stack

Cloud Services

* AWS S3
* AWS Lambda (Python 3.12)
* AWS IAM
* AWS CloudWatch

DevOps & IaC

* Terraform
* Docker
* Bash scripting

Programming

* Python
* Pillow (PIL)

End-to-End Workflow

1. User uploads an image to the **upload bucket**
2. Amazon S3 emits an **ObjectCreated event**
3. Event triggers the **Lambda function**
4. Lambda:
   * Downloads image in memory
   * Validates and converts format
   * Resizes large images
   * Generates multiple variants
   * Creates thumbnail
5. Processed images uploaded to **processed bucket**
6. Execution logs stored in **CloudWatch**

Image Variants Generated

For every uploaded image, the system produces:

* Compressed JPEG (quality 85)
* Low-quality JPEG (quality 60)
* WebP optimized version
* PNG version
* Thumbnail (300×300)

Security Best Practices Implemented

* 🚫 S3 public access fully blocked
* 🔒 Server-side encryption enabled (AES256)
* 🎯 Least-privilege IAM policies
* 🧩 Lambda dependency isolated via Layer
* 📉 CloudWatch log retention configured
* 🧹 Sensitive files excluded via `.gitignore`

Project Structure

```
aws-serverless-image-processor/
├── Terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── providers.tf
├── lambda/
│   └── lambda_function.py
├── scripts/
│   ├── deploy.sh
│   └── build_layer_docker.sh
├── docs/
│   └── images/
│       ├── architecture.gif
│       ├── upload-bucket.png
│       ├── processed-output.png
│       └── cloudwatch-logs.png
├── .gitignore
└── README.md
```

 Deployment Instructions

### Prerequisites

* AWS CLI configured
* Terraform installed
* Docker installed and running
* Proper AWS permissions

### Deploy the Infrastructure

```bash
cd scripts
bash deploy.sh
```

This will automatically:

* Build Lambda Pillow layer via Docker
* Initialize Terraform
* Provision AWS resources
* Configure S3 trigger
* Output bucket names

---

### Test the Pipeline

Upload an image:

```bash
aws s3 cp your-image.jpg s3://<upload-bucket>/
```

Within seconds, processed images will appear in the processed bucket.

---

## Sample Output

### Upload Bucket
<img width="1142" height="480" alt="S3 upload bucket" src="https://github.com/user-attachments/assets/33127b52-5981-4b0e-9a28-aab211de5549" />



### Processed Images Generated

<img width="1197" height="541" alt="Processed images in destination bucket" src="https://github.com/user-attachments/assets/1f28053c-c74f-41ba-97db-cd27ead4e3c6" />

### Lambda Execution Logs

<img width="1201" height="458" alt="Lambda CloudWatch logs" src="https://github.com/user-attachments/assets/0458a3bb-1fe1-43bc-8c77-36eb3965e249" />

Performance Characteristics

* ⚡ Average processing time: ~2–3 seconds
* 🧠 Memory allocated: 1024 MB
* 📦 In-memory image processing (no disk usage)
* 🔁 Supports batch S3 events

 Key Design Decisions

### Why Lambda Layer?

* Smaller function package
* Reusable dependency
* Faster deployments
* Cleaner separation of concerns

### Why Docker for Layer Build?

AWS Lambda requires **Linux-compatible binaries**.
Docker ensures Pillow is compiled for:

* linux/amd64
* Python 3.12 runtime
* Lambda compatibility

---

### Why Two Separate Buckets?

* Security isolation
* Clear data lifecycle
* Easier access control
* Reduced blast radius

