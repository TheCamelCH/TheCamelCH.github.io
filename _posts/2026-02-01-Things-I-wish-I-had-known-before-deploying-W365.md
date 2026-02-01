---
title: "Things I wish I had known before deploying W365"
date: 2026-02-01:09:00-00:00
categories:
  - blog
tags:
  - W365
  - Intune
  - Troubleshooting
---

Over the last few months, I've spent some time deploying a large number of W365 Cloud PCs. Now, in the production environment, I'm facing some challenges that I didn't encounter during testing. In this blog post, I'm gonna list some things that I wish I had known before. I hope this helps you avoid making the same mistakes I did 
 
### Reprovisioning = New client
 
When you use the cloud pc as a development environment, you need to reprovision the clients from time to time to get a fresh start. Well, reprovision means, thaty ou will get a brand new client. That means new hostname and new IP. So you need to push back these changes in things like Firewall, Asset Database, etc. Keep taht in mind, if that could affect you. As an alternative, you can use the restore points.

<span style="color:red">Learning: Check if the hostname and IP adress matter in your ecosystem</span>

### You cannot change the provisioning policy for existing Cloud PCs

In my environment, I have different provisioning policies for different use cases. Everything worked great until one day I got a request: a specific user group should be moved to a new Windows version. This should also apply when reprovisioning. Unfortunately, this user group was only a small part of an existing provisioning policy. So I can't change the entire policy to a new Windows image... and existing Cloud PCs can't be moved to a new policy. The only option left is to do the reprovisioning with the old image and perform the upgrade afterwards (which takes a lot of time). In our case, we modified the reprovisioning process so that the old client is removed and the new client is added to a new provisioning policy... also not great, because with that solution, you have some parts of the user groups in the old provisioning policy and some in the new...

<span style="color:red">Learning: Plan your provisioning policy architecture carefully!</span>

### Nameing template is limited



<span style="color:red">Learning: Check if W365 naming template matches your company hostname template. If not, find a compromise</span>

### No maintenance window

If you're using Citrix CVAD or AVD, you will be familiar with maintenance / drain mode. Clients in this mode cannot be accessed. There is no maintenance mode in W365. Therefore, if you are deploying updates or new software and want to ensure that users cannot cancel or interrupt the installation / update process, you need to prevent them from logging in to the Cloud PC. Best solution for us is to stop and disable the " PROZESS" process before the update and start it after the maintenance. This solution is not ideal, because your monitorin system could throw some alerts. But right now I haven't a better solution. If you have one, please let me know 

<span style="color:red">Learning: Check if user logon braeks your maintenance process. If yes, disbale the service and check the alerting</span>

### Check the available IP's!

<span style="color:red">Learning: Make sure you plan with enough IP addresses!</span>