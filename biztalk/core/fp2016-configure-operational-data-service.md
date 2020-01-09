---
title: "Configure Operational Data Service in BizTalk Server 2016 Feature Pack | Microsoft Docs"
description: Configure Operational Data Service in BizTalk Server 2016 Feature Pack
author: "Elvis-Shi"
ms.author: "elsh"
manager: "dougeby"
ms.date: "1/7/2019"
ms.topic: conceptual
ms.prod: biztalk-server

# optional metadata

#ROBOTS:

ms.reviewer: 
ms.suite:
ms.tgt_pltfrm:
ms.assetid: 
ms.custom: "biztalk-2016fp"

---
## Configure Operational Data Service in BizTalk Server 2016 Feature Pack

1. Run Windows PowerShell as Administrator (**Start** menu, type **PowerShell**, right click, and select **Run as administrator**).
2. Go to the BizTalk installation folder (for example, type: `cd 'C:\Program Files (x86)\Microsoft BizTalk Server 2016\'`).
3. In the following text, replace `Default Web Site`, `operationalDataServiceAppPool`, `domain\user`, `password`, and `domain\group` with your values:

    ```Powershell
    FeaturePack.ConfigureServices.ps1 -Service operationaldata -WebSiteName '<Default Web Site>' -ApplicationPool <operationalDataServiceAppPool> -ApplicationPoolUser <domain>\<user\> -ApplicationPoolUserPassword <password> -AuthorizationRoles '<domain>\<group1\>, <domain>\<group2\>, <domain>\<user\>, <domain>\<user2\>'
    ```

   * **Service**: The service to be configured (**OperationalData** for Power BI)
   * **WebSiteName**: The existing IIS web site that hosts the service. The default value is **Default Web Site**.
   * **ApplicationPool**: The Application Pool used by the service. If it exists, a new one is not created. The default value is **DefaultAppPool**.
   * **ApplicationPoolUser**: Configures the application pool to run as this user identity. Must have BizTalk Server Operator, or higher privileges.
   * **ApplicationPoolUserPassword**: Password for the ApplicationPoolUser
   * **AuthorizationAccount**: List of authorized Groups or Users that can use this service

     In the following example, we use the `Default Web Site`, create an application pool named `PowerBIAppPool`, run the appPool as the `bootcampbts2016\btsservice` account, use `BIZTALK-serviceacct` as the user account password, and give the `BizTalk Server Administrators` group permissions. Be sure to enter the following, including the single quotes surrounding values with spaces:

     ```Powershell
     FeaturePack.ConfigureServices.ps1 -Service operationaldata -WebSiteName 'Default Web Site' -ApplicationPool PowerBIAppPool -ApplicationPoolUser bootcampbts2016\btsservice -ApplicationPoolUserPassword  BIZTALK-serviceacct -AuthorizationRoles 'BOOTCAMPBTS2016\BizTalk Server Administrators'
     ```

     When complete, the BizTalkOperationalDataService application is created within IIS:
     ![BizTalkMOperationalDataServer application](../core/media/biztalkmanagementservice-apppool.png)


4. To confirm it’s working, browse to `http://localhost/BizTalkOperationalDataService`.

    If you are prompted to sign-in, sign in with an account that is member of the domain\group you entered in the previous step (`-AuthorizationRoles 'BOOTCAMPBTS2016\BizTalk Server Administrators'`).

    If you are prompted to open or save BizTalkOperationalDataService.json, then your install completed. You can save it locally, and then open it in notepad or Visual Studio to see the contents.

> [!WARNING]
> The BizTalkOperationalDataService application in IIS uses a web.config file. Elements within web.config **are case sensitive**. So when you execute the Windows PowerShell script, be sure to enter the correct case for `-AuthorizationRoles` value. If you’re not sure of the case, here’s an easy way to find out:
>
> 1. Open **Computer Management**, and expand **Local Users and Groups**.
> 2. Select **Groups**, and scroll down to the **SQLServer…** groups.
> 3. In the following example, notice **BOOTCAMPBTS2016** is in all caps. If you see all caps, then enter the computer name in all caps.
>
> ![Computer name is in all CAPS](../core/media/groups-case.png)

## See also
[More about Operational Data Service](operational-data-service.md)