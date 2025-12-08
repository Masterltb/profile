---
title: "Module 5: Setup API Gateway"
date: "2025-09-30"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Module 5: Create API Gateway REST APIs

## Module Objectives

- Create 2 REST APIs from scratch
- Create resources & methods
- Integrate with Lambda functions
- Configure authentication & authorization
- Set up CORS & rate limiting
- Deploy & test the APIs

**Duration**: 3-4 hours

---

## API Gateway Overview

2 REST APIs will be created:

| API Name | Region | Purpose | Resources | Methods |
|----------|--------|---------|-----------|---------|
| smoking-cessation-user-api | ap-southeast-1 | User management, Admin operations | /admin/coaches, /api/user-info, /upload | GET, POST, PUT, DELETE |
| smoking-cessation-chat-api | ap-southeast-1 | Chat & WebSocket | /chat/rooms, /chat/messages, /ws | GET, POST, WebSocket |

---

## Part 1: Create the First REST API (User Management)

### Step 1: Access the API Gateway Console

1. Log in to the AWS Console
2. Search for "API Gateway"
3. Click the "API Gateway" service
4. Select the region: **ap-southeast-1**
5. Click "Create API"

![API Gateway Console](../assets/05-api-gateway-console.png)

### Step 2: Choose the API Type

1. Choose "REST API"
2. Click "Build"

![Choose API Type](../assets/05-choose-api-type.png)

### Step 3: Configure API Details

1. **API name**: `smoking-cessation-user-api`
2. **Description**: REST API for user management and admin operations
3. **Endpoint type**: Regional
4. Click "Create API"

⏳ **Wait for the API to be created (about 1-2 minutes)**

![Create API](../assets/05-create-api.png)

### Step 4: View the API Dashboard

After the API is created, you will see:
- The resources tree (currently only the "/" root)
- The methods available for the root
- Integration settings

![API Created](../assets/05-api-created.png)

---

## Part 2: Create the /admin Resource & /admin/coaches Sub-resource

### Step 1: Create the /admin Resource

1. Click the root "/"
2. Click "Create Resource"
3. **Resource name**: `admin`
4. **Resource path**: `/admin`
5. ✅ "Enable API Gateway CORS"
6. Click "Create Resource"

![Create Admin Resource](../assets/05-create-admin-resource.png)

### Step 2: Create the /admin/coaches Sub-resource

1. Click on the `/admin` resource
2. Click "Create Resource"
3. **Resource name**: `coaches`
4. **Resource path**: `coaches` (automatically becomes `/admin/coaches`)
5. ✅ "Enable API Gateway CORS"
6. Click "Create Resource"

![Create Coaches Resource](../assets/05-create-coaches-resource.png)

### Step 3: Create the GET Method for /admin/coaches

1. Click on the `/admin/coaches` resource
2. Click "Create Method"
3. Select "GET"
4. Click the checkmark to confirm

![Create GET Method](../assets/05-create-get-method.png)

### Step 4: Configure the GET Method Integration

1. **Integration type**: Lambda Function
2. **Lambda Region**: ap-southeast-1
3. **Lambda Function**: `smoking-cessation-admin-manage-coaches`
4. ✅ "Use Lambda Proxy integration"
5. Click "Save"
6. **Confirm** the permission popup

![Configure Lambda Integration](../assets/05-configure-lambda-integration.png)

### Step 5: Create the POST Method for /admin/coaches

1. Click "Create Method"
2. Select "POST"
3. Same integration as GET:
   - **Integration type**: Lambda Function
   - **Lambda Function**: `smoking-cessation-admin-manage-coaches`
   - ✅ "Use Lambda Proxy integration"
4. Click "Save"

### Step 6: Create the PUT Method for /admin/coaches

1. Click "Create Method"
2. Select "PUT"
3. Same Lambda integration
4. Click "Save"

### Step 7: Create the DELETE Method for /admin/coaches

1. Click "Create Method"
2. Select "DELETE"
3. Same Lambda integration
4. Click "Save"

**Result**: /admin/coaches now has GET, POST, PUT, and DELETE methods.

---

## Part 3: Create the /api/user-info Resource & Sub-resources

### Step 1: Create the /api Resource

1. Click the root "/"
2. Click "Create Resource"
3. **Resource name**: `api`
4. **Resource path**: `/api`
5. ✅ "Enable API Gateway CORS"
6. Click "Create Resource"

### Step 2: Create the /api/user-info Resource

1. Click on the `/api` resource
2. Click "Create Resource"
3. **Resource name**: `user-info`
4. **Resource path**: `user-info`
5. ✅ "Enable API Gateway CORS"
6. Click "Create Resource"

### Step 3: Create Methods for /api/user-info

Create GET, POST, PUT, and DELETE methods (same as /admin/coaches):

1. For each method:
   - Click "Create Method"
   - Select the method type
   - Integration: `smoking-cessation-admin-manage-coaches` Lambda
   - ✅ Lambda Proxy integration
   - Save

### Step 4: Create the /{id} Resource (Path Parameter)

1. Click on `/api/user-info`
2. Click "Create Resource"
3. **Resource name**: `{id}`
4. **Resource path**: `{id}`
5. Click "Create Resource"

### Step 5: Create the GET Method for /api/user-info/{id}

1. Click on the `/{id}` resource
2. Click "Create Method"
3. Select "GET"
4. Integration: `smoking-cessation-admin-manage-coaches` Lambda
5. Save

### Step 6: Create the /by-user-id Resource

1. Click on `/api/user-info` (parent)
2. Click "Create Resource"
3. **Resource name**: `by-user-id`
4. **Resource path**: `by-user-id`
5. Click "Create Resource"
6. Add a GET method with Lambda integration

### Step 7: Create the /empty Resource

1. Click on `/api/user-info` (parent)
2. Click "Create Resource"
3. **Resource name**: `empty`
4. **Resource path**: `empty`
5. Click "Create Resource"
6. Add a GET method with Lambda integration

**Result**: The /api/user-info resource tree is created with all sub-resources.

---

## Part 4: Create the /upload Resource

### Step 1: Create the /upload Resource

1. Click the root "/"
2. Click "Create Resource"
3. **Resource name**: `upload`
4. **Resource path**: `/upload`
5. ✅ "Enable API Gateway CORS"
6. Click "Create Resource"

### Step 2: Create the POST Method

1. Click on the `/upload` resource
2. Click "Create Method"
3. Select "POST"
4. **Integration type**: Lambda Function
5. **Lambda Function**: `smoking-cessation-image-upload`
6. ✅ "Use Lambda Proxy integration"
7. Click "Save"

---

## Part 5: Configure Authentication & Authorization

### Step 1: Create a Cognito Authorizer

1. Left menu: Click "Authorizers"
2. Click "Create Authorizer"
3. **Authorizer name**: `cognito-user-pool-authorizer`
4. **Type**: Cognito
5. **Cognito User Pool**: `smoking-cessation-users` (created in Module 3)
6. **Token source**: `Authorization`
7. Click "Create authorizer"

![Create Authorizer](../assets/05-create-authorizer.png)

### Step 2: Test the Authorizer (Optional)

1. **Token**: (enter a valid JWT token from your Cognito user pool)
2. Click "Test authorizer"
3. Verify the response shows the user claims

### Step 3: Add Authorization to Methods

For critical endpoints (e.g., /admin/coaches):

1. Click on the `/admin/coaches` resource
2. Click the "GET" method
3. Click "Method Request"
4. **Authorization**: Select `cognito-user-pool-authorizer`
5. Click the checkmark to save
6. Repeat for the POST, PUT, and DELETE methods

![Add Authorization](../assets/05-add-authorization.png)

---

## Part 6: Set up Request Validators

### Step 1: Create a Request Validator

1. Left menu: Click "Request Validators"
2. Click "Create"
3. **Name**: `validate-body-and-params`
4. ✅ "Validate request body"
5. ✅ "Validate query string parameters and headers"
6. Click "Create"

### Step 2: Apply the Validator to POST Methods

1. Click on `/admin/coaches` → "POST" method
2. Click "Method Request"
3. **Request Validator**: Select `validate-body-and-params`
4. Save

---

## Part 7: Set up CORS

### Step 1: Enable CORS for All Resources

1. Click the root "/"
2. Click "Enable CORS"
3. Review the default settings
4. Click "Enable CORS and replace existing CORS headers"

![Enable CORS](../assets/05-enable-cors.png)

### Step 2: Configure CORS Headers

The CORS headers are automatically configured:
- **Access-Control-Allow-Headers**: Content-Type, X-Amz-Date, Authorization, X-Api-Key, X-Amz-Security-Token
- **Access-Control-Allow-Methods**: GET, POST, PUT, DELETE, OPTIONS
- **Access-Control-Allow-Origin**: * (for dev, restrict in production)

---

## Part 8: Create the Second API - Chat API (WebSocket)

### Step 1: Create a WebSocket API

1. Click "Create API"
2. Select "WebSocket API"
3. Click "Build"

![Create WebSocket API](../assets/05-create-websocket.png)

### Step 2: Configure the WebSocket API

1. **API name**: `smoking-cessation-chat-api`
2. **Description**: WebSocket API for chat functionality
3. **Route Selection Expression**: `$request.body.action`
4. Click "Create API"

⏳ **Wait for the API to be created**

### Step 3: Create Routes

Create the default routes:

1. **$default** route:
   - Integration: Lambda Function
   - Function: `smoking-cessation-websocket-authorizer`

2. **$connect** route:
   - Integration: Lambda Function
   - Function: `smoking-cessation-websocket-authorizer`

3. **$disconnect** route:
   - Integration: Lambda Function
   - Function: `smoking-cessation-websocket-authorizer`

---

## Part 9: Deploy the APIs

### Step 1: Create a Deployment Stage

For the User API:

1. Click "Deploy API"
2. **Stage name**: `prod`
3. **Stage description**: Production environment
4. Click "Deploy"

![Deploy API](../assets/05-deploy-api.png)

### Step 2: Note the API Endpoint

After deployment, you'll see:
- **Invoke URL**: `https://{api-id}.execute-api.ap-southeast-1.amazonaws.com/prod`
- Save this URL

### Step 3: Deploy the Chat API

1. Go to the Chat API
2. Click "Deploy API"
3. **Stage name**: `prod`
4. Click "Deploy"
5. Note the WebSocket Invoke URL

---

## Part 10: Test the REST API Endpoints

### Step 1: Test GET /admin/coaches

1. In the API Gateway console:
   - Select the User API
   - Click `/admin/coaches` → "GET"
   - Click "Test"

2. **Method**: GET
3. **Path**: /admin/coaches
4. **Headers**:
   ```
   Authorization: Bearer {your-cognito-token}
   ```
5. Click "Test"
6. **Expected result**:
   - Status: 200
   - Body: JSON response from Lambda

![Test API](../assets/05-test-api.png)

### Step 2: Test POST /admin/coaches

1. Click `/admin/coaches` → "POST"
2. Click "Test"
3. **Method**: POST
4. **Headers**:
   ```
   Authorization: Bearer {your-cognito-token}
   Content-Type: application/json
   ```
5. **Body**:
   ```json
   {
     "name": "Coach Name",
     "email": "coach@example.com",
     "specialization": "Smoking Cessation"
   }
   ```
6. Click "Test"
7. **Expected**: Status 200 with the created coach data

### Step 3: Test the /upload Endpoint

1. Click `/upload` → "POST"
2. Click "Test"
3. **Method**: POST
4. **Headers**:
   ```
   Authorization: Bearer {your-cognito-token}
   Content-Type: multipart/form-data
   ```
5. Click "Test"
6. **Expected**: Status 200 with an S3 URL

### Step 4: Test with cURL (Optional)

```bash
# Get a list of coaches
curl -X GET \
  https://{api-id}.execute-api.ap-southeast-1.amazonaws.com/prod/admin/coaches \
  -H "Authorization: Bearer {token}" \
  -H "Content-Type: application/json"

# Create a new coach
curl -X POST \
  https://{api-id}.execute-api.ap-southeast-1.amazonaws.com/prod/admin/coaches \
  -H "Authorization: Bearer {token}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Coach John",
    "email": "john@example.com",
    "specialization": "Smoking Cessation"
  }'
```

---

## Part 11: Set up CloudWatch Logging

### Step 1: Enable Full Request/Response Logging

1. Click "Settings" (left menu)
2. **CloudWatch log role ARN**:
   - Click "Edit"
   - Select or create an IAM role for API Gateway logging
   - The role should have `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents` permissions
3. Click "Save changes"

### Step 2: Set the Log Level

1. Go to "Stages" → "prod"
2. Click "Logs"
3. ✅ "Enable CloudWatch Logs"
4. **Log level**: INFO (or DEBUG for troubleshooting)
5. **Data trace enabled**: ✅
6. **Full request/response data**: ✅
7. Click "Save changes"

### Step 3: View the Logs

1. Search for "CloudWatch"
2. Click the "CloudWatch" service
3. Left menu: "Logs" → "Log groups"
4. Search for `API-Gateway-Execution-Logs_{api-id}`
5. View recent requests

![CloudWatch Logs](../assets/05-cloudwatch-logs.png)

---

## Part 12: Set up Rate Limiting (Usage Plans)

### Step 1: Create an API Key

1. Left menu: Click "API Keys"
2. Click "Create API Key"
3. **Name**: `mobile-app-key`
4. **Description**: API key for the mobile app
5. Click "Create API Key"
6. Copy and save the key

### Step 2: Create a Usage Plan

1. Left menu: Click "Usage Plans"
2. Click "Create"
3. **Name**: `free-tier-plan`
4. **Description**: Free tier plan with rate limiting
5. Click "Next"

### Step 3: Configure Throttling & Quota

1. **Rate**: 100 requests per second
2. **Burst**: 200 requests
3. **Quota**: 1,000,000 requests per month
4. Click "Next"

### Step 4: Associate the API with the Plan

1. **Associated API Stages**:
   - Select the User API
   - Select the stage: `prod`
2. Click "Next"

### Step 5: Associate the API Key with the Plan

1. **API Keys**:
   - Search for and select `mobile-app-key`
2. Click "Finish"

---

## Part 13: Set up a Resource-Based Policy (Optional)

To restrict API access to specific IAM users/roles:

### Step 1: Add a Resource Policy

1. Left menu: Click "Resource Policy"
2. Click "Edit"
3. Paste the policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "execute-api:Invoke",
      "Resource": "execute-api:/*"
    }
  ]
}
```

4. Click "Save"

---

## Part 14: Enable API Caching (Optional)

### Step 1: Enable the Cache for GET Endpoints

1. Go to the stage: "prod"
2. Click "Settings"
3. **Cache cluster enabled**: ✅
4. **Cache cluster size**: 0.5 (smallest)
5. Click "Save changes"

### Step 2: Configure the Cache per Method

1. Click on `/admin/coaches` → "GET"
2. Click "Integration Response"
3. Expand the "200" response
4. **Cache settings**: ✅ "Enable method caching"
5. **Cache time to live (TTL)**: 300 seconds (5 minutes)
6. Click the checkmark

---

## Environment Variables Summary

Save these URLs after API deployment:

```env
# User Management API
USER_API_ENDPOINT=https://{api-id-1}.execute-api.ap-southeast-1.amazonaws.com/prod
USER_API_ID={api-id-1}

# Chat API (WebSocket)
CHAT_API_ENDPOINT=wss://{api-id-2}.execute-api.ap-southeast-1.amazonaws.com/prod
CHAT_API_ID={api-id-2}

# Authorizer
COGNITO_AUTHORIZER_ID=cognito-user-pool-authorizer

# Rate Limiting
API_KEY={your-api-key}
```

---

## Checklist

- [ ] User API (smoking-cessation-user-api) created
- [ ] /admin/coaches resource with GET, POST, PUT, and DELETE methods created
- [ ] /api/user-info resource with sub-resources created
- [ ] /upload resource with a POST method created
- [ ] Chat API (smoking-cessation-chat-api) created
- [ ] Cognito authorizer configured
- [ ] CORS enabled for all resources
- [ ] Request validators configured
- [ ] Both APIs deployed to the `prod` stage
- [ ] CloudWatch logging enabled
- [ ] Rate limiting with Usage Plans configured
- [ ] All endpoints tested successfully
- [ ] API endpoints saved to environment variables
- [ ] Ready for Module 6 (Create EC2 Instances & Databases)

---

## Troubleshooting

### CORS Errors in the Browser

**Issue**: "Access to XMLHttpRequest blocked by CORS policy"

**Solution**:
1. Verify that CORS is enabled for the resource
2. Check the Access-Control-Allow-Origin header
3. For development, use the `*` wildcard
4. For production, restrict to a specific domain

### 403 Unauthorized Errors

**Issue**: "User is not authorized to perform: execute-api:Invoke"

**Solution**:
1. Verify the JWT token is valid
2. Check that the authorizer is configured on the method
3. Verify the user has the required role in Cognito
4. Review the API resource policy

### Lambda Function Not Found

**Issue**: "Invalid Lambda function ARN specified"

**Solution**:
1. Verify the Lambda function exists in the same region
2. Check the Lambda function name spelling
3. Verify the IAM role has Lambda invoke permissions
4. Check the CloudWatch logs for integration errors

### Rate Limiting Not Working

**Issue**: "Requests are not being throttled according to the plan"

**Solution**:
1. Verify the API key is passed in the request
2. Check that the Usage Plan is associated with the stage
3. Verify the API key is associated with the Usage Plan
4. Wait for throttling to take effect (may take a few minutes)

### High Latency

**Issue**: "API requests are taking too long"

**Solution**:
1. Enable caching for GET requests
2. Increase the Lambda memory (Module 4)
3. Check the database connection from Lambda (Module 6)
4. Monitor CloudWatch metrics

---

## Next Steps

1. Implement API Gateway request/response transformations (optional)
2. Set up a WAF (Web Application Firewall) for security (optional)
3. Configure CloudFront for API caching (Module 7)
4. Set up API documentation (optional)
5. Test the end-to-end flow with the frontend application

---

## What You Will Achieve

After Module 5, you will have:

1. ✅ 2 REST APIs created (User API & Chat API)
2. ✅ All resources & methods configured
3. ✅ Lambda integrations set up
4. ✅ Cognito authorization configured
5. ✅ CORS enabled for all endpoints
6. ✅ Request validators configured
7. ✅ Both APIs deployed to the `prod` stage
8. ✅ CloudWatch logging enabled
9. ✅ Rate limiting configured
10. ✅ All endpoints tested & verified
11. ✅ API endpoints documented
12. ✅ Ready for Module 6 (Create EC2 Instances & Databases)
