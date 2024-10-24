# Continuous Deployment of Static Website to Amazon S3 and CloudFront

This repository hosts the automation workflow for deploying a **static website** to **Amazon S3** and **Amazon CloudFront** using **GitHub Actions**. The workflow automates the deployment process to the **User Acceptance Testing (UAT)** environment whenever changes are made to the repository.

### READ the following
1. DOCUMENTATION-Hosting a static website using S3 and Cloudfront.pdf
2. 1.README.md

## Overview

We use GitHub Actions to automate the following tasks:
- Sync the static website files with an **Amazon S3** bucket.
- Invalidate the **CloudFront** distribution cache to ensure that users get the latest version of the site.
- Automatically trigger deployments when:
  - Code is pushed to the `main` or `uat` branches.
  - A pull request is merged into the `uat` branch.
  - The workflow is manually dispatched via GitHub.

## Workflow Details

The GitHub Actions workflow is located in the `.github/workflows/` directory. It triggers on:
- **Push events** to the `main` and `uat` branches.
- **Pull requests** that are opened, closed, or reopened, specifically targeting the `uat` branch.
- **Manual workflow dispatch** via the GitHub Actions interface.

### Workflow Trigger Conditions

The deployment to the **UAT environment** happens automatically when a **pull request is merged** into the `uat` branch, ensuring that only approved changes are deployed for user acceptance testing.

### Job Steps

The workflow includes the following steps:
1. **Checkout Code**: Fetch the latest version of the repository.
2. **Configure AWS Credentials**: Uses stored AWS secrets (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) to authenticate with AWS.
3. **Sync with S3**: Sync the repository files with the specified **S3 bucket** that hosts the static website. Any files that are deleted locally are also deleted in the S3 bucket.
4. **CloudFront Invalidation**: Invalidate the **CloudFront distribution cache**, ensuring that users see the most up-to-date version of the website.

### Key Variables and Secrets

To run this workflow, you need to configure the following **secrets** in your GitHub repository:

- `AWS_ACCESS_KEY_ID`: Your AWS Access Key ID with permissions to access S3 and CloudFront.
- `AWS_SECRET_ACCESS_KEY`: Your AWS Secret Access Key.
- `AWS_REGION`: The region where your S3 bucket and CloudFront distribution are hosted (e.g., `us-east-1`).

### AWS Resources Used
- **S3 Bucket**: Stores the static website files and serves the content.
- **CloudFront**: Provides a Content Delivery Network (CDN) to cache and distribute the website globally for faster access.

## How to Set Up

1. **AWS S3 Bucket**: Create an S3 bucket with static website hosting enabled. Configure your S3 bucket permissions to allow public access to your files (or use CloudFront for secure access).
   
2. **CloudFront Distribution**: Set up a CloudFront distribution to cache the content from the S3 bucket. Make sure to get the CloudFront **Distribution ID** as it will be used in the workflow for invalidations.

3. **GitHub Repository**: Create a GitHub repository for your static website files. Store your website files in this repository, and configure the GitHub Actions workflow to deploy them.

4. **Secrets Configuration**: Add your AWS credentials as secrets in your GitHub repository under **Settings > Secrets and variables > Actions**.

## Notes
1. Ensure that your S3 bucket permissions and CloudFront distribution settings are correctly configured to serve the static website.
2. Keep your AWS credentials secure by storing them in GitHub Secrets.

## Conclusion
This workflow automates the process of deploying a static website to Amazon S3 and CloudFront using GitHub Actions. It ensures that changes are tested and deployed in a controlled manner, making it easier to maintain and update the website with minimal manual effort.
