# Video 2: Enterprise Automation with PowerShell (`workshop.lab`)

Use the checkboxes `[ ]` below to track your progress as you complete each step of the automation lab.

## 📋 Lab Scenario Overview

Manually clicking through the Active Directory GUI to create accounts works for 1 or 2 users, but it does not scale in a real corporate environment. As a Systems Administrator, you will use **PowerShell** to automate user provisioning.

In this lab, you will write a PowerShell script that:

1. Reads a comma-separated values (`.csv`) file containing a list of 50 new hires.
    
2. Automatically creates an Active Directory account for each person.
    
3. Sets their initial passwords and forces a password change at next logon.
    
4. Places them into a targeted Organizational Unit (OU) based on their department (**Sales, Marketing, IT, HR, or Finance**).
    

## 📂 Phase 1: Creating the New Hire CSV Data File

_Perform these steps on your Windows Server (**DC-01**)._

- [ ] **1.** Open **Notepad** on the server.
    
- [ ] **2.** Copy and paste the 50-user dataset provided in the  Notepad.
    
- [ ] **3.** Click **File** $\rightarrow$ **Save As...**
    
- [ ] **4.** Change the _Save as type_ dropdown to **All Files (*.*)**.
    
- [ ] **5.** Name the file `NewHires.csv` and save it directly to the root of your C: drive (`C:\NewHires.csv`).
    

## 🛠️ Phase 2: Preparing the Active Directory Structure

Before running the automation script, the target OUs must exist so Active Directory knows where to drop the users.

- [ ] **1.** Open **Server Manager** $\rightarrow$ **Tools** $\rightarrow$ **Active Directory Users and Computers**.
    
- [ ] **2.** Right-click your domain name (**`workshop.lab`**) $\rightarrow$ **New** $\rightarrow$ **Organizational Unit**.
    
- [ ] **3.** Name the OU `Corporate` and click **OK**.
    
- [ ] **4.** Right-click your newly created `Corporate` OU $\rightarrow$ **New** $\rightarrow$ **Organizational Unit**. Name it `Sales`.
    
- [ ] **5.** Repeat the step above to create four more OUs inside `Corporate` named:
    
    - `Marketing`
        
    - `IT`
        
    - `HR`
        
    - `Finance`
        

> 💡 _Check Point:_ You should now have a hierarchy that looks like this: `workshop.lab` $\rightarrow$ `Corporate` $\rightarrow$ `Sales`, `Marketing`, `IT`, `HR`, and `Finance`.

## 💻 Phase 3: Writing the PowerShell Script

_Perform these steps on your Windows Server (**DC-01**)._

- [ ] **1.** Click the Windows Start Menu, search for **PowerShell ISE**, right-click it, and select **Run as administrator**.
    
- [ ] **2.** Click **File** $\rightarrow$ **New** to open a blank script canvas.
    
- [ ] **3.** Copy and paste the following automation script into the top editor window
    

PowerShell

```
# Define the path to the CSV file and the default password security string
$CSVPath = "C:\NewHires.csv"
$SecurePassword = ConvertTo-SecureString "Welcome2026!" -AsPlainText -Force

# Import the CSV data file
$Users = Import-Csv -Path $CSVPath

# Loop through each row in the CSV file
foreach ($User in $Users) {
    
    # Dynamically build the target Distinguished Name (DN) path based on their department
    $TargetOU = "OU=$($User.Department),OU=Corporate,DC=workshop,DC=lab"
    
    # Define parameters for the new AD User
    $UserParams = @{
        GivenName = $User.FirstName
        Surname = $User.LastName
        Name = "$($User.FirstName) $($User.LastName)"
        SamAccountName = $User.Username
        UserPrincipalName = "$($User.Username)@workshop.lab"
        Path = $TargetOU
        AccountPassword = $SecurePassword
        ChangePasswordAtLogon = $true
        Enabled = $true
    }
    
    # Execute the account creation command
    Write-Host "Creating user account for: $($User.FirstName) $($User.LastName) in OU: $($User.Department)..." -ForegroundColor Cyan
    New-ADUser @UserParams
}

Write-Host "Automation Complete! All 50 users successfully provisioned." -ForegroundColor Green
```

- [ ] **4.** Click **File** $\rightarrow$ **Save As...** and save the script to your desktop as `CreateUsers.ps1`.
    

## 🚀 Phase 4: Executing the Automation Script

- [ ] **1.** In PowerShell ISE, press the green **Run Script** play button (or hit `F5`).
    
- [ ] **2.** Look at the console output pane at the bottom. You should see 50 bright cyan lines scrolling down indicating the creation of each user, followed by the green success message.
    
- [ ] **3.** Minimize PowerShell ISE and return to **Active Directory Users and Computers**.
    
- [ ] **4.** Click **Action** $\rightarrow$ **Refresh** from the top menu bar.
    
- [ ] **5.** Click through your `Sales`, `Marketing`, `IT`, `HR`, and `Finance` OUs to verify that all 50 employee accounts were successfully parsed and separated into their proper departments.
    

## ✅ Phase 5: Final Verification Check

Ensure the script configured the login security requirements correctly by logging in on a client machine.

- [ ] **1.** Go to **Client-01** or **Client-02** and switch to the Windows logon screen.
    
- [ ] **2.** Select **Other User** and log in using one of the new automated accounts:
    
    - **Username:** `jsmith` (or any username from the 50-user sheet)
        
    - **Password:** `Welcome2026!`
        
- [ ] **3.** Verify that Windows catches the domain policy and states: _"The user's password must be changed before signing in."_
    
- [ ] **4.** Change the password and verify completion