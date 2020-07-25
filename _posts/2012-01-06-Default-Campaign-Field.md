---
title: "Adding a default value for Campaign Standard Field"
header:
  overlay_color: "#5e616c"
  overlay_image: /images/galaxy.jpg
excerpt: >
  In this post, we'll use a VF page to override defaults for Campaign fields
categories:
  - Salesforce
tags:
toc: false
---

Scenario: Without needing to enter the Campaign Name field on the edit Campaign screen, have a trigger define the Campaign Name field based on a formula like criteria.
Problem:  The Name field is required on the Campaign, and saving the page layout fires a field validation rule before the before update trigger runs.  Furthermore, there is no option to provide a default value for the Name field unlike custom text fields.
Solution: Feed a dummy data value into the Campaign name through a custom Visualforce solution.  The solution involves overriding the New button on the Campaign and directing the user to the Campaign edit page with the Campaign name pre-populated, allowing the campaign to save and fire the trigger.

- Create a Controller extension to prepopulate the Campaign Name:

```
public with sharing class CampaignExtension {
  public CampaignExtension(ApexPages.StandardController stdController) {}

  public PageReference gotoUrl(){
    PageReference pageRef = new PageReference('/701/e');
    Map params = pageRef.getParameters();
    params.put('nooverride','1');
    params.put('RecordType',recordType);
    //See notes below
    params.put('cpn1','Campaign Name');
    return pageRef;
  }
}
```

**ANote** The 'cpn1' parameter is related to whichever field to default.  Viewing the HTML source of the Campaign edit screen reveals that the HTML id of the Name field is 'cpn1', hence the cpn1 parameter for the Campaign Name field.
{: .notice--primary}
  
- Create a VF page:

```<apex:page standardController="Campaign" extensions="CampaignExtension" action="{!gotoUrl}"></apex:page>
```

- Override standard new button for Campaign:
  - Setup->Customize->Campaigns->Buttons and Links
  - Click Edit next the standard New button
  - Select the Visualforce Page radio button and select the Visualforce page from Step 2 above.
  - Validate that "Skip Record Type Selection Page" is unchecked. 
  - Save the override properties

Wa-la, you should have a mechanism to set the default value for a required standard field on a standard object.
