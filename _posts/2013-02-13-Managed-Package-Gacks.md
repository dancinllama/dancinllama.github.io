---
title: "Managed Package Unknown Server Errors (Gacks)"
excerpt: >
  This posts discuss a few of the gacks you might encounter when developing / deploying managed packages
categories:
  - Salesforce
tags:
  - Development
  - Packaging
  - ISV
---

Creating a Salesforce managed package and get a non-descript "Salesforce has encountered an Unknown error "8675309" code?  These are called gacks in Salesforce land.  The only way to decode these errors is to contact Salesforce support, or someone at Salesforce who can look up the gack for you.

Through a bit of trial and error, here are the reasons I've encountered gacks while uploading packages

- Record Types or Business Processes have the same name (even though they are across different objects)  
  - The resolution for this is simple (hopefully) and involves renaming the record type or business process to be distinct, such as RecordTypeName_ObjectName
- Invalidated class.  
  - The result of perhaps only deploying a subset of classes to the packaging org.  I deployed a change to a VF controller, but removed a method its test class was dependent on.  After fixing the issue, the package was successfully uploaded.
- Permission Sets
  - Permission sets containing CRUD permissions on Standard objects fail with "Unknown error".  This is because Permission sets cannot by design update Contacts, Leads, Accounts, Opportunities, and other standard objects.  The fix for this issue is to modify the permission set to uncheck all permissions on standard objects, and update them manually when installed.

If you have any other gacks and resolutions to them you've come across, feel free to comment below, and I will update the list above.
