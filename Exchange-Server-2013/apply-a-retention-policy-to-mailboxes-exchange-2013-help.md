﻿---
title: 'Apply a retention policy to mailboxes: Exchange 2013 Help'
TOCTitle: Apply a retention policy to mailboxes
ms:assetid: 6ccc80db-d201-44f7-8d4b-473a89c14b2f
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Dd298052(v=EXCHG.150)
ms:contentKeyID: 49318580
ms.date: 12/10/2017
mtps_version: v=EXCHG.150
---

# Apply a retention policy to mailboxes

 

_**Applies to:** Exchange Online, Exchange Server 2013_


You can use retention policies to group one or more retention tags and apply them to mailboxes to enforce message retention settings. A mailbox can't have more than one retention policy.


> [!WARNING]
> Messages are expired based on settings defined in the retention tags linked to the policy. These settings include actions such moving messages to the archive or permanently deleting them. Before applying a retention policy to one or more mailboxes, we recommended that you test the policy and inspect each retention tag associated with it.



For additional management tasks related to messaging records management (MRM), see [Messaging Records Management Procedures](messaging-records-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

  - Estimated time to complete: 5 minutes.

  - You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Applying retention policies" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

  - For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-exchange-online-protection-help.md).


> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at <A href="https://go.microsoft.com/fwlink/p/?linkid=60612">Exchange Server</A>, <A href="https://go.microsoft.com/fwlink/p/?linkid=267542">Exchange Online</A>, or <A href="https://go.microsoft.com/fwlink/p/?linkid=285351">Exchange Online Protection</A>.



## What do you want to do?

## Use the EAC to apply a retention policy to a single mailbox

1.  Navigate to **Recipients** \> **Mailboxes**.

2.  In the list view, select the mailbox to which you want to apply the retention policy, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3.  In **User Mailbox**, click **Mailbox features**.

4.  In the **Retention policy** list, select the policy you want to apply to the mailbox, and then click **Save**.

## Use the EAC to apply a retention policy to multiple mailboxes

1.  Navigate to **Recipients** \> **Mailboxes**.

2.  In the list view, use the Shift or Ctrl keys to select multiple mailboxes.

3.  In the details pane, click **More options**.

4.  Under **Retention Policy**, click **Update**.

5.  In **Bulk Assign Retention Policy**, select the retention policy you want to apply to the mailboxes, and then click **Save**.

## Use the Shell to apply a retention policy to a single mailbox

This example applies the retention policy RP-Finance to Morris's mailbox.

    Set-Mailbox "Morris" -RetentionPolicy "RP-Finance"

For detailed syntax and parameter information, see [Set-Mailbox](https://technet.microsoft.com/en-us/library/bb123981\(v=exchg.150\)).

## Use the Shell to apply a retention policy to multiple mailboxes

This example applies the new retention policy New-Retention-Policy to all mailboxes that have the old policy Old-Retention-Policy.

    $OldPolicy={Get-RetentionPolicy "Old-Retention-Policy"}.distinguishedName
    Get-Mailbox -Filter {RetentionPolicy -eq $OldPolicy} -Resultsize Unlimited | Set-Mailbox -RetentionPolicy "New-Retention-Policy"

This example applies the retention policy RetentionPolicy-Corp to all mailboxes in the Exchange organization.

    Get-Mailbox -ResultSize unlimited | Set-Mailbox -RetentionPolicy "RetentionPolicy-Corp"

This example applies the retention policy RetentionPolicy-Finance to all mailboxes in the Finance organizational unit.

    Get-Mailbox -OrganizationalUnit "Finance" -ResultSize Unlimited | Set-Mailbox -RetentionPolicy "RetentionPolicy-Finance"

For detailed syntax and parameter information, see [Get-Mailbox](https://technet.microsoft.com/en-us/library/bb123685\(v=exchg.150\)) and [Set-Mailbox](https://technet.microsoft.com/en-us/library/bb123981\(v=exchg.150\)).

## How do you know this worked?

To verify that you have applied the retention policy, run the [Get-Mailbox](https://technet.microsoft.com/en-us/library/bb123685\(v=exchg.150\)) cmdlet to retrieve the retention policy for the mailbox or mailboxes.

This example retrieves the retention policy for Morris’s mailbox.

    Get-Mailbox Morris | Select RetentionPolicy

This command retrieves all mailboxes that have the retention policy RP-Finance applied.

    Get-Mailbox -ResultSize unlimited | Where-Object {$_.RetentionPolicy -eq "RP-Finance"} | Format-Table Name,RetentionPolicy -Auto
