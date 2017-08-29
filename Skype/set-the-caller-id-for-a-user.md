---
title: Set the Caller ID for a user
ms.assetid: c7323490-d9b7-421a-aa76-5bd485f80583
---


# Set the Caller ID for a user

Cloud PBX in Skype for Business Online provides a default caller ID that is the user's assigned telephone number. You can either change or block the caller ID (also called a Calling Line ID) for a user. You learn more about how to use caller ID in your organization by going  [How can caller ID be used in your organization](how-can-caller-id-be-used-in-your-organization.md).
  
    
    

There are settings that you can change:
> [!NOTE]
> This **is not** for on-premises organizations with Lync or Skype for Business Server.
  
    
    


- **Change their outgoing caller ID** You can replace a user's Caller ID, which by default is their telephone number, with another phone number. For example, you could change the user's Caller ID from their phone number to a main phone number for your business or change the user's Calling Line ID from their phone number to a main phone number for the legal department. You can change the Calling ID number to any Online **service** number (toll or toll-free).
    
    > [!NOTE]
      > If you want to use the  _Service_ parameter, you must specify a valid service number.
- **Block their outbound caller ID** You can block their outgoing Caller ID from being sent on a user's outgoing PSTN calls. Doing this will block their phone number from being displayed on the phone of a person being called.
    
  
- **Block their incoming caller ID** You can block the user from receiving Caller ID on any incoming PSTN calls.
    
  

> [!IMPORTANT]
> Emergency calls will always send the user's telephone number (caller ID). 
  
    
    

By default, all of these caller ID settings are **turned off**. This means that the Skype for Business Online user's phone number can be seen when that user makes a call to a PSTN phone.To learn more about these settings and how you can use them, go  [How can caller ID be used in your organization](how-can-caller-id-be-used-in-your-organization.md).
## Set your caller ID policy settings


> [!NOTE]
> For all of the callerID settings in Skype for Business Online you must use Windows PowerShell and you **can't use** the **Skype for Business admin center**. 
  
    
    


### Verify and start Windows PowerShell


- **Check that you are running Windows PowerShell version 3.0 or higher**
    
1. To verify that you are running version 3.0 or higher: **Start Menu** > **Windows PowerShell**.
    
  
2. Check the version by typing  _Get-Host_ in the **Windows PowerShell** window.
    
  
3. If you don't have version 3.0 or higher, you need to download and install updates to Windows PowerShell. See  [Windows Management Framework 4.0 ](https://go.microsoft.com/fwlink/?LinkId=716845) to download and update Windows PowerShell to version 4.0. Restart your computer when you are prompted.
    
  
4. You will also need to install the Windows PowerShell module for Skype for Business Online that enables you to create a remote Windows PowerShell session that connects to Skype for Business Online. This module, which is supported only on 64-bit computers, can be downloaded from the Microsoft Download Center at  [Windows PowerShell Module for Skype for Business Online](https://go.microsoft.com/fwlink/?LinkId=294688). Restart your computer if you are prompted.
    
  

    If you need to know more, see  [Connect to all Office 365 services in a single Windows PowerShell window](https://technet.microsoft.com/EN-US/library/dn568015.aspx).
    
  
- **Start a Windows PowerShell session**
    
1. From the **Start Menu** > **Windows PowerShell**.
    
  
2. In the **Windows PowerShell** window, connect to your Office 365 organization by running:
    
    > [!NOTE]
      > You only have to run the **Import-Module** command the first time you use the Skype for Business Online Windows PowerShell module.


  
    
    
> 
  ```
  
Import-Module "C:\\Program Files\\Common Files\\Skype for Business Online\\Modules\\SkypeOnlineConnector\\SkypeOnlineConnector.psd1"
  ```


  
    
    
> 
  ```
  $credential = Get-Credential
  ```


  
    
    
> 
  ```
  $session = New-CsOnlineSession -Credential $credential
  ```


  
    
    
> 
  ```
  Import-PSSession $session
  ```


    If you want more information about starting Windows PowerShell, see  [Connect to all Office 365 services in a single Windows PowerShell window](https://technet.microsoft.com/EN-US/library/dn568015.aspx) or [Connecting to Skype for Business Online by using Windows PowerShell](https://technet.microsoft.com/en-us/library/dn362795%28v=ocs.15%29.aspx).
    
  

### See all of the caller ID policy settings in your organization


- View all of the caller ID policy settings in your organization, run:
    
  - 
  ```
  
Get-CsCallingLineIdentity |fl
  ```


    See more examples and details for  [Get-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793856.aspx)
    
  

### Create a new caller ID policy for your organization


- Create a new caller ID policy that sets the caller ID to anonymous, run:
    
  - 
  ```
  
New-CsCallingLineIdentity  -Identity Anonymous -Description "Anonymous policy" -CallingIDSubstitute Anonymous -EnableUserOverride $false
  ```


    See more examples and details for  [New-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793855.aspx)
    
  
- Apply the new policy you created to Amos Marble, run:
    
  - 
  ```
  
 Grant-CsCallingLineIdentity -Identity "amos.marble@contoso.com" -PolicyName Anonymous
  ```


    See more on the  [Grant-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793857.aspx) cmdlet.
    
  
If you have already created a policy, you can use the  [Set-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793854.aspx) cmdlet to make changes to the existing policy, then use the [Grant-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793857.aspx) to apply the settings to your users.
  
    
    

### Set it so the incoming caller ID is blocked


- Block the incoming caller ID, run:
    
  - 
  ```
  
Set-CsCallingLineIdentity  -Identity "Block Incoming" -BlockIncomingPstnCallerID $true -EnableUserOverride $true
  ```


    See more examples and details for  [Set-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793854.aspx)
    
  
- Apply the policy setting you created to a user in your organization, run:
    
  - 
  ```
  
Grant-CsCallingLineIdentity -Identity "amos.marble@contoso.com" -PolicyName "Block Incoming"
  ```


    See more on the  [Grant-CsCallingLineIdentity](https://technet.microsoft.com/en-us/library/mt793857.aspx) cmdlet.
    
  

### Remove a caller ID policy

To remove a policy from your organization, run:
  
    
    

```

Remove-CsCallingLineIdentity -Identity "My Caller ID Policy"
```

To remove a policy from a user, run:
  
    
    



```

Grant-CsCallingLineIdentity -Identity "amos.marble@contoso.com" -PolicyName $null
```


## Want to know more about Windows PowerShell?


- When it comes to Windows PowerShell is all about managing users and what users are allowed or not allowed to do. With Windows PowerShell, you can manage Office 365 and Skype for Business Online using a single point of administration that can simplify your daily work, when you have multiple tasks to do. To get started with Windows PowerShell, see these topics:
    
  -  [An introduction to Windows PowerShell and Skype for Business Online](https://go.microsoft.com/fwlink/?LinkId=525039)
    
  
  -  [Six Reasons Why You Might Want to Use Windows PowerShell to Manage Office 365 ](https://go.microsoft.com/fwlink/?LinkId=525041)
    
  
- Windows PowerShell has many advantages in speed, simplicity, and productivity over only using the Office 365 admin center such as when you are making setting changes for many users at one time. Learn about these advantages in the following topics:
    
  -  [Best ways to manage Office 365 with Windows PowerShell](https://go.microsoft.com/fwlink/?LinkId=525142)
    
  
  -  [Using Windows PowerShell to manage Skype for Business Online](https://go.microsoft.com/fwlink/?LinkId=525453)
    
  
  -  [Using Windows PowerShell to do common Skype for Business Online management tasks](https://go.microsoft.com/fwlink/?LinkId=525038)
    
  

## Related Topics

 [Emergency calling terms and conditions](emergency-calling-terms-and-conditions.md)
  
    
    
 [Skype for Business Online PSTN services use terms](skype-for-business-online-pstn-services-use-terms.md)
  
    
    
