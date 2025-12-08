---
title: "Module 3: Setup Cognito"
date: "2025-09-30"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Module 3: Create Cognito User Pool & Authentication

## Module Objectives

- Create a new Cognito User Pool
- Configure Sign-up & Sign-in options
- Create an App Client for the frontend
- Set up User Groups (admin, coach, user)
- Create test users
- Test the authentication flow

**Duration**: 3-4 hours

---

## Part 1: Create a Cognito User Pool

### Step 1: Access the Cognito Console

1. Log in to the AWS Console (https://console.aws.amazon.com/)
2. Search for "Cognito"
3. Click on "Cognito" from the services list
4. Click "User pools" (left menu)
5. Click "Create user pool"

![Cognito Console](../assets/03-cognito-create-pool-button.png)

### Step 2: Fill in "Set up resources for your application"

After clicking "Create user pool", you will see the "Set up resources for your application" form:

#### **2.1 Define your application**

1. **Application type**: Choose **"Single-page application (SPA)"**
   - This is the type of your React application
2. **Name your application**: Enter `smoking-cessation-app`
   - Limit: 128 characters, can only contain letters, numbers, spaces, +, =, ,, ., @, -

#### **2.2 Options for sign-in identifiers**

Choose these options:
- ☑️ **Email** (Check)
- ☐ Phone number (Uncheck)
- ☐ Username (Uncheck)

**Reason**: Email is the simplest way for users to log in.

#### **2.3 Self-registration**

- ☑️ **Enable self-registration** (Check)
  - This allows users to register themselves on the platform
  - A "Sign up" link will be displayed on the login page

#### **2.4 Required attributes for sign-up**

Choose the required attributes:
- ☑️ **email** (Already checked because you selected Email for sign-in)
- ☑️ **name** (Check - to store the user's name)

#### **2.5 Add a return URL (optional)**

1. Click on the "Return URL" field
2. Enter: `https://localhost:3000/callback`
   - This is where Cognito will redirect after a successful login
   - **Note**: For development, localhost supports HTTP; for production, it must be HTTPS

3. After filling it out, click the **"Create user directory"** button at the bottom.

![Set up resources](../assets/03-cognito-define-application.png)

### Step 3: Configure Security Requirements

After clicking "Authentication methods", you will be taken to the "Authentication methods" page:

1. **Password policy**:
   - Choose Cognito defaults in **Password policy mode**

2. **Edit email configuration**:
   - Choose **Send email with Cognito** in the Email provider
   - Click **Save changes**

3. **Account recovery**:
   - Self-service account recovery: ☑️ **Enable**
   - Recovery method:
     - ☑️ Email
     - ☑️ SMS

![Security Requirements](../assets/03-cognito-security-requirements.png)

### Step 4: Configure Sign-up Experience

"Configure sign-up experience" page:

1. **Self-registration** (enabled in step 2):
   - Enable self-registration: ✅ **Yes**
   - Allow users to sign themselves up: ✅ **Yes**

2. **Standard attributes to collect**:
   - ☑️ **email** (Required)
   - ☑️ **name** (Required)
   - ☐ phone_number (Optional - not needed)
   - ☐ family_name (Optional - not needed)

3. **Verification settings**:
   - How will a user be confirmed?: **Email**
   - Cognito will send a confirmation link via email

4. Click **"Next"**

![Sign-up Experience](../assets/03-cognito-signup-experience.png)

### Step 5: Configure Message Delivery

"Configure message delivery" page:

1. **Email provider**:
   - Select: **Cognito (default)**
   - Note: The free tier allows 50 emails/day. For production, use Amazon SES.

2. **From email address**:
   - Use the Cognito default email
   - Emails will be sent from `no-reply@cognito.amazonaws.com`

3. Click **"Next"**

![Message Delivery](../assets/03-cognito-message-delivery.png)

### Step 6: Review & Create

The final "Review and create" page:

1. Review all the settings you have configured:
   - Application type: Single-page application (SPA)
   - Application name: smoking-cessation-app
   - Sign-in experience
   - Security requirements
   - Sign-up experience
   - Message delivery

2. Scroll down and enter the **User pool name**:
   - Name: `smoking-cessation-users`
   - This is the name of the User Pool (different from the Application name)

3. Click **"Create user pool"**

![Create User Pool](../assets/03-cognito-create-confirm.png)

⏳ **Wait about 2-3 minutes** for the user pool to be created. You will see a success message when it's done.

### Step 7: Success Page - Quick Setup Guide

After the user pool is created, you will see the "Set up resources for your application" page with several options:

![Success Page](../assets/03-cognito-success.png)

This page displays:
1. **"Your application ... have been created successfully!"** - Success notification
2. **"Check out your sign-in page"** - A link to test the login page
3. **"What's the development platform for your single page application?"** - Options (React, Angular, JavaScript)
4. **Code examples** - Guidance for frontend integration

**Note**: You will configure the integration code in the next Module. For now, click **"Go to overview"** at the bottom to go to the user pool overview page.

![Go to Overview](../assets/03-cognito-go-overview.png)

---

## Part 2: Get the User Pool ID

After the user pool is created:

1. You will see a success message
2. **Note the User Pool ID**: The format is `ap-southeast-1_xxxxxxxxxxxxx`
   - Example: `ap-southeast-1_GAXOSoku5`
3. Save it to your `.env` file:
   ```
   COGNITO_USER_POOL_ID=ap-southeast-1_dskxxxxt3
   COGNITO_REGION=ap-southeast-1
   ```

![User Pool ID](../assets/03-cognito-pool-id.png)

---

## Part 3: Create an App Client

### Step 1: Access App Integration

1. From the User Pool dashboard (smoking-cessation-users)
2. Left menu: Click "App integration"
3. Click "App clients and analytics"
4. Click "Create app client"

![App Integration Menu](../assets/03-cognito-app-integration.png)

### Step 2: Configure the App Client

1. **App client name**: `smoking-cessation-frontend`
2. **Refresh token expiration**: 30 days
3. **Access token expiration**: 1 hour (3600 seconds)
4. **ID token expiration**: 1 hour (3600 seconds)
5. **Token validity units**: hours
6. Click "Next"

![App Client Basic Config](../assets/03-cognito-app-client-config.png)

### Step 3: Configure Authentication Flows

1. **Authentication flows and security**
2. Select:
   - ✅ ALLOW_USER_PASSWORD_AUTH (for username/password login)
   - ✅ ALLOW_REFRESH_TOKEN_AUTH (for refresh token rotation)
   - ✅ ALLOW_USER_SRP_AUTH (secure password authentication)
3. Click "Next"

![Auth Flows](../assets/03-cognito-auth-flows.png)

### Step 4: Configure Hosted UI (Optional but Recommended)

1. **Hosted UI settings**
2. **Hosted UI domain name**: `smoking-cessation-dev`
   - Click "Check availability"
   - If taken, append `-{number}` (e.g., `smoking-cessation-dev-2`)
3. **Allowed callback URLs** (add for your frontend):
   - For development: `http://localhost:3000/callback`
   - For production: `https://yourdomain.com/callback`
4. **Allowed sign-out URLs**:
   - For development: `http://localhost:3000/logout`
   - For production: `https://yourdomain.com/logout`
5. **Allowed OAuth 2.0 scopes**:
   - ✅ openid
   - ✅ email
   - ✅ profile
6. Click "Next"

![Hosted UI Config](../assets/03-cognito-hosted-ui.png)

### Step 5: Advanced Security Settings

1. **Advanced security settings**
2. **Prevent user existence errors**: ✅ **Enable**
   - This prevents attackers from discovering valid usernames
3. Click "Create app client"

![Advanced Security](../assets/03-cognito-advanced-security.png)

⏳ **Wait for the app client to be created**

### Step 6: Get the App Client ID

After the App Client is created:

1. Find the **Client ID** in the app client details section
   - Example: `4175kqc33olfjinhkll4jme379`
2. Save it to your `.env` file:
   ```
   COGNITO_CLIENT_ID=4175kqc33olfjinhkll4jme379
   ```

![App Client ID](../assets/03-cognito-client-id.png)

---

## Part 4: Create User Groups

### Step 1: Access User Groups

1. From the User Pool (smoking-cessation-users)
2. Left menu: Click "User groups"
3. Click "Create group"

![User Groups Menu](../assets/03-cognito-user-groups-menu.png)

### Step 2: Create the Admin Group

1. **Group name**: `admins`
2. **Description**: `Platform administrators with full access`
3. **Assign IAM role to this group**: (Optional, skip for now)
4. Click "Create group"

![Create Admin Group](../assets/03-cognito-create-admin-group.png)

### Step 3: Create the Coach Group

1. Click "Create group"
2. **Group name**: `coaches`
3. **Description**: `Coaches who help users quit smoking`
4. Click "Create group"

![Create Coach Group](../assets/03-cognito-create-coach-group.png)

### Step 4: Create the User Group

1. Click "Create group"
2. **Group name**: `users`
3. **Description**: `Regular users of the platform`
4. Click "Create group"

![Create User Group](../assets/03-cognito-create-user-group.png)

---

## Part 5: Create Test Users

### Step 1: Access Users

1. From the User Pool (smoking-cessation-users)
2. Left menu: Click "Users"
3. Click "Create user"

![Users Menu](../assets/03-cognito-users-menu.png)

### Step 2: Create an Admin User

1. **Username**: `admin-test`
2. **Email address**: `admin@test.com`
3. **Temporary password**: `TempAdminPass123!`
4. **Mark email as verified**: ✅ **Check**
5. **Mark phone number as verified**: ☐
6. Click "Create user"

![Create Admin User](../assets/03-cognito-create-admin.png)

### Step 3: Assign the Admin User to the Admin Group

1. Click on the newly created `admin-test` user
2. Scroll down to "Group membership"
3. Click "Add user to groups"
4. Select the `admins` group
5. Click "Add user to groups"

![Assign to Admin Group](../assets/03-cognito-assign-admin-group.png)

### Step 4: Create a Coach User

1. Go back to the Users list
2. Click "Create user"
3. **Username**: `coach-test`
4. **Email address**: `coach@test.com`
5. **Temporary password**: `TempCoachPass123!`
6. **Mark email as verified**: ✅ **Check**
7. Click "Create user"

![Create Coach User](../assets/03-cognito-create-coach.png)

### Step 5: Assign the Coach User to the Coach Group

1. Click on the `coach-test` user
2. Scroll down to "Group membership"
3. Click "Add user to groups"
4. Select the `coaches` group
5. Click "Add user to groups"

![Assign to Coach Group](../assets/03-cognito-assign-coach-group.png)

### Step 6: Create a Regular User

1. Go back to the Users list
2. Click "Create user"
3. **Username**: `user-test`
4. **Email address**: `user@test.com`
5. **Temporary password**: `TempUserPass123!`
6. **Mark email as verified**: ✅ **Check**
7. Click "Create user"
8. Assign to the `users` group (similar to step 5)

![Create Regular User](../assets/03-cognito-create-user.png)

---

## Part 6: Set Permanent Passwords (Optional)

If you want users to be able to log in immediately without changing their temporary password:

1. From the Users list
2. Click on a user (e.g., `admin-test`)
3. Click "Actions" → "Set password"
4. **Permanent password**: `AdminPass123!`
5. **Make this permanent password**: ✅ **Check**
6. Click "Set password"

![Set Permanent Password](../assets/03-cognito-set-password.png)

---

## Part 7: Additional App Client Configuration (Authentication Methods)

### Step 1: Configure App Client Details

1. From the User Pool → App integration → App clients
2. Click on `smoking-cessation-frontend`
3. Scroll down → "Client secret"
   - ⚠️ **Note**: If you create a Client Secret, your frontend JavaScript will not be able to use it (because the secret cannot be stored securely on the client)
   - **Recommendation**: Do not create a Client Secret for a public frontend
4. Leave "Client secret" as-is (do not create)

![App Client Details](../assets/03-cognito-app-client-details.png)

---

## Part 8: Enable Cognito Hosted UI (Optional)

### Step 1: Configure the Hosted UI Domain

1. From the User Pool → App integration → Domain name
2. If already configured → Skip
3. If not → Click "Create domain"
4. **Domain prefix**: `smoking-cessation-dev`
5. Click "Create domain"

⏳ **Wait 1-2 minutes** for the domain to be created

### Step 2: Test the Hosted UI

1. From App integration → App clients
2. Click on `smoking-cessation-frontend`
3. Scroll down to "Hosted UI settings"
4. Find the **Hosted UI domain URL**:
   - Format: `https://smoking-cessation-dev.auth.us-east-1.amazoncognito.com`
5. Click the link to open the Hosted UI
6. Test login:
   - Username: `admin-test`
   - Password: `AdminPass123!`
   - Should redirect to `http://localhost:3000/callback` (or the configured callback URL)

![Hosted UI Login](../assets/03-cognito-hosted-ui-login.png)

---

## Part 9: Summary Information

Save this information to your `.env` file:

```env
# Cognito Configuration
COGNITO_REGION=us-east-1
COGNITO_USER_POOL_ID=us-east-1_dskUsnKt3
COGNITO_CLIENT_ID=4175kqc33olfjinhkll4jme379
COGNITO_DOMAIN=smoking-cessation-dev
COGNITO_HOSTED_UI_DOMAIN=https://smoking-cessation-dev.auth.us-east-1.amazoncognito.com

# Test User Credentials (to be removed before production)
TEST_ADMIN_USER=admin-test
TEST_ADMIN_PASSWORD=AdminPass123!

TEST_COACH_USER=coach-test
TEST_COACH_PASSWORD=TempCoachPass123!

TEST_USER=user-test
TEST_USER_PASSWORD=TempUserPass123!
```

![Environment Variables](../assets/03-cognito-env-variables.png)

---

## Part 10: Troubleshooting

### "Email already exists"

**Problem**: Creating a user but the email already exists.

**Solution**:
1. From the Users list
2. Find the user with that email
3. Delete it (if it's a test user)
4. Create a new user with a different email

### "Temporary password doesn't meet requirements"

**Problem**: The password does not meet the requirements.

**Solution**:
- The password must:
  - Be a minimum of 12 characters
  - Have an uppercase letter (A-Z)
  - Have a lowercase letter (a-z)
  - Have a number (0-9)
  - Have a special character (!@#$%^&*)
- Valid examples: `TempPass123!`, `AdminTest456!`

### "Hosted UI domain not available"

**Problem**: The domain is already in use.

**Solution**:
- Add a number to the suffix: `smoking-cessation-dev-2`
- Or use a different name

---

## Checklist

- [ ] User Pool "smoking-cessation-users" created successfully
- [ ] App Client "smoking-cessation-frontend" created successfully
- [ ] 3 User Groups created (admins, coaches, users)
- [ ] 3 test users created & assigned to groups
- [ ] Permanent passwords set for test users
- [ ] Hosted UI domain configured
- [ ] Login test successful with admin-test user
- [ ] .env file updated with Cognito credentials
- [ ] Ready for Module 4 (Setup Lambda)

---

## What You Will Achieve

After Module 3, you will have:

1. ✅ A fully functional Cognito User Pool
2. ✅ An App Client for the frontend to connect to
3. ✅ User Groups (admins, coaches, users) for role-based access control
4. ✅ Test users to verify the authentication flow
5. ✅ A Hosted UI domain that can be used for login/signup
6. ✅ Credentials saved for use in the following modules
