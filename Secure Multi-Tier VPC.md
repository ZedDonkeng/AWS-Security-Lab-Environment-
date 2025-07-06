🔧 Tech: CloudFormation, NACLs, WAF
✅ Implements zero-trust networking with:

Public/private/isolated subnets
Application Load Balancer with WAF protection
VPC Flow Logs integration

<a href="https://youtu.be/svUj_aHjNVk" target="_blank">
  <img src="https://github.com/user-attachments/assets/d0953b7c-1ff4-4445-bc63-f7f014832cf7" width="720" height="400" />
</a>

### 1. Open IAM Console

### 2. Viewing Current Users
- [ ] **On the left-hand side, click on "Users" to view the current user list.**

### 3. Create a New IAM User and Set Password
- [ ] **Click on "Create user."**
- [ ] **Enter a username (e.g., admin).**
- [ ] **Select "Provide user access to the AWS Management Console."**
- [ ] **Choose "I want to Create an IAM user" option.**
- [ ] **Choose "Custom password" and enter your password.**
- [ ] **Uncheck "Users must create a new password at next sign-in.”**
- [ ] **Click "Next".**

### 4. Create a User Group and Assign Permissions
- [ ] **Choose "Add user to group."**
- [ ] **Click "Create group."**
- [ ] **Name the group (e.g., administration).**
- [ ] **Attach "AdministratorAccess" policy to the group.**
- [ ] **Click "Create user group".**
- [ ] **Add the user to the newly created admin group by selecting the group.**
- [ ] **Click "Next".**

### 5. Review and Create User

### 6. Verify User and Group Setup

### 7. Create an Account Alias (Optional)
- [ ] **Go to your AWS IAM Dashboard.**
- [ ] **Create an account alias (e.g., aws-adminaccess-v2).**

### 8. Sign in with IAM User
- [ ] **Open a new private browser window.**
- [ ] **Use the IAM sign-in URL.**
- [ ] **Enter account alias or account ID, and IAM username (e.g., admin).**
- [ ] **Enter the IAM user password to log in.**
- [ ] **Check the top right to ensure you're signed in as the IAM user.**
