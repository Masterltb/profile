---
title: "Module 1: Infrastructure Introduction"
date: "2025-09-30"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---


# Module 1: Smoking Cessation Platform Infrastructure Introduction

## Module Objectives

- Understand the overall AWS system architecture
- Grasp the main components of the platform
- Understand the data flow between services
- Prepare for the following modules

---

## Overall System Architecture (Hybrid - EC2 + Lambda)

### AWS Architecture Diagram

**Architecture Type**: Hybrid (EC2 + Serverless Lambda)
**Deployment Pattern**: EC2 for stateful services, Lambda for event-driven tasks

![AWS Hybrid Architecture Diagram](/images/5-Workshop/architecture.png)

---

## Key Components

### 1. Frontend Layer (React + Vite)

Hosting: S3 + CloudFront

Features:
- Responsive UI for Web & Mobile
- Real-time Chat with WebSocket
- User Authentication (Cognito)
- Progress Tracking Dashboard
- Coach Management Interface

### 2. API Gateway Layer

REST API: /api/v1/* endpoints
WebSocket: /ws endpoints for real-time chat

Responsibilities:
- Request routing
- Authentication validation
- Rate limiting
- CORS handling

### 3. Backend Layer (Hybrid: EC2 + Lambda)

EC2 Application Servers (Always-on):
- user-cessation service (Port 8000)
  - User profiles, progress tracking
  - Coaching session management
  - Statistics & analytics

- social-media service (Port 8000)
  - Social features
  - Notifications
  - Community management

Lambda Functions (Event-driven):
- File Upload Lambda
  - Handle image/file uploads to S3

- Payment Processing Lambda
  - Process payments & subscriptions

- Specific Trigger Functions
  - Webhooks
  - Scheduled tasks

### 4. WebSocket & Real-time Layer

NLB (Network Load Balancer):
- Handles persistent WebSocket connections
- Port 443 (HTTPS)
- Distributes real-time chat traffic
- Integration with EC2 backend servers

### 5. Database Layer (EC2-hosted)

PostgreSQL Server (DB-PG):
- User profiles & authentication
- Progress tracking data
- Coaching session records
- Relational data

MongoDB Server (DB-Mongo):
- Chat message history
- Social media content
- Message metadata
- Flexible schema data

### 6. Security Layer

- AWS Cognito: User authentication & authorization
- IAM Roles: Service-to-service permissions
- VPC: Network isolation (private subnets for databases)
- Security Groups: Firewall rules for EC2 & databases
- SSL/TLS: Data encryption in transit & at rest
- NLB Security Group: Restricts access to WebSocket port

### 7. Monitoring & Logging

- CloudWatch: Logs & Metrics for all services
- CloudTrail: API audit trail
- EC2 Instance Monitoring: CPU, memory, disk usage
- Database Monitoring: Query performance, connections
- Alarms: Performance & health alerts

---

## User Journeys & Data Flow

### Journey 1: User Registration & Login

```
1. User goes to registration page
   ↓
2. Frontend sends credentials → API Gateway → Lambda (Auth Service)
   ↓
3. Lambda validates & creates user in PostgreSQL (EC2)
   ↓
4. AWS Cognito creates user account
   ↓
5. Lambda returns access token & refresh token
   ↓
6. Frontend stores token → has access to API
```

### Journey 2: Real-time Chat

```
1. User A sends message
   ↓
2. Message sent → API Gateway WebSocket endpoint
   ↓
3. Lambda (Chat Service) processes message
   ↓
4. Saves message to MongoDB (EC2)
   ↓
5. WebSocket broadcasts message to User B (connected)
   ↓
6. User B receives real-time message
```

### Journey 3: Progress Tracking

```
1. User updates progress data (e.g., smoke-free days)
   ↓
2. Frontend sends → API Gateway → Lambda (User Service)
   ↓
3. Lambda validates & updates PostgreSQL (EC2)
   ↓
4. Coach receives notification (via WebSocket)
   ↓
5. Dashboard updates real-time
```

---

## Technologies & Services

| Component | Service | Details |
|-----------|---------|---------|
| Frontend Hosting | S3 + CloudFront | Static website hosting + CDN |
| Authentication | Cognito | User authentication & SSO |
| API Management | API Gateway | REST API routing |
| Compute (Always-on) | EC2 (t4g.small) | Main application servers |
| Compute (Event-driven) | Lambda | Specific functions (upload, payment) |
| Real-time | NLB + WebSocket | WebSocket connections & messaging |
| Database (SQL) | EC2 + PostgreSQL | User data, relational data |
| Database (NoSQL) | EC2 + MongoDB | Chat history, social data |
| Storage | S3 | File uploads & assets |
| Security | VPC, Security Groups, IAM | Network isolation & access control |
| Monitoring | CloudWatch | Logs, metrics, alarms |
| CDN | CloudFront | Content delivery network |

---

## Next Modules

1. Module 2: Prerequisites - Prepare account & tools
2. Module 3: Setup Cognito - User authentication
3. Module 4: Setup Lambda - Backend functions
4. Module 5: Setup API Gateway - API endpoints
5. Module 6: Verify EC2 Servers & Databases - PostgreSQL + MongoDB on EC2
6. Module 7: Setup S3 + CloudFront - Frontend hosting
7. Module 8: Setup VPC & Security - Network security
8. Module 9: Monitoring & Logging - System observability
9. Module 10: Cleanup - Delete resources & cost optimization

---

## Checklist

- [ ] Understand overall AWS architecture
- [ ] Grasp 6 main components
- [ ] Understand 3 key user journeys
- [ ] Ready for Module 2

---

## Notes

- System uses Serverless Architecture - no need to manage servers
- All services auto-scale according to demand
- Pay-as-you-go pricing model
- Highly available & disaster recovery ready