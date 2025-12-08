---
title: "Module 10: Cleanup"
date: "2025-09-30"
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

# Module 10: Cleanup & Cost Optimization

## Module Objectives

- Identify unused/underutilized resources
- Delete test resources
- Optimize costs
- Archive important data
- Back up before deletion
- Document lessons learned

---

## Part 1: Resource Inventory & Assessment

### Current AWS Resources

List all resources from the AWS Console:

**Lambda Functions:**
- Go to the Lambda console
- List functions in each region (us-east-1, ap-southeast-1)
- Note: Function names, creation dates, memory allocation

**API Gateways:**
- Go to the API Gateway console
- List all REST APIs in ap-southeast-1
- Note: API names, deployment status

**EC2 Database Instances:**
- Go to the EC2 console
- List all instances in ap-southeast-1
- Filter for database instances (DB-PG, DB-Mongo)
- Note: Instance IDs, instance types, IPs, creation dates

**S3 Buckets:**
- Go to the S3 console
- List all buckets
- Note: Bucket names, creation dates, storage used

**CloudFront Distributions:**
- Go to the CloudFront console
- List all distributions
- Note: Domain names, distribution IDs

**VPC Resources:**
- Go to the VPC console
- List all VPCs in ap-southeast-1
- Note: VPC IDs, CIDR ranges

Create an inventory spreadsheet:
- Resource name & type
- Region
- Creation date
- Current usage
- Estimated monthly cost

---

## Part 2: Identify Unused Resources

### Check Resource Usage

#### Lambda Functions

To check Lambda invocations:

1. Go to the CloudWatch console
2. Click "Metrics" → "Lambda"
3. Select each function
4. Choose the metric: "Invocations"
5. Set the time range: Last 7 days
6. Check if Sum = 0 (unused function)

If there are no invocations in the last 7 days, it's an unused function (consider deletion).

#### API Gateways

To check the API request count:

1. Go to the CloudWatch console
2. Click "Metrics" → "API Gateway"
3. Select your API
4. Choose the metric: "Count"
5. Set the time range: Last 30 days
6. Check if Sum = 0 (unused API)

#### S3 Buckets

To check bucket sizes:

1. Go to the S3 console
2. Click on each bucket
3. Go to the "Storage" tab
4. See the "Storage used" value
5. Review the storage class (Standard, Infrequent Access, Glacier)
6. Consider moving old objects to cheaper storage classes

S3 storage class pricing:
- Standard: $0.023/GB/month
- Infrequent Access: $0.0125/GB/month
- Glacier: $0.004/GB/month

#### CloudFront

To check the cache hit ratio:

1. Go to the CloudWatch console
2. Click "Metrics" → "CloudFront"
3. Select your distribution
4. Choose the metric: "CacheHitRate"
5. Set the time range: Last 30 days
6. If the cache hit ratio is < 50%, it may need optimization.

#### EC2 Database Instances

To check CPU & memory usage:

1. Go to the CloudWatch console
2. Click "Metrics" → "EC2"
3. Select your database instance (DB-PG or DB-Mongo)
4. Choose the metric: "CPUUtilization"
5. Set the time range: Last 30 days
6. Check the Average and Maximum values

If the CPU is < 10% consistently, you may be able to downsize the instance type (saving money on EC2).

---

## Part 3: Cost Optimization Strategies

### 1. Right-Sizing EC2 Database Instances

Current: t4g.small EC2 instances (PostgreSQL + MongoDB)
Cost: ~$30-40/month per instance

Options:
- t4g.nano: ~$3-5/month (for test/dev)
- t4g.micro: ~$8-10/month (for light production)
- t4g.small: Keep as-is (recommended for production)

Check: If EC2 CPU is < 10% on average, downsize to save money.

### 2. Reserve Capacity (if usage is stable)

For production, consider AWS EC2 Reserved Instances:
- 1-year: ~30% discount
- 3-year: ~50% discount

To purchase reserved EC2 instances:

1. Go to the EC2 console
2. Click "Reserved Instances" (left menu)
3. Click "Purchase Reserved Instances"
4. Configure:
   - Instance type: t4g.small
   - Term length: 1-year or 3-year
5. Click "Purchase"

### 3. Optimize Data Transfer

Costs:
- S3 to CloudFront: Free
- S3 to the Internet: $0.09/GB
- S3 to Lambda (same region): Free

Recommendation: Keep CloudFront caching enabled.

### 4. Lambda Optimization

Current: 256 MB average
Cost: ~$0.0000002 per invocation

If the invocation rate is high:
- Use Lambda provisioned concurrency: Higher cost but predictable
- Monitor: Check if a memory increase helps reduce the duration (saves cost)

### 5. Database Optimization

Recommendations:
- Enable automated backups (already done)
- Set up read replicas if you're scaling reads (cost: ~50% of the main instance)
- Archive old logs (CloudWatch retention is 30 days by default)

### 6. CloudFront Optimization

If the cache hit ratio is < 50%:
1. Increase the TTL for static assets
2. Add more cache behaviors
3. Use cache policies with parameters

---

## Part 4: Backup & Archive

### Step 1: Back up the EC2 Databases

To back up PostgreSQL and MongoDB on EC2:

**PostgreSQL (DB-PG: 172.0.8.55):**

1. SSH into the EC2 instance
2. Create a backup: `pg_dump smokingcessation > backup_$(date +%Y%m%d).sql`
3. Compress it: `gzip backup_*.sql`

**MongoDB (DB-Mongo: 172.0.8.124):**

1. SSH into the EC2 instance
2. Create a backup: `mongodump --db smokingcessation --out backup_$(date +%Y%m%d)`
3. Compress it: `tar czf backup_*.tar backup_*`

### Step 2: Upload the Database Backups to S3

To export the database backups for archival:

1. SSH into the database EC2 instance
2. Upload to S3:
   - Via the AWS CLI (if configured): `aws s3 sync /backups s3://smoking-cessation-backups/`
   - Or manually download and upload via the S3 console

### Step 3: Archive S3 Data

For old data (> 90 days), set up lifecycle policies:

1. Go to the S3 console
2. Click on the bucket name (e.g., "leaflungs-images")
3. Go to the "Management" tab
4. Click "Create lifecycle rule"
5. Configure:
   - Name: "archive-old-data"
   - Filter: Prefix "chat/"
   - Transitions: Move to Glacier after 90 days
   - Expiration: Delete after 365 days
6. Click "Create rule"

---

## Part 5: Test Resource Cleanup (Non-Production)

### Step 1: Delete Test Lambda Functions

If any test functions exist:

1. Go to the Lambda console
2. Click on the test function name
3. Click "Actions" → "Delete"
4. Type the function name to confirm
5. Click "Delete"

### Step 2: Delete an Unused API Gateway

If you have multiple APIs for testing:

1. Go to the API Gateway console
2. Click on the API name
3. Click "Actions" → "Delete API"
4. Confirm the deletion

### Step 3: Delete Empty S3 Buckets

To delete a bucket:

1. Go to the S3 console
2. Click on the bucket name
3. Click "Empty" to remove all objects
4. Confirm emptying
5. Once it's empty, click the "Delete" button
6. Type the bucket name to confirm
7. Click "Delete bucket"

### Step 4: Delete an Unused CloudFront Distribution

To delete a CloudFront distribution:

1. Go to the CloudFront console
2. Click on the distribution
3. Click "Disable" (if it's enabled)
4. Wait 15 minutes for propagation
5. Once the status shows "Disabled", click "Delete"
6. Confirm the deletion

---

## Part 6: Delete Production Resources (if shutting down)

WARNING: This is destructive and cannot be undone!

### Backup Checklist Before Deletion

- [ ] PostgreSQL data backed up (pg_dump)
- [ ] MongoDB data backed up (mongodump)
- [ ] Database backups uploaded to S3
- [ ] S3 data backed up externally (if needed)
- [ ] CloudTrail logs reviewed & archived
- [ ] Code backed up to GitHub
- [ ] DNS records updated (if redirecting)
- [ ] Team notified

### Step 1: Delete the EC2 Database Instances

To delete the database EC2 instances:

1. Go to the EC2 console
2. Click "Instances"
3. Select a database instance (DB-PG or DB-Mongo)
4. Click "Instance State" → "Terminate"
5. Confirm the termination
6. Wait for the instance to terminate
7. Verify the EBS volumes are deleted (optional: create snapshots first if needed)
8. Repeat for the second database instance

### Step 2: Delete the Lambda Functions

To delete each Lambda function:

1. Go to the Lambda console
2. Click on the function name
3. Click "Actions" → "Delete"
4. Type the function name to confirm
5. Click "Delete"
6. Repeat for each function

### Step 3: Delete the API Gateways

To delete each API Gateway:

1. Go to the API Gateway console
2. Click on the API name (e.g., "LeafLungs-UserInfo-API")
3. Click "Actions" → "Delete API"
4. Confirm the deletion
5. Repeat for the second API (leaflungs-chat-api)

### Step 4: Delete the Cognito User Pool

To delete the Cognito User Pool:

1. Go to the Cognito console
2. Click "User pools"
3. Click on your user pool
4. Click "Delete user pool" (bottom right)
5. Type the user pool name to confirm
6. Click "Delete"

### Step 5: Delete the S3 Buckets (and their contents)

To delete each S3 bucket:

1. Go to the S3 console
2. For each bucket (leaflungs-frontend-new, leaflungs-images, leaflungs-images-sg):
   - Click on the bucket name
   - Click "Empty" to remove all objects
   - Confirm emptying
   - Once it's empty, click the "Delete" button
   - Type the bucket name to confirm
   - Click "Delete bucket"

### Step 6: Delete the CloudFront Distribution

To delete the CloudFront distribution:

1. Go to the CloudFront console
2. Click on the distribution (if it's not already disabled)
3. If it's enabled, click "Disable" first
4. Wait 15 minutes for propagation
5. Once the status shows "Disabled", click "Delete"
6. Confirm the deletion

---

## Part 7: Cost Analysis & Reporting

### Monthly Cost Breakdown

Typical costs for this architecture:

| Service | Usage | Cost |
|---------|-------|------|
| Lambda | 100K invocations/month | ~$2 |
| API Gateway | 10M requests/month | ~$50 |
| EC2 (2x DB instances) | 2 x t4g.small for PostgreSQL + MongoDB | ~$60-70 |
| EC2 (2x App instances) | 2 x t4g.small for applications | ~$60-70 |
| S3 | 100 GB storage | ~$2 |
| CloudFront | 1 TB/month transfer | ~$85 |
| Cognito | 1K users | ~$0 (free tier) |
| NAT Gateway | 10 GB transfer | ~$5 |
| **Total** | | ~**$300-350/month** |

Cost optimization opportunities:
1. Use t4g.nano/micro for light databases: Saves ~$20-30/month
2. Cache more aggressively: Saves ~$30/month
3. Reserved EC2 instances: Saves ~$60-80/month
4. Total savings: ~$110-140/month (35-40% reduction)

### AWS Cost Explorer

1. AWS Console → Billing → Cost Explorer
2. View costs by:
   - Service
   - Linked account
   - Region
   - Usage type
3. Set up cost anomaly detection
4. Review trends

---

## Part 8: Documentation & Lessons Learned

### Create a Post-Mortem Document

```markdown
# Workshop Completion Report

## Architecture Summary
- Services deployed: Lambda, API Gateway, EC2 (PostgreSQL + MongoDB), S3, CloudFront, Cognito, NLB
- Regions: us-east-1 (Cognito), ap-southeast-1 (main)
- Total users: 1000+
- Monthly costs: $300-350

## Key Learnings

1. Regional considerations
   - Cognito must be in us-east-1 for CloudFront
   - Main services are in ap-southeast-1 for latency

2. Security best practices implemented
   - VPC isolation
   - Security group restrictions
   - IAM least-privilege
   - Secrets Manager for credentials

3. Cost optimization
   - CloudFront caching is important
   - EC2 instances can be right-sized or use Reserved Instances
   - Reserved instances save 30-50%

4. Monitoring is critical
   - CloudWatch alarms prevented many issues
   - X-Ray helped debug performance

## Recommendations for the Next Phase

1. Set up automated backups for the EC2 databases (pg_dump/mongodump cronjobs)
2. Implement CI/CD for automated deployments
3. Add a comprehensive test suite
4. Set up an on-call rotation & runbooks
5. Schedule quarterly cost reviews
```

---

## Checklist - Cleanup Decision

Before the final cleanup, verify:

- [ ] All data is backed up
- [ ] Snapshots are created
- [ ] CloudTrail logs are archived
- [ ] Code is pushed to GitHub
- [ ] Cost analysis is completed
- [ ] The team is trained on the infrastructure
- [ ] Documentation is complete
- [ ] Stakeholders have approved
- [ ] A DNS cutover plan exists (if applicable)
- [ ] A rollback plan is documented

---

## Final Recommendations

1. Keep the infrastructure running (you've built it correctly)
2. Implement automated scaling if needed
3. Set up a CI/CD pipeline for updates
4. Add comprehensive testing
5. Plan quarterly cost reviews
6. Train the team on operational procedures
7. Consider a disaster recovery plan

---

## Summary & Next Steps

Congratulations! You've successfully:

1. ✓ Verified/set up Cognito authentication
2. ✓ Verified/set up Lambda functions
3. ✓ Verified/set up API Gateways
4. ✓ Verified the EC2 databases (PostgreSQL + MongoDB)
5. ✓ Verified S3 & CloudFront
6. ✓ Verified the VPC & Security
7. ✓ Implemented monitoring & logging
8. ✓ Optimized costs

This infrastructure can support:
- 1000+ concurrent users
- 100K+ requests/day
- Highly available (multi-AZ capable)
- Scalable (auto-scale Lambda, upgrade EC2 instance types)
- Secure (VPC isolation, encryption, IAM)
- Observable (comprehensive logging & monitoring)

---

## What You Will Achieve

After Module 10:

1. Resource inventory & usage analyzed
2. Cost optimization strategies identified
3. Unused resources identified for cleanup
4. Backup procedures implemented
5. A cost reduction plan created (up to 36% savings possible)
6. A post-mortem & lessons learned documented
7. Recommendations for the next phase
8. The workshop is complete & the infrastructure is production-ready
