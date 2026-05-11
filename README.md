# Morocco Vision 2030

## Project Overview

Morocco Vision 2030 is a modern responsive static website presenting Morocco’s economic expansion through tourism, technology and sport.

The goal of this project was to deploy a secure, low-cost and globally available static website using AWS services.

## Objective

The objective was to build and deploy a static website on AWS while applying cloud best practices such as private storage, CDN delivery, HTTPS access, WAF protection and basic monitoring.

## Architecture

User traffic is served through Amazon CloudFront.

The static website files are stored in a private Amazon S3 bucket. Direct public access to the S3 bucket is blocked. CloudFront is the only service allowed to access the bucket through Origin Access Control.

Architecture flow:

User  
→ Amazon CloudFront + AWS WAF  
→ Origin Access Control  
→ Private Amazon S3 bucket  
→ Static website files

## AWS Services Used

- Amazon S3 for storing static website files
- Amazon CloudFront for CDN, HTTPS and global content delivery
- CloudFront Origin Access Control to restrict access to the private S3 bucket
- AWS WAF for web security protection
- Amazon CloudWatch for monitoring
- CloudFront viewer reports for traffic and visitor location analysis
- Route 53 studied for DNS management
- AWS Certificate Manager studied for custom SSL certificates

## Skills Demonstrated

- Deployment of a static website on AWS
- Configuration of a private Amazon S3 bucket
- Creation of a CloudFront distribution
- Configuration of CloudFront Origin Access Control
- S3 bucket policy allowing access only from CloudFront
- CloudFront default root object configuration
- AWS WAF Web ACL configuration
- AWS Managed Rules configuration
- CloudWatch metrics monitoring
- Basic cost optimization
- Understanding of Route 53 and ACM for custom domain setup

## Security

The S3 bucket is fully private and public access is blocked.

Access to the bucket is restricted to a single CloudFront distribution using Origin Access Control and an S3 bucket policy with the `AWS:SourceArn` condition.

This prevents users from bypassing CloudFront, AWS WAF and monitoring rules.

AWS WAF is associated with the CloudFront distribution through a Web ACL.

The Web ACL uses AWS Managed Rules, including:

- Amazon IP Reputation List
- Common Rule Set
- Known Bad Inputs Rule Set

The rules were first deployed in Count mode to observe traffic safely before switching to Block mode.

## HTTPS and Domain

For this demo project, the website is served through the default CloudFront domain with HTTPS enabled by default.

A custom domain was not purchased for this version of the project.

Route 53 and AWS Certificate Manager were reviewed as part of the architecture. A custom ACM certificate would only be required when attaching a custom domain name through Route 53.

## Monitoring and Observability

Monitoring was implemented using Amazon CloudFront metrics and Amazon CloudWatch.

The distribution tracks key indicators such as:

- Request count
- Bytes downloaded
- Cache hit ratio
- HTTP 4xx error rate
- HTTP 5xx error rate
- Total error rate

CloudFront viewer reports were used to analyze visitor locations, including the countries generating the most traffic, devices, browsers and operating systems.

For deeper observability, CloudFront standard logs can be delivered to Amazon S3 and analyzed to identify the most used edge locations through the `x-edge-location` field.

## Cost Optimization

This project was designed to remain low-cost by using the CloudFront Free plan.

The current version uses the default CloudFront domain, which avoids Route 53 domain registration costs.

The website contains only a few static files, so the storage requirement is minimal.

Main cost factors to monitor:

- CloudFront requests
- CloudFront data transfer
- AWS WAF usage
- Amazon S3 storage
- Route 53 costs if a custom domain is added later

## Troubleshooting

During the deployment, the website initially returned an Access Denied error through CloudFront.

The issue was caused by the missing CloudFront default root object. After setting the default root object to `index.html`, the website loaded correctly from the CloudFront URL.

This helped clarify the difference between accessing `/` and accessing `/index.html` through a CloudFront distribution.

## Possible Improvements

Future improvements could include:

- Purchasing a custom domain and managing DNS with Route 53
- Creating an ACM certificate in `us-east-1` for the CloudFront distribution
- Adding CloudFront standard logs to Amazon S3
- Querying logs with Amazon Athena
- Adding a CI/CD pipeline with GitHub Actions
- Optimizing images and serving them locally from S3
- Adding CloudFront response headers for additional browser security
