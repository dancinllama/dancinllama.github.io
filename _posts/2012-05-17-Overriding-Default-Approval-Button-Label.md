---
title: "Overriding Approval Button Label"
excerpt: >
  This post details how to override the approval button label in Salesforce.
categories:
  - Salesforce
tags:
  - Declarative
  - Approval
toc: false  
---

I had a recent need for override the label of the 'Submit for Approval' button, which is enabled by creating an approval process on an object out of the box in Salesforce.  Unfortunately, since the Submit for Approval is sort of generic across different objects, there is no way of overriding the label through Setup-&gt;Customize-&gt;Tab Names and Labels.

The solution is to implement a two line VF page and hook it up to it's own button, complete with the new label.  The VF page still uses standard out of the box functionality, and no apex was used nor harmed during this customization.  

The trick is to utilize the $Action global variable.  If you take a look at the Name of the Submit For Approval button on the page layout, you should find the name of the button/action is "Submit".  Going forward with this, I created the following simple Visualforce page:

```<apex:page standardController="Case" action="{!URLFOR($Action.Case.Submit,case.Id)}"></apex:page>
```

Note that utilizing the action parameter is considered a CSR security risk and will be flagged if you depend on packaging up the Visualforce page.

After the Visuaforce page is created, the next step is to create a custom detail page button, opening the page in existing window with sidebar.  Set the label on the button to the text you desire.  Finally, modify the page layout(s) and replace the 'Submit for Approval' button with your custom VF button.

There you have it, an easy-peasy way to "override the label" using (mostly) out-of-the-box functionality.
