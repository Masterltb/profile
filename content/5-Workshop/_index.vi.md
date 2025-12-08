---
title: "Workshop"
date: "2025-09-30"
weight: 5
chapter: true
pre: " <b> 5. </b> "
---
# Smoking Cessation Platform - AWS Infrastructure Workshop

Hướng dẫn triển khai, xác minh và tối ưu hóa hạ tầng AWS cho nền tảng Smoking Cessation Platform.


---

## Workshop Overview

Workshop này bao gồm 10 modules chi tiết về cách thiết lập, xác minh và quản lý hạ tầng AWS hoàn chỉnh cho ứng dụng Smoking Cessation Platform.


---

## Prerequisites

- AWS Account (free tier đủ để học)
- AWS Console access
- Máy tính/laptop với browser
- 20-30 giờ để hoàn thành workshop (có thể chia nhiều buổi)

---

## Workshop Modules

### Module 1: Giới Thiệu Hạ Tầng
**Duration**: 1-2 giờ

Học về kiến trúc AWS toàn bộ hệ thống (Hybrid - EC2 + Lambda):
- 7 thành phần chính (Frontend, API Gateway, EC2 servers, Lambda, Databases, Security, Monitoring)
- User journeys & data flow
- Technology stack

**Mục tiêu**: Hiểu toàn bộ kiến trúc trước khi bắt đầu

**Thêm hình ảnh**: Architecture diagram, user flow diagrams

[Tới Module 1](./5.1-Introduction/_index.vi.md)

---

### Module 2: Chuẩn Bị & Prerequisites
**Duration**: 2-3 giờ

Chuẩn bị tài khoản AWS & tools:
- Tạo AWS Account
- Tạo IAM User
- Configure AWS CLI
- Chuẩn bị environment variables

**Mục tiêu**: Sẵn sàng truy cập AWS services

**Thêm hình ảnh**: Account setup screenshots, IAM user creation steps

[Tới Module 2](./5.2-Perequisites/_index.vi.md)

---

### Module 3: Verify Cognito Authentication
**Duration**: 2-3 giờ

Xác minh & cấu hình AWS Cognito:
- Review User Pool hiện tại
- Verify App Client settings
- Setup User Groups (users, coaches, admins)
- Test authentication flow
- Frontend integration

**Tài nguyên hiện có**:
- User Pool ID: us-east-1_dskUsnKt3
- Client ID: 4175kqc33olfjinhkll4jme379

**Mục tiêu**: Cognito authentication fully operational

**Thêm hình ảnh**: Cognito console screenshots, login flow diagram

[Tới Module 3](./5.3-Setup-cognito/_index.vi.md)

---

### Module 4: Verify Lambda Functions
**Duration**: 2-3 giờ

Xác minh & cấu hình Lambda functions:
- Review 5 Lambda functions
- Verify IAM roles & permissions
- Check environment variables
- Review CloudWatch logs
- Test functions
- Optimize memory & timeout

**Tài nguyên hiện có**:
- 1 function in us-east-1 (Cognito trigger)
- 4 functions in ap-southeast-1 (Admin, Chat, Payment, Image upload)

**Mục tiêu**: All Lambda functions verified & optimized

**Thêm hình ảnh**: Lambda dashboard, CloudWatch logs, test results

[Tới Module 4](./5.4-setup-lambda/_index.vi.md)

---

### Module 5: Verify API Gateway
**Duration**: 2 giờ

Xác minh & cấu hình API Gateways:
- Review 2 REST APIs
- Verify resources & methods
- Test API endpoints
- Setup CORS
- Configure rate limiting
- CloudWatch logging

**Tài nguyên hiện có**:
- LeafLungs-UserInfo-API (v7agf76rrh)
- leaflungs-chat-api (vuds39de1b)

**Mục tiêu**: All APIs fully functional with proper security

**Thêm hình ảnh**: API Gateway resources tree, test results, error responses

[Tới Module 5](./5.5-Setup-api-gateway/_index.vi.md)

---

### Module 6: Verify EC2 Servers & Databases
**Duration**: 3-4 giờ

Verify 4 EC2 instances & databases:
- Verify 4 EC2 instances (all t4g.small)
- Check PostgreSQL server (DB-PG) - 172.0.8.55
- Check MongoDB server (DB-Mongo) - 172.0.8.124
- Verify application servers (user-cessation, social-media)
- Test inter-server connectivity
- Configure monitoring & backups
- Troubleshooting & optimization

**Tài nguyên hiện có**:
- 2 Database servers (PostgreSQL + MongoDB)
- 2 Application servers (user-cessation + social-media)
- All running t4g.small in ap-southeast-1

**Mục tiêu**: All EC2 servers verified & operational with monitoring

**Thêm hình ảnh**: EC2 instances, instance status, monitoring dashboard

[Tới Module 6](./5.6-Setup-rds-database/_index.vi.md)

---

### Module 7: Verify S3 & CloudFront
**Duration**: 2 giờ

Xác minh & cấu hình S3 & CloudFront:
- Verify S3 bucket
- Verify CloudFront distribution
- Deploy frontend React app
- Test cache invalidation
- Setup custom domain (optional)
- Security headers

**Tài nguyên hiện có**:
- S3 Bucket: leaflungs-frontend-new
- CloudFront: E1NREZDKTJH6Y9

**Mục tiêu**: Frontend accessible via HTTPS with caching

**Thêm hình ảnh**: S3 console, CloudFront distribution, browser access

[Tới Module 7](./5.7-setup-s3-cloudfront/_index.vi.md)

---

### Module 8: Verify VPC & Security
**Duration**: 2-3 giờ

Xác minh & cấu hình VPC & security:
- Verify VPC architecture
- Review Security Groups
- Verify NLB for WebSocket
- IAM policies review
- Optional: VPC Flow Logs, GuardDuty

**Tài nguyên hiện có**:
- VPC: project-bundau-milo (vpc-046dc916dde2fb93f)
- NLB: leaflungs-userinfo-nlb
- 4 Security Groups configured

**Mục tiêu**: Network fully secured with proper isolation

**Thêm hình ảnh**: VPC diagram, security group rules, NLB configuration

[Tới Module 8](./5.8-setup-vpc-security/_index.vi.md)

---

### Module 9: Monitoring & Logging
**Duration**: 3-4 giờ

Setup comprehensive monitoring & logging:
- CloudWatch Logs aggregation
- Dashboards & metrics
- Alarms & notifications
- CloudTrail audit logging
- X-Ray distributed tracing
- Cost monitoring & budgets

**Mục tiêu**: Visibility into all systems with automated alerts

**Thêm hình ảnh**: CloudWatch dashboard, alarm configuration, X-Ray service map

[Tới Module 9](./5.9-Monitoring-logging/_index.vi.md)

---

### Module 10: Cleanup & Cost Optimization
**Duration**: 2-3 giờ

Cleanup, optimize costs & document:
- Identify unused resources
- Cost optimization strategies
- Backup & archive data
- Delete test resources
- Cost analysis
- Lessons learned documentation

**Mục tiêu**: Optimized costs & sustainable operations

**Thêm hình ảnh**: Cost Explorer reports, optimization recommendations

[Tới Module 10](./5.10-cleanup/_index.vi.md)

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

Làm từng module từ 1 đến 10 theo thứ tự:

1. Start with Module 1 (Giới thiệu)
2. Follow prerequisites in Module 2
3. Work through Modules 3-10 in order
4. Each module builds on previous modules

**Duration**: 20-30 giờ (có thể chia nhiều buổi)

### Option 2: Focused Path

Nếu chỉ quan tâm Module nhất định:

- **Security focused**: Modules 2 + 8
- **Backend focused**: Modules 2 + 3 + 4 + 5 + 6
- **Frontend focused**: Modules 2 + 7
- **Operations focused**: Modules 2 + 9 + 10

---

## Resources Inventory

Xem file [AWS_RESOURCES_INVENTORY.md](./AWS_RESOURCES_INVENTORY.md) để danh sách chi tiết toàn bộ resources hiện có.

---

## Notes for Adding Screenshots

Mỗi module có placeholder cho hình ảnh:

```
**Thêm hình ảnh: [Description]**
[Screenshot: ...]
```

Hãy thay thế bằng actual screenshots từ AWS Console:
1. Mở AWS Console
2. Navigate đến service
3. Chụp ảnh cột trái hoặc form dialog
4. Lưu với tên có ý nghĩa
5. Add vào file markdown

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

Xem detailed troubleshooting sections ở mỗi module.

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

Sau khi hoàn thành workshop, bạn sẽ:

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

Để phản hồi hoặc cải thiện workshop:
1. Note specific module & issue
2. Suggest improvement or correction
3. Share any additional resources found helpful

---

**Happy Learning! Chúc bạn thành công với AWS infrastructure!**

---

Generated: 2025-12-03
Status: Ready for screenshots & final review
