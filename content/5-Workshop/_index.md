---
title: "Workshop"
date: "2025-09-30"
weight: 5
chapter: true
pre: " <b> 5. </b> "
---

# Smoking Cessation Platform - AWS Infrastructure Workshop

A guide to deploying, verifying, and optimizing AWS infrastructure for the Smoking Cessation Platform.

---

## Workshop Overview

This workshop includes 10 detailed modules on how to set up, verify, and manage the complete AWS infrastructure for the Smoking Cessation Platform application.

---

## Prerequisites

- AWS Account (free tier is sufficient for learning)
- AWS Console access
- Computer/laptop with a browser
- 20-30 hours to complete the workshop (can be split into multiple sessions)

---

## Workshop Modules

### Module 1: Infrastructure Introduction
**Duration**: 1-2 hours

Learn about the entire AWS architecture of the system (Hybrid - EC2 + Lambda):
- 7 main components (Frontend, API Gateway, EC2 servers, Lambda, Databases, Security, Monitoring)
- User journeys & data flow
- Technology stack

**Objective**: Understand the entire architecture before starting

**Add images**: Architecture diagram, user flow diagrams

[Go to Module 1](./5.1-Introduction/)

---

### Module 2: Preparation & Prerequisites
**Duration**: 2-3 hours

Prepare AWS account & tools:
- Create AWS Account
- Create IAM User
- Configure AWS CLI
- Prepare environment variables

**Objective**: Ready to access AWS services

**Add images**: Account setup screenshots, IAM user creation steps

[Go to Module 2](./5.2-Perequisites/)

---

### Module 3: Verify Cognito Authentication
**Duration**: 2-3 hours

Verify & configure AWS Cognito:
- Review current User Pool
- Verify App Client settings
- Setup User Groups (users, coaches, admins)
- Test authentication flow
- Frontend integration

**Existing Resources**:
- User Pool ID: us-east-1_dskUsnKt3
- Client ID: 4175kqc33olfjinhkll4jme379

**Objective**: Cognito authentication fully operational

**Add images**: Cognito console screenshots, login flow diagram

[Go to Module 3](./5.3-Setup-cognito/)

---

### Module 4: Verify Lambda Functions
**Duration**: 2-3 hours

Verify & configure Lambda functions:
- Review 5 Lambda functions
- Verify IAM roles & permissions
- Check environment variables
- Review CloudWatch logs
- Test functions
- Optimize memory & timeout

**Existing Resources**:
- 1 function in us-east-1 (Cognito trigger)
- 4 functions in ap-southeast-1 (Admin, Chat, Payment, Image upload)

**Objective**: All Lambda functions verified & optimized

**Add images**: Lambda dashboard, CloudWatch logs, test results

[Go to Module 4](./5.4-setup-lambda/)

---

### Module 5: Verify API Gateway
**Duration**: 2 hours

Verify & configure API Gateways:
- Review 2 REST APIs
- Verify resources & methods
- Test API endpoints
- Setup CORS
- Configure rate limiting
- CloudWatch logging

**Existing Resources**:
- LeafLungs-UserInfo-API (v7agf76rrh)
- leaflungs-chat-api (vuds39de1b)

**Objective**: All APIs fully functional with proper security

**Add images**: API Gateway resources tree, test results, error responses

[Go to Module 5](./5.5-Setup-api-gateway/)

---

### Module 6: Verify EC2 Servers & Databases
**Duration**: 3-4 hours

Verify 4 EC2 instances & databases:
- Verify 4 EC2 instances (all t4g.small)
- Check PostgreSQL server (DB-PG) - 172.0.8.55
- Check MongoDB server (DB-Mongo) - 172.0.8.124
- Verify application servers (user-cessation, social-media)
- Test inter-server connectivity
- Configure monitoring & backups
- Troubleshooting & optimization

**Existing Resources**:
- 2 Database servers (PostgreSQL + MongoDB)
- 2 Application servers (user-cessation + social-media)
- All running t4g.small in ap-southeast-1

**Objective**: All EC2 servers verified & operational with monitoring

**Add images**: EC2 instances, instance status, monitoring dashboard

[Go to Module 6](./5.6-Setup-rds-database/)

---

### Module 7: Verify S3 & CloudFront
**Duration**: 2 hours

Verify & configure S3 & CloudFront:
- Verify S3 bucket
- Verify CloudFront distribution
- Deploy frontend React app
- Test cache invalidation
- Setup custom domain (optional)
- Security headers

**Existing Resources**:
- S3 Bucket: leaflungs-frontend-new
- CloudFront: E1NREZDKTJH6Y9

**Objective**: Frontend accessible via HTTPS with caching

**Add images**: S3 console, CloudFront distribution, browser access

[Go to Module 7](./5.7-setup-s3-cloudfront/)

---

### Module 8: Verify VPC & Security
**Duration**: 2-3 hours

Verify & configure VPC & security:
- Verify VPC architecture
- Review Security Groups
- Verify NLB for WebSocket
- IAM policies review
- Optional: VPC Flow Logs, GuardDuty

**Existing Resources**:
- VPC: project-bundau-milo (vpc-046dc916dde2fb93f)
- NLB: leaflungs-userinfo-nlb
- 4 Security Groups configured

**Objective**: Network fully secured with proper isolation

**Add images**: VPC diagram, security group rules, NLB configuration

[Go to Module 8](./5.8-setup-vpc-security/)

---

### Module 9: Monitoring & Logging
**Duration**: 3-4 hours

Setup comprehensive monitoring & logging:
- CloudWatch Logs aggregation
- Dashboards & metrics
- Alarms & notifications
- CloudTrail audit logging
- X-Ray distributed tracing
- Cost monitoring & budgets

**Objective**: Visibility into all systems with automated alerts

**Add images**: CloudWatch dashboard, alarm configuration, X-Ray service map

[Go to Module 9](./5.9-Monitoring-logging/)

---

### Module 10: Cleanup & Cost Optimization
**Duration**: 2-3 hours

Cleanup, optimize costs & document:
- Identify unused resources
- Cost optimization strategies
- Backup & archive data
- Delete test resources
- Cost analysis
- Lessons learned documentation

**Objective**: Optimized costs & sustainable operations

**Add images**: Cost Explorer reports, optimization recommendations

[Go to Module 10](./5.10-cleanup/)

---

## AWS Resources Created/Verified

### Compute
- EC2: 4 instances (verified) - 2 databases + 2 applications
- Lambda: 5 functions (verified)
- API Gateway: 2 REST APIs (verified)
- Network Load Balancer: 1 for WebSocket (verified)

### Database
- PostgreSQL: Running on EC2 instance DB-PG (verified)
- MongoDB: Running on EC2 instance DB-Mongo (verified)

### Storage
- S3: 3 buckets for frontend & images (verified)
- CloudFront: 1 distribution for CDN (verified)

### Authentication & Security
- Cognito: User Pool (verified)
- VPC: Network isolation (verified)
- Security Groups: 4 groups (verified)
- IAM Roles: 4 roles with proper permissions (verified)

### Monitoring
- CloudWatch: Logs, metrics, alarms (to be configured)
- CloudTrail: API audit logging (to be enabled)
- X-Ray: Distributed tracing (to be enabled)

### Total Estimated Monthly Cost: ~$300-350/month
Current breakdown:
- EC2 (4 × t4g.small): ~$70/month
- API Gateway: ~$50/month
- CloudFront: ~$80-100/month
- Cognito: ~$0 (free tier covers)
- Data transfer: ~$30-50/month
- Monitoring & misc: ~$20/month

Can be optimized to ~$150/month with recommendations in Module 10

---

## Architecture Diagram (Hybrid - EC2 + Lambda)

```
Users/Browsers
      ↓
CloudFront (CDN) + S3 (Frontend)
      ↓
Cognito (Auth) + NLB (WebSocket)
      ↓
API Gateway (REST API)
      ↙                    ↘
EC2 Backend          Lambda Functions
Servers              - File Upload
- user-cessation     - Payments
  (Port 8000)        - Webhooks
- social-media
  (Port 8000)

      ↓
EC2 Database Servers
- PostgreSQL (DB-PG): User data
- MongoDB (DB-Mongo): Chat history
      ↓
AWS Security & Infrastructure
- VPC & Security Groups
- IAM Roles & Policies
- Monitoring (CloudWatch)
```

---

## How to Use This Workshop

### Option 1: Sequential (Recommended)

Follow each module from 1 to 10 in order:

1. Start with Module 1 (Introduction)
2. Follow prerequisites in Module 2
3. Work through Modules 3-10 in order
4. Each module builds on previous modules

**Duration**: 20-30 hours (can be split into multiple sessions)

### Option 2: Focused Path

If you are only interested in specific Modules:

- **Security focused**: Modules 2 + 8
- **Backend focused**: Modules 2 + 3 + 4 + 5 + 6
- **Frontend focused**: Modules 2 + 7
- **Operations focused**: Modules 2 + 9 + 10

---

## Resources Inventory

See the [AWS_RESOURCES_INVENTORY.md](./AWS_RESOURCES_INVENTORY.md) file for a detailed list of all existing resources.

---

## Notes for Adding Screenshots

Each module has a placeholder for images:

```
**Add images: [Description]**
[Screenshot: ...]
```

Please replace with actual screenshots from the AWS Console:
1. Open AWS Console
2. Navigate to the service
3. Capture screenshot of the left column or dialog form
4. Save with a meaningful name
5. Add to the markdown file

---

## Best Practices Emphasized

### Security
- VPC isolation
- Least-privilege IAM
- Secrets Manager for credentials
- Encryption in transit & at rest

### Reliability
- Multi-AZ deployment (optional)
- Automated backups
- CloudFront caching
- Health checks

### Performance
- CloudFront caching
- Lambda memory optimization
- EC2 database instance tuning (optional)
- Connection pooling

### Cost Optimization
- Right-sizing instances
- Caching strategy
- Reserved instances
- Archive old data

### Operational Excellence
- CloudWatch monitoring
- CloudTrail audit logging
- X-Ray tracing
- Incident runbooks

---

## Troubleshooting

### Common Issues

**AWS CLI credentials expired**
- Solution: Sync system time with `date` command
- Update credentials: `aws configure`

**CloudFront shows old content**
- Solution: Create cache invalidation in Module 7
- Or enable automatic invalidation on deploy

**Lambda timeout errors**
- Solution: Increase timeout in Module 4
- Check EC2 database connection in Module 6

**API Gateway CORS errors**
- Solution: Enable CORS in Module 5
- Verify callback URLs in Module 3

See detailed troubleshooting sections in each module.

---

## Support & Resources

### AWS Documentation
- [Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)
- [EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
- [Cognito Documentation](https://docs.aws.amazon.com/cognito/)

### AWS Training
- [AWS Skill Builder](https://skillbuilder.aws.com/)
- [AWS Workshops](https://workshops.aws/)

### Community
- [AWS Forum](https://forums.aws.amazon.com/)
- [Stack Overflow AWS tag](https://stackoverflow.com/questions/tagged/amazon-aws)

---

## Estimated Timeline

| Phase | Duration | Modules |
|-------|----------|---------|
| Learning | 4-5 hours | 1-2 |
| Setup | 8-10 hours | 3-6 |
| Testing & Verification | 4-5 hours | 7-8 |
| Operations | 3-4 hours | 9-10 |
| **Total** | **20-30 hours** | **1-10** |

Can spread over multiple weeks/months

---

## Success Criteria

Upon completion of the workshop, you will:

✓ Understand complete AWS architecture
✓ Verify all services properly configured
✓ Deploy frontend to CloudFront
✓ Create production database
✓ Setup comprehensive monitoring
✓ Identify cost optimization opportunities
✓ Have operational runbooks
✓ Be able to troubleshoot issues
✓ Scale infrastructure as needed

---

## Next Steps After Workshop

1. **Implement CI/CD**: Add GitHub Actions for automated deployments
2. **Add Testing**: Unit tests, integration tests, E2E tests
3. **Team Training**: Train team on operational procedures
4. **Disaster Recovery**: Plan & test DR scenarios
5. **Scheduled Reviews**: Quarterly cost & architecture reviews
6. **Monitoring**: Set up on-call rotation & escalation
7. **Security**: Regular security audits & penetration testing
8. **Documentation**: Keep runbooks & architecture docs updated

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| 2025-12-03 | 1.0 | Initial workshop created |

---

## Feedback

To provide feedback or improve the workshop:
1. Note specific module & issue
2. Suggest improvement or correction
3. Share any additional resources found helpful

---

**Happy Learning! Good luck with your AWS infrastructure!**

---

Generated: 2025-12-03
Status: Ready for screenshots & final review