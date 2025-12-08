---
title: "Proposal"
date: "2025-01-01"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Smoking Cessation Support System Proposal

## 1. Executive Summary

This proposal outlines the design and implementation of a cloud-based **Smoking Cessation Support Platform** aimed at helping users quit smoking through data tracking, behavioral insights, AI coaching, and community engagement.

The system integrates a modern, scalable backend infrastructure deployed on **AWS Cloud**, ensuring high availability, security, and a seamless user experience. The goal is to provide an intelligent, personalized journey for users to monitor, plan, and achieve their smoking cessation goals—while giving administrators and health coaches the tools to support and guide them.

---

## 2. System Objectives

- **Personalized Plans:** Help users build and follow personalized quit-smoking plans.
- **Real-time Tracking:** Track smoking behavior and health progress in real-time.
- **AI Coaching:** Offer AI-driven coaching, reminders, and motivational feedback.
- **Community Engagement:** Enable social interaction and encouragement among members.
- **Reliability:** Provide a secure, cloud-native infrastructure for scalability and reliability.

---

## 3. Key Features

### User-Oriented Features

- **Homepage & Knowledge Hub:** Public homepage that introduces the platform, shares success stories, FAQs, and a blog for experience sharing.
- **Membership & Payments:** Users register, select tiers (free/premium), and pay via secure third-party providers; subscription status drives access.
- **Smoking Status Tracking:** Log daily cigarette consumption, frequency, brand/cost, and emotional triggers.
- **Quit Plan & Journey Branching:** Dynamic planning (reasons, stages, dates). If relapse occurs, the journey branches with tailored recovery steps.
- **Progress Tracking:** Visualize smoke-free days, money saved, health improvements, and streaks.
- **Motivational Notifications:** Daily/weekly reminders via in-app, email, or SMS.
- **Achievements (Gamification):** Earn and share milestones (e.g., “1 Day Smoke-Free”, “₫100K Saved”).
- **AI Coaching Agent:** Personalized guidance powered by AI with safety disclaimers and human handoff.
- **Coach Interaction:** Chat or schedule video sessions with platform health coaches.
- **Profile Management:** Edit goals, notifications, and privacy settings.

### Admin & Operator Features

- **Dashboard & Analytics:** Monitor user metrics, engagement, and health impact analytics.
- **Coach Portal:** Assign caseloads, triage at-risk users, and conduct counseling.
- **Pricing Management:** Define fee packages, benefits, and promotions.
- **Content Management:** Manage blogs, motivational messages, and educational content.
- **Feedback Management:** Track user satisfaction metrics and content quality.

---

## 4. System Architecture (AWS Cloud)

The system leverages AWS-managed services for scalability and security.

![Platform Architecture Diagram](/images/2-Proposal/arch.drawio.png)

### Frontend Layer
- **Amazon S3:** Hosts the static website and the web app frontend (React).
- **Amazon CloudFront:** Distributes web content globally and handles SSL/TLS encryption.

### Authentication & Authorization
- **Amazon Cognito:** Manages user sign-up, sign-in, and identity federation, ensuring secure access.

### Application Layer
- **AWS Lambda:** Serverless compute for payment webhooks and scheduled jobs.
- **Coach Chat/Video:** Enables real-time communication.
- **Payments:** Integrated via secure webhooks; API keys stored in **AWS Secrets Manager**.
- **Network Load Balancer (NLB):** Distributes requests to backend services.
- **EC2 Instances (Private Subnet):** Host core microservices (User, Cessation, Social, AI Orchestration).

### Data Layer
- **PostgreSQL (on EC2):** Stores structured user, plan, and transaction data.
- **MongoDB (on EC2):** Stores unstructured social features and content.
- **Amazon S3:** Stores media assets and encrypted database backups.

### DevOps Pipeline
- **CodePipeline:** Automates builds and deployments.
- **Amazon ECR:** Stores container images.
- **VPC Endpoint:** Ensures secure internal communication.

---

## 5. Security and Compliance

- **Encryption:** Data encrypted in transit (TLS) and at rest (AES-256).
- **Access Control:** Fine-grained IAM policies for system roles.
- **Network Security:** Backend isolated in Private Subnets; internal traffic via VPC Endpoints.
- **Payment Security:** PCI-DSS compliant flow (card data handled by provider).
- **Privacy:** Strict consent management and data retention policies.

---

## 6. Scalability and Performance

- **Auto Scaling:** Automatically adjusts EC2 and Lambda capacity based on demand.
- **Caching:** CloudFront caches static content for global speed.
- **Microservices:** Decoupled design allows independent scaling of features.
- **Event-Driven:** EventBridge decouples notifications and background workflows.

---

## 7. Pricing Estimate

### What Will Be FREE ($0.00)
- **Lambda Functions (x2):** Within 1M requests/month Free Tier.
- **S3 Buckets (x2):** Within 5GB Standard Storage Free Tier.
- **Amazon ECR:** Within 500MB/month Free Tier.
- **CloudFront:** Within 1TB data transfer Free Tier.
- **EC2 Instance (x1):** 750 hours/month (t4g.micro).
- **Cognito:** First 50,000 MAU are free.

### Estimated Monthly Costs (Paid)
- **Compute (Microservices):** 3x EC2 instances (t4g.small) ≈ **$45.00**
- **Networking (Load Balancing):** 1x Network Load Balancer ≈ **$18.00**
- **Security (Private Access):** 1x VPC Endpoint ≈ **$5.00**

**Total Estimated Monthly Cost: ~$68.00**

---

## 8. Expected Outcomes

- **Success Rates:** Increased cessation success through structured plans and tracking.
- **Engagement:** Higher user retention via gamification and community support.
- **Scalability:** A robust platform capable of supporting large user growth.
- **Security:** A compliant infrastructure protecting sensitive health data.
