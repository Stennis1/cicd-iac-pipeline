
# 🚀 AWS CI/CD Pipeline for Static Website Hosting

This project demonstrates a **fully automated CI/CD pipeline** using **AWS CloudFormation**, **AWS CodePipeline (CloudOne)**, and **GitHub** to deploy a **static website** hosted on **Amazon S3**.

Every time you push code to your GitHub repository, the pipeline automatically:
1. Fetches the latest code,
2. Builds it using CodeBuild,
3. Deploys it to an S3 bucket configured for static website hosting.

---

## 🧱 Architecture Overview

```

GitHub Repo
↓
AWS CodePipeline (CloudOne)
├── Source (GitHub)
├── Build (CodeBuild)
└── Deploy (S3 Website)

```

**AWS Resources Created:**
- S3 Bucket (for hosting website)
- S3 Bucket Policy (for public read access)
- CodeBuild Project
- CodePipeline
- IAM Roles for Pipeline & Build
- Optional: CloudFront (can be added later)

---

## 📁 Project Structure

```

ci-cd-static-site/
│
├── index.html                  # Home page
├── error.html                  # Error page
├── buildspec.yml               # CodeBuild instructions
└── cloudformation-template.yml # Infrastructure as Code

````

---

## ⚙️ Prerequisites

Before deployment, make sure you have:

1. **An AWS Account** with permissions for:
   - CloudFormation
   - CodePipeline
   - CodeBuild
   - S3
2. **A GitHub Repository** with your static website files.
3. **A GitHub Personal Access Token (PAT)**  
   - Go to: [GitHub → Settings → Developer Settings → Tokens → Generate New Token (Classic)](https://github.com/settings/tokens)  
   - Enable scopes:  
     ```
     repo
     admin:repo_hook
     ```
   - Copy and save it (you’ll use it in CloudFormation).

---

## 🪜 Step-by-Step Setup

### 1️⃣ Clone This Repository
```bash
git clone https://github.com/<your-username>/ci-cd-static-site.git
cd ci-cd-static-site
````

### 2️⃣ Edit the Files

* Replace text in `index.html` and `error.html` with your website content.
* Commit and push to GitHub.

### 3️⃣ Deploy via CloudFormation

1. Go to the AWS Management Console → **CloudFormation**
2. Click **Create Stack → With new resources (standard)**
3. Choose **Upload a template file** → select `cloudformation-template.yml`
4. Click **Next**
5. Fill in parameters:

   * **GitHubRepo:** `yourusername/yourrepo`
   * **GitHubBranch:** `main`
   * **GitHubToken:** your GitHub PAT
6. Click **Next → Next → Create Stack**

Wait a few minutes for stack creation to complete.

---

## 🔄 Pipeline Workflow

Once deployed, your pipeline will:

1. **Detect changes** in your GitHub repo.
2. **Trigger the build** stage via CodeBuild.
3. **Deploy updated files** to your S3 bucket.
4. **Host the latest version** automatically.

You can monitor the progress from **AWS CodePipeline**.

---

## 🌐 View Your Website

After deployment:

1. Go to **AWS CloudFormation → Stacks → Outputs**
2. Copy the **WebsiteURL**
3. Open it in your browser 🎉

---

## 🧩 Files Explained

### `buildspec.yml`

Defines what happens in the build stage.
For static sites, it simply passes files to the deploy stage.

```yaml
version: 0.2
phases:
  install:
    commands:
      - echo Installing dependencies...
  build:
    commands:
      - echo Building static website...
artifacts:
  files:
    - '**/*'
```

### `cloudformation-template.yml`

Defines the AWS infrastructure (S3, CodeBuild, CodePipeline, IAM roles).

---

## 🧠 Key Concepts

* **Infrastructure as Code (IaC):**
  CloudFormation provisions all AWS resources consistently.
* **Continuous Integration (CI):**
  GitHub commits trigger automatic builds.
* **Continuous Deployment (CD):**
  CodePipeline deploys updates to S3 automatically.
* **Scalability:**
  Add CloudFront, Route 53, or SSL later for production setups.

---

## 🧰 Optional Enhancements (Future Work)

* Add **CloudFront** for HTTPS + global caching
* Add **Route53** for a custom domain name
* Add **Slack notifications** for pipeline updates
* Add **Build Tests** or **Linting** in CodeBuild

## ✅ Summary

| Component                   | Description                             |
| --------------------------- | --------------------------------------- |
| **CloudFormation**          | Defines and deploys AWS infrastructure  |
| **CodePipeline (CloudOne)** | Automates CI/CD workflow                |
| **CodeBuild**               | Builds or prepares deployment artifacts |
| **S3 Bucket**               | Hosts the static website                |
| **GitHub Repo**             | Source of truth for code                |
