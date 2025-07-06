<summary>Project 3 - Install AWS CLI on Windows PC</summary>

  ###

  <a href="https://youtu.be/h5HW1z7BS9M" target="_blank"><img src="https://github.com/user-attachments/assets/e89b6c6f-a2c1-4988-b50b-7e0e5eba1883" width="720" height="400" /></a>

  ### 1. Search for AWS CLI Installation
  ### 2. Download and Run AWS CLI Version 2 Installer
  ### 3. Verify AWS CLI Installation
  - [ ] **Open "Command Prompt" on Windows.**
  - [ ] **Type `aws --version` and press Enter.**
  - [ ] **Check for output that starts with "aws-cli/2" followed by the Python version and Windows details.**
  - [ ] **Confirm that AWS CLI version 2 is installed and working correctly.**

</details>

<details>



###
  <summary>Project 4 - Create CLI Access Keys</summary>

  ###

  <a href="https://youtu.be/YFVP_o9Z_1k" target="_blank"><img src="https://github.com/user-attachments/assets/a3a7667a-22db-4c9a-b64e-9f5f850e24e5" width="720" height="400" /></a>

  ### 1. Navigate to Security Credentials
  - [ ] **Open the IAM Console.**
  - [ ] **Click on your username (e.g., admin).**
  - [ ] **Go to the "Security credentials" section.**
  - [ ] **Scroll down to the "Access keys" section.**
  - [ ] **Click "Create access key."**

  ### 2. Create an Access Key
  - [ ] **Choose the purpose for the access key, such as CLI.**
  - [ ] **Acknowledge AWS's recommendations regarding alternative methods.**
  - [ ] **Check "I understand the above recommendation" to proceed.**
  - [ ] **Generate the access key and secret access key.**
  - [ ] **Save the access key and secret access key immediately as this is the only time they will be visible.**

  ### 3. Configure AWS CLI
  - [ ] **Open Command Prompt on Windows.**
  - [ ] **Run the command `aws configure`.**
  - [ ] **Enter the access key ID when prompted.**
  - [ ] **Enter the secret access key when prompted.**
  - [ ] **Set the default region (e.g., us-east-2).**
  - [ ] **Set the default output format (press Enter to skip or choose format).**

  ### 4. Verify AWS CLI Configuration
  - [ ] **Execute a test command like `aws iam list-users`.**
  - [ ] **Confirm the output matches your IAM console, showing user details.**

  ### 5. Modify User Permissions
  - [ ] **Use your root account to remove the user (e.g., admin) from the "administration" group.**
  - [ ] **Verify the user now has no permissions.**

  ### 6. Test Permissions with CLI
  - [ ] **Run `aws iam list-users` in CLI.**
  - [ ] **Confirm that no response is returned due to lack of permissions.**

  ### 7. Re-add User to Administration Group
  - [ ] **Go back to "User groups."**
  - [ ] **Add the user (e.g., admin) back into the "admin" group to restore permissions.**
  - [ ] **Verify that the user permissions have been successfully restored.**

</details>

<details>
