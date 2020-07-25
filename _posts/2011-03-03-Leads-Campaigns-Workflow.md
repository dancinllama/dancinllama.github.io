---
title: "Leads, Campaigns, and Workflow"
excerpt: >
  Discussing standard, declaritive functionality around leads, campaigns, and workflow.
categories:
  - Salesforce
tags:
  - Declarative
toc: false
---

I have a requirement for a client that wishes to create a Web 2 Lead form and assign it to a campaign.

Once that is done, a workflow shall fire off a series of emails depending on what sort of campaign the lead is tied to.  This sounds easy enough to do with Salesforce's out of the box Web2Lead functionality and workflow, but there were some other tweaks we needed, so utilizing our own solution is the only way to go.

**The form**

Utilizing a jQuery modal, the user clicks a link and populates a small web to lead form.  When the user hits submit, sever side validation happens in a VF controller, so we can control the UI (messages and labels if a field is required for instance). 

**Allowing Portal users to use the web 2 lead form**

Once the form is submitted, it stores the lead in a custom lead object.  The custom lead object is needed, since leads cannot be created (owned) by Portal users.  When a portal user creates the mirrored lead object, it fires off a trigger calling an @future method to insert the lead and campaign member.

**Tricky workflows with Leads and Campaigns**

Here's the tricky part of the workflow.  When the lead is inserted, it doesn't have access to its related Campaign member and hence related Campaign (the Campaign member is a junction object to connect Campaigns to leads or contacts).  Our workflow needed to look at the Campaign Name field on the Campaign.  The lead doesn't have this when it is inserted, nor if the Campaign member is updated, does it update the lead.  Therefore, modifying the campaign member or campaign doesn't fire the workflow rule until the lead is updated next.  This is true even if the workflow rule is set to 'Created or did not previously meet the criteria'.

The solution is to create the workflow rule on the Campaign member and not the Lead.  When the Campaign Member junction is created, it should have access to both its Campaign and its Lead objects.  Thus, when the member is created, we can create a rule like 'Campaign Name equals My Name' and fire off a workflow when the Lead is assigned to the particular campaign.

Hope this helps others.
