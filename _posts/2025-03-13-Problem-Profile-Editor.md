---
title: Problem Profile Editor
description: Introduction and overview to a simple tool for editing tricky, hard-to-reach profiles.
# date: 2025-03-15 14:00:00 -0500
categories: [Salesforce, LWC]
tags: [flow, problem profile editor]     # TAG names should always be lowercase
pin: true
# image:
#   path: /assets/img/automation-add-on/teaser.png
#   alt: Example view of the Automation Add-On components.
---

🆕 v0.2 released 2025-APR-12: [Support inaccessible profiles!]({% link _posts/2025-04-12-Problem-Profile-Editor-v0.2.md %})

📢 [Jump to Installation Info](#installation-info)

Have you ever tried remove profile access to a default record type, either because of an accidental assignment, org clean up, or to uninstall a package, only to get stopped in your tracks when pesky profiles like the Site Guest Users, Chatter Users or Platform License Users (and more!) have access?  Or even trying to clean up field access as part of security/compliance checks and these same profiles are a problem?

If you haven't, I'm sorry to tell you that it's not so easy.

If you have, you know that the workaround is to disable some things and then juggle URL parameters of other (working) profiles, and then remember to go back to re-enable those things. And if you're like me, you often forget this or find it really annoying.

> [Salesforce Help](https://help.salesforce.com/s/articleView?id=000385230&type=1) <i class="fa-brands fa-salesforce fa-lg" style="color: #74C0FC;"></i> tells us...
{: .prompt-info }

1. Disable the "Enhanced Profile User Interface" within the User Management Settings of Setup
1. (Recommended) Switch to the Classic interface
1. Copy the link from the error message in order to get the 15-character Id (starting with `00e`) for the offending profile and put it into a text editor
1. Go to a working Profile, like the System Administrator, and get to the editing page for the Record Type Settings for the object needed
1. Copy this URL, paste it into a text editor, find the URL parameter for the `id=` and swap out the working profile id with the offending profile id copied earlier
1. Use this modified URL in the browser to access the editing page in order to make your changes and save them
1. Re-enable the "Enhanced Profile User Interface" and go back to the Lightning interface.

## Introducing Problem Profile Editor

With this simple tool, you can now do this in a few quick steps without having to remember how to do all these steps or having to copy/paste your way to a resolution.

![Quick and Simple](/assets/img/problem-profile-editor/demo.png)

1. Just go to the "Problem Profile Editor" tab from the App Launcher
1. Search/select the Profile from the first input
1. Search/select the Object from the second input
1. Click the action button(s) for what needs to be resolved
   1. Record Type Settings
   1. Field-Level Security
   1. Classic Profile Editor
1. Make the changes on the new tab that opens directly to the Classic interface and click save

No more hassle.  You can even return to the tab and change Profiles or Objects and repeat the process as needed.

## Setup

Just install the package!

Want to explore the code? It's an 🔓📦 unlocked package and the repository is open-source too!

<i class="fab fa-github"></i> [/tschug/ProblemProfiles/](https://github.com/tschug/ProblemProfiles)

## Installation Info

v0.2 - 2025-APR-12
Sandbox & Scratch Orgs
: [https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC](https://test.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC)

Production & Developer Edition Orgs
: [https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC)

Old Versions (Production-only)
[v0.1 - 2025-MAR-13](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t3x000001ZqqqAAC)

## Uninstall Steps

If you followed the steps we've outlined here, it should be simple:

1. Navigate to Setup
1. In the Quick Find, search for `Installed Packages`
1. Click `Uninstall` next to the "Problem Profile Editor" package (fydo)
1. Follow the steps

> **Did you know?**
>
> With unlocked packages, you can remove most metadata components from the package by clicking `Remove` which will keep them in your Salesforce org, even though you're uninstalling the package!
>
> Watch out, Salesforce says there are [some things to consider](https://developer.salesforce.com/docs/atlas.en-us.pkg2_dev.meta/pkg2_dev/sfdx_dev_dev2gp_md_removal_considerations.htm) <i class="fa-brands fa-salesforce fa-lg" style="color: #74C0FC;"></i>.
{: .prompt-tip }