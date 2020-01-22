---
title: "How to Enable Receive Location fault tolerance  | Microsoft Docs"
description: How to Enable Receive Location Fault Tolerance.
author: "Pravakar Garnayak"
ms.author: "pravagar"
manager: "dougeby"
ms.date: "01/10/2020"
ms.topic: conceptual
ms.prod: biztalk-server

# optional metadata

#ROBOTS:

ms.reviewer: 
ms.suite:
ms.tgt_pltfrm:
ms.assetid: 
ms.custom: "biztalk-2020"

---
# How to Enable Receive Location Fault Tolerance

## Overview

 A receive location can be completely disabled by BizTalk, if BizTalk encounters more than a threshold number of errors due to adapter. If receive location is running in multi host environment and receive location encounters error in one host, for example, due to certificate expire error or disk full error, the receive location in other host will also stop running as the receive location will be completely disabled by the faulty host.

**Starting with BizTalk Server 2020 and newer**, administrators can configure receive locations to recover from transient errors instead of completely disabled by BizTalk. On error, receive locations will be disabled in the host instance where it is faulted and BizTalk will attempt to recover it in certain configurable interval. The receive location will continue running in other host instances.

## Configuring fault tolerance behavior

1. In the **BizTalk Server Administration** console, right-click the **BizTalk Group**, and select **Settings**.
2. In the **BizTalk Settings Dashboard** dialog box, select the **Group** page.

3. Check **Enable fault tolerance**.

4. Set **Retry interval** to desired number. By default, BizTalk attempts to recover receive locations from fault in 60 seconds interval.

5. Select **OK** to save your changes.
