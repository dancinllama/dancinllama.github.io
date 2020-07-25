---
title: "Salesforce DX and Windows Paths: Issue with Spaces"
categories:
  - Salesforce
tags:
  - Salesforce DX
  - Windows
toc: false
---

Salesforce DX has an issue when it attempts to resolve paths with spaces.  This can happen in Windows environments, and reared its ugly head because the person that set up my work laptop added a space to the username.  When you issue a Salesforce DX command, you'll get an obscure error like C:\Users\User is not a valid command.  To account for this, you can:
- Issue the following in command prompt: FOR %d IN ("%LOCALAPPDATA%") DO SET LOCALAPPDATA=%~sd
- Beg your administrator to never ever use spaces in Windows usernames.
- Ask for a Mac instead.
