<p align="center">
  <img src="GithubBanner.png" alt="Wallet Watcher GitHub Banner" width="100%">
</p>

<h1 align="center">Wallet Watcher</h1>

<h3 align="center">Cloud-Native Expense Tracker | AWS Static Website Hosting</h3>

<p align="center">
  A personal finance application demonstrating secure AWS hosting,
  content delivery, and automated deployment.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Cloud-AWS-orange?logo=amazonaws" alt="AWS">
  <img src="https://img.shields.io/badge/Storage-Amazon%20S3-569A31?logo=amazons3" alt="Amazon S3">
  <img src="https://img.shields.io/badge/CDN-CloudFront-8C4FFF?logo=amazoncloudfront" alt="Amazon CloudFront">
  <img src="https://img.shields.io/badge/Scripting-Bash-4EAA25?logo=gnubash" alt="Bash">
  <img src="https://img.shields.io/badge/Frontend-HTML%20%7C%20CSS%20%7C%20JavaScript-E34F26" alt="Frontend">
</p>

---

## 📌 Project Overview

**Wallet Watcher** is a responsive personal finance application designed to help users track income, expenses, and their overall balance through an interactive interface.

Beyond frontend development, this project demonstrates how a static web application can be hosted on AWS using **Amazon S3 and Amazon CloudFront**, with an emphasis on secure origin access, efficient content delivery, and automated deployment operations.

The application uses HTML, CSS, and JavaScript for its frontend functionality, while browser `localStorage` provides client-side data persistence.

The cloud infrastructure is designed to keep the S3 bucket private and deliver application content through CloudFront. Bash scripting and AWS CLI commands simplify the process of synchronizing website files and invalidating cached content after updates.

### Key Highlights

- **Secure Static Hosting:** Stores website assets in a private Amazon S3 bucket.
- **Global Content Delivery:** Uses Amazon CloudFront to cache and distribute static content.
- **Restricted Origin Access:** Uses Origin Access Control (OAC) to authorize CloudFront access to the private S3 origin.
- **Automated Deployment Operations:** Uses Bash scripting and AWS CLI commands to synchronize files and invalidate CloudFront cache.
- **Repeatable Infrastructure Configuration:** Applies Infrastructure as Code principles to reduce repetitive manual configuration.
- **Client-Side Data Persistence:** Uses browser `localStorage` to retain application data locally.
- **Responsive Interface:** Provides a dark-themed interface designed for desktop and mobile devices.

---

## 🎯 Problem Statement

Deploying a web application involves more than developing a functional user interface. The application must also be hosted appropriately, delivered efficiently, protected against unnecessary exposure, and updated through a maintainable deployment process.

A basic static website deployment can introduce several challenges:

| Challenge | Problem |
|---|---|
| Publicly accessible storage | Exposing the storage origin unnecessarily increases the attack surface. |
| Inefficient content delivery | Users located far from the origin may experience additional latency. |
| Manual deployments | Repeated file uploads increase operational effort and the possibility of human error. |
| Stale cached content | Users may continue receiving older versions of files after an update. |
| Inconsistent infrastructure configuration | Repeated manual changes can lead to configuration drift. |

### Project Objective

Design and implement an AWS-hosted static application that addresses these challenges through:

- Private object storage using Amazon S3.
- Content delivery and caching using Amazon CloudFront.
- Controlled origin access using Origin Access Control.
- Scripted deployment operations using Bash and AWS CLI.
- Repeatable infrastructure configuration following Infrastructure as Code principles.

---

## 💡 Solution Approach

Wallet Watcher combines a lightweight frontend with AWS-managed storage and content delivery services.

The solution focuses on four areas.

### 1. Secure Static Website Hosting

Amazon S3 stores the application's HTML, CSS, JavaScript, and other static assets.

The bucket is configured for private access rather than serving website files directly to anonymous users. CloudFront accesses the origin through Origin Access Control, subject to the configured S3 bucket policy.

### 2. Efficient Content Delivery

Amazon CloudFront acts as the application's content delivery layer.

It caches eligible static assets at edge locations and serves cached responses to users. When content is not available in the cache or requires revalidation, CloudFront retrieves the necessary content from the S3 origin according to its cache configuration.

This reduces repeated origin requests and helps improve content delivery performance.

### 3. Automated Deployment Operations

A Bash deployment template uses AWS CLI commands to automate important deployment tasks.

The workflow includes:

- Synchronizing application files with Amazon S3.
- Requesting CloudFront cache invalidation after updates.
- Reducing repetitive manual deployment steps.

### 4. Repeatable Infrastructure Configuration

Infrastructure-related configuration and deployment operations are handled programmatically where applicable.

This approach helps reduce reliance on manual setup and provides a foundation for extending the project with dedicated Infrastructure as Code tools and CI/CD pipelines.

---

## 🏗️ System Architecture

<p align="center">
  <img src="S3%2BCloudFront_Architecture.png" alt="Wallet Watcher AWS Architecture Diagram" width="100%">
</p>

### Architecture Components

| Component | Responsibility |
|---|---|
| Client Devices | Access the application through a web browser. |
| Internet | Provides connectivity between users and the application delivery endpoint. |
| Amazon CloudFront | Delivers static content over HTTPS and caches eligible assets at edge locations. |
| Origin Access Control (OAC) | Allows the configured CloudFront distribution to access the private S3 origin. |
| Amazon S3 | Stores static website assets, including HTML, CSS, and JavaScript files. |
| GitHub | Hosts the source code and provides version control. |
| AWS CLI | Executes AWS operations required for deployment and cache invalidation. |
| Bash Deployment Script | Automates the deployment operations defined in the script. |

### Request Flow

The application follows this request flow:

**Step 1: User Request**

A user opens the application through its configured CloudFront distribution domain or associated custom domain.

**Step 2: CloudFront Receives the Request**

The request reaches Amazon CloudFront, which handles content delivery over HTTPS when the distribution is configured accordingly.

**Step 3: Cache Evaluation**

CloudFront checks whether the requested asset can be served from its cache.

If an eligible cached response is available, CloudFront serves it without retrieving the asset from S3 for that request.

**Step 4: Fetch from the S3 Origin**

When the requested content needs to be retrieved from the origin, CloudFront accesses the private S3 bucket through the configured Origin Access Control and bucket policy.

**Step 5: Deliver Content**

CloudFront returns the requested content to the user. Eligible responses may also be cached according to the distribution's cache behavior.

### Security Design

The architecture is designed to prevent direct public access to the S3 origin.

- **Private S3 Bucket:** Public access is blocked, and the bucket is not intended to serve files directly to anonymous users.
- **Origin Access Control:** The bucket policy authorizes the intended CloudFront distribution to access the origin.
- **HTTPS Delivery:** CloudFront supports secure content delivery when the distribution, certificate, and domain settings are configured correctly.
- **Controlled Origin Access:** Users access the application through its intended delivery endpoint rather than bypassing CloudFront to retrieve files directly from S3.

> **Security note:** OAC must be configured together with an appropriate S3 bucket policy and CloudFront origin settings. HTTPS delivery also depends on the distribution and certificate configuration. Verify these controls against the deployed AWS environment.

---

## 🔄 Automated Deployment Workflow

A key engineering objective of this project is to reduce repetitive deployment operations.

Instead of manually uploading website files and clearing cached content after every update, the project uses a Bash deployment template with AWS CLI commands.

### Deployment Flow

```text
Developer
    |
    | Modify application code
    v
GitHub Repository
    |
    | Prepare updated website files
    v
deploy.sh.template
    |
    | AWS CLI: aws s3 sync
    v
Private Amazon S3 Bucket
    |
    | AWS CLI: create-invalidation
    v
Amazon CloudFront
    |
    v
Updated Content Delivered to Users
```

### Deployment Operations

#### 1. Synchronize Website Files

The AWS CLI `s3 sync` command synchronizes files from a local directory to an S3 bucket.

Example:

```bash
aws s3 sync ./ s3://YOUR_BUCKET_NAME \
  --exclude ".git/*" \
  --exclude "deploy.sh.template"
```

**How it works:**

- Compares files in the local directory with those in the destination.
- Uploads new and changed files as required.
- Avoids uploading files excluded by the specified patterns.
- Reduces the need to upload every file manually.

Replace `YOUR_BUCKET_NAME` with the target bucket name.

The source directory and exclusion patterns should be adjusted to match the actual project structure.

#### 2. Invalidate CloudFront Cache

After deploying updated files, the following command requests a cache invalidation:

```bash
aws cloudfront create-invalidation \
  --distribution-id YOUR_DISTRIBUTION_ID \
  --paths "/*"
```

**How it works:**

- Identifies the CloudFront distribution.
- Requests invalidation of cached content matching the specified paths.
- Allows subsequent requests to retrieve updated content from the origin when appropriate.

Replace `YOUR_DISTRIBUTION_ID` with the actual distribution ID.

Invalidation is useful when updated content must become available without waiting for the existing cache entries to expire naturally.

### Benefits of Deployment Automation

- Reduces repetitive manual file uploads.
- Makes deployment operations easier to reproduce.
- Helps prevent outdated content from remaining cached after an update.
- Provides a foundation for more advanced CI/CD workflows.
- Improves maintainability as the application evolves.

> **Implementation note:** The repository contains a deployment template named `deploy.sh.template`. The commands above illustrate its deployment operations. Review the actual template for the complete implementation, configuration requirements, and execution order. A script alone does not imply that every GitHub push automatically triggers deployment; that requires a configured trigger, such as a GitHub Actions workflow.

---

## 🛠️ Technology Stack

### Frontend Technologies

| Technology | Purpose |
|---|---|
| HTML5 | Defines the application structure. |
| CSS3 | Implements the responsive layout and dark theme. |
| Vanilla JavaScript | Handles application interactions, calculations, and client-side logic. |
| Browser localStorage | Persists application data locally in the browser. |

### AWS and Cloud Engineering

| Technology | Purpose |
|---|---|
| Amazon S3 | Stores static website assets. |
| Amazon CloudFront | Provides content delivery, caching, and HTTPS support. |
| Origin Access Control | Restricts access to the S3 origin through the intended CloudFront distribution. |
| AWS CLI | Executes cloud deployment and cache-management operations. |
| Bash | Automates deployment operations. |
| Infrastructure as Code Principles | Encourages repeatable, programmatic infrastructure configuration. |
| Git and GitHub | Provide version control and source-code management. |

---

## ✨ Application Features

### 💰 Personal Finance Tracking

- Tracks income and expenses.
- Calculates the total balance dynamically.
- Displays financial summaries based on recorded transactions.
- Provides an interactive interface for managing personal finance records.

### 💾 Client-Side Data Persistence

The application uses browser `localStorage` to retain financial records locally.

Benefits include:

- Data persistence between browser sessions.
- No requirement for a remote database for the implemented client-side functionality.
- Local storage of application records within the user's browser.

**Limitation:** Browser localStorage is specific to the browser and device. Data is not automatically synchronized across devices, and localStorage should not be treated as a secure financial database.

### 📱 Responsive User Interface

- Dark-themed interface.
- Responsive layouts for desktop and mobile screens.
- Interactive client-side calculations.
- Lightweight frontend implementation without requiring a backend for the existing functionality.

---

## ⚙️ Implementation Details

The project separates the application, hosting, and deployment responsibilities.

### Application Layer

The frontend is implemented using HTML, CSS, and JavaScript.

JavaScript handles the application's calculations and interface interactions, while browser localStorage provides client-side persistence.

### Hosting Layer

Amazon S3 stores the static application files.

Amazon CloudFront provides the content delivery endpoint, handles eligible cached responses, and retrieves content from the configured S3 origin through OAC.

### Deployment Layer

Bash scripting and AWS CLI commands automate file synchronization and CloudFront cache invalidation.

The deployment process is designed to reduce repetitive manual tasks and make updates easier to reproduce.

### Infrastructure Configuration

Infrastructure-related configuration is handled programmatically where applicable.

This follows Infrastructure as Code principles by encouraging repeatable configuration and reducing dependence on manual setup.

The project does not require an application server or remote database for its existing client-side functionality.

---

## 🚀 Local Setup

### Prerequisites

- Git
- A modern web browser
- AWS CLI for deployment operations
- Appropriate AWS credentials and permissions if deploying to your own AWS account

### Step 1: Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
```

Navigate to the project directory:

```bash
cd YOUR_PROJECT_DIRECTORY
```

Replace the placeholders with the actual repository URL and directory name.

### Step 2: Run the Application Locally

Open `index.html` in your browser.

If the application requires an HTTP server because of its asset paths or browser restrictions, serve the project directory using an appropriate local development server.

### Step 3: Configure AWS CLI

If deploying your own copy, configure the AWS CLI using an authentication method appropriate for your environment.

```bash
aws configure
```

Use this interactive setup only if it matches your authentication method. Prefer temporary credentials or AWS IAM Identity Center where appropriate.

Never commit AWS access keys, secret keys, or other credentials to the repository.

### Step 4: Configure Deployment Settings

Review `deploy.sh.template` and provide the required deployment configuration, including:

- Target S3 bucket name.
- CloudFront distribution ID.
- Source directory for the website files.
- Required AWS CLI commands and permissions.

Ensure that the configuration matches the resources in your AWS account.

### Step 5: Deploy the Application

Follow the instructions in the deployment template to synchronize the website files with S3 and request CloudFront cache invalidation.

Before executing the deployment process, verify the target AWS account, bucket name, distribution ID, and required IAM permissions.

---

## 🔐 Security Considerations

Security is an important part of the hosting architecture.

- Keep the S3 bucket private and maintain Block Public Access settings.
- Configure the S3 bucket policy to authorize the intended CloudFront distribution through OAC.
- Apply least-privilege IAM permissions to deployment identities.
- Avoid hardcoding AWS credentials in scripts or configuration files.
- Keep deployment templates free of account-specific secrets.
- Review uploaded files to prevent accidental exposure of sensitive configuration or development files.
- Configure HTTPS and verify the distribution's certificate and domain settings.
- Remember that client-side localStorage is not suitable for sensitive financial records requiring strong confidentiality or server-side access controls.

---

## 🧠 Engineering Decisions and Trade-offs

| Decision | Reasoning | Trade-off |
|---|---|---|
| Static frontend | Keeps the application lightweight and avoids unnecessary backend infrastructure. | Requires backend development for shared accounts, centralized storage, and server-side processing. |
| Amazon S3 | Provides managed object storage for static assets. | Private origin access requires a correctly configured delivery layer. |
| Amazon CloudFront | Supports edge caching and content delivery. | Requires distribution, origin, cache, and domain configuration. |
| Origin Access Control | Keeps the S3 origin private while allowing authorized CloudFront access. | Incorrect bucket policies or origin configuration can prevent content delivery. |
| Bash deployment script | Reduces repetitive deployment operations. | Requires validation, error handling, and appropriate AWS CLI permissions. |
| CloudFront invalidation | Helps updated content become available without waiting for normal cache expiration. | Invalidating every path can be less efficient than invalidating only changed files. |
| Browser localStorage | Enables persistence without a backend database. | Data is browser-specific and is not automatically synchronized across devices. |

---

## 📈 Future Improvements

The project provides a foundation for exploring additional cloud engineering practices.

Potential improvements include:

### 1. GitHub Actions CI/CD

Configure a GitHub Actions workflow to validate changes and deploy approved updates automatically.

This would extend the existing scripted deployment process with a defined trigger and automated execution.

### 2. Terraform Infrastructure as Code

Define the S3 bucket, CloudFront distribution, OAC, and supporting configuration using Terraform.

This would make infrastructure provisioning more declarative, reviewable, and repeatable.

### 3. Custom Domain Configuration

Associate a custom domain with the CloudFront distribution and configure the appropriate DNS and HTTPS certificate settings.

### 4. Automated Validation and Error Handling

Extend the deployment script with:

- Configuration validation.
- Command exit-status checks.
- Error handling and logging.
- Safer deployment failure handling.

### 5. Cloud Monitoring

Explore CloudFront access logs, CloudWatch metrics, and relevant alarms to understand application delivery and infrastructure behavior.

### 6. Cost Optimization

Review S3 storage, CloudFront usage, data transfer, and invalidation patterns to identify opportunities for reducing unnecessary costs.

### 7. Backend Integration

Introduce authenticated server-side storage if cross-device synchronization, centralized financial records, or additional security requirements become necessary.

These improvements represent possible extensions rather than functionality already implemented.

---

## 🎓 Key Learnings

This project provided practical exposure to the relationship between frontend development and AWS cloud infrastructure.

Key learning areas include:

- Hosting static websites using Amazon S3 and CloudFront.
- Understanding private S3 origins and Origin Access Control.
- Understanding how requests flow through a CDN to an origin.
- Exploring edge caching and cache invalidation.
- Automating deployment operations using Bash and AWS CLI.
- Applying Infrastructure as Code principles to cloud configuration.
- Understanding the trade-offs between static hosting and backend-based architectures.
- Considering security, maintainability, and operational requirements when designing cloud-hosted applications.

---

## 📋 Project Summary

Wallet Watcher demonstrates how a lightweight personal finance application can be hosted on AWS using a private S3 bucket and Amazon CloudFront.

The project combines a responsive frontend with secure origin access, content delivery, and scripted deployment operations.

It provides a practical foundation for exploring AWS architecture, cloud security, deployment automation, and Infrastructure as Code principles.

---

<p align="center">
  <b>Built to explore secure, automated, and maintainable cloud hosting with AWS.</b>
</p>
