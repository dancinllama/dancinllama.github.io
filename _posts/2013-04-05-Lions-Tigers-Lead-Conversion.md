---
title: "Lions, Tigers, and Lead Conversion. Oh My!"
excerpt: >
  In this post, we'll discuss a core functionality of Salesforce: Lead conversion, and how we can make it work with a simple integration.
categories:
  - Salesforce
tags:
  - Integration
  - Lead Conversion
  - Jitterbit
---

For something as simple as clicking a button on a Lead to flip its status from "Open" to "Qualified", there sure are a lot of operations and complexity happening behind the scenes.  Converting a lead takes a lot of forethought.  For instance it must abide by Business processes associated with its Record Type. Fields must be mapped correctly for them to be carried over to the related Opportunity, Account, and Contact records.

What if you want to step outside the box with Lead conversion?  What if you want to implement your own lead button with a custom Visualforce UI?  What if you want to implement mass lead conversion through an integration or middleware?  The latter is where lead conversion gets really tricky, and that's the topic of this blog post.

I'm working on a rather large scale integration, which synchronizes both standard and custom objects from a source Salesforce org to another target Salesforce org.  The integration itself is a bit too complex to go with a standard Salesforce to Salesforce implementation.  One of the objects is Leads, and hence Converted Leads as well.

A few things I needed to consider were:
- How do I convert leads using an external id?
- How do I talk back to the source record, after a successful conversion, and prevent that record from being synchronized again?
- Can this be accomplished with a successful web service call or two?
- After toying around with a simple web service, it was quickly apparent that a single call really wouldn't suffice.  The leads in the integration are basically under a single account, and once you try and convert leads under a single account using logic similar to below, you get a "Duplicate Id in list exception".  Here's my post on Stackexchange asking for help on this issue: http://salesforce.stackexchange.com/questions/10520/best-approach-for-integrating-converted-leadswith-external-ids/10531?noredirect=1#10531



```List leadConversions = new List();
for(Lead l : leadsToConvert){
  Database.LeadConvert lc = new Database.LeadConvert();
  if(l.Contact__r != null){
    lc.setContactId(l.Contact__r.Id);
    lc.setAccountId(l.Contact__r.AccountId);
  }
  lc.setLeadId(l.Id);
  lc.setDoNotCreateOpportunity(true);
  lc.setConvertedStatus(status);
  leadConversions.add(lc);               
  leadExternalIds.add(l.External_Id__c);
}
      
List lcr = new List();
Integer i=0;
for(Database.LeadConvertResult r : Database.convertLead(leadConversions,false)){
  lcr.add(
    new LeadConversionResult(
      r.isSuccess(),
      r.getErrors().isEmpty() ? null : r.getErrors().get(0).getMessage(),
      leadExternalIds.get(i++)
    )
  );
}
```

After realizing this wouldn't work, it was time for a completely different approach.  Breaking out SoapUI (http://www.soapui.org/), a handy plugin for testing web service calls, and your best friend for testing out the Salesforce.com Enterprise and Partner WSDLs,  it was apparent that the api convertLead operation behaves differently than the apex version.  The convertLead operation does allow multiple leads under the same account!  Continuing the LOTR theme, the convertLead operation was my Samwise Gamgee to my integration Shelob (http://en.wikipedia.org/wiki/Shelob)

Taking the above into consideration I came up with the following solution:

When the source lead is converted, the external id is loaded into a custom object.  This allows the control successes and failures in the integration since leads cannot be updated after they are converted.
The integration queries the converted object queue above and passes the external ids to a webservice on the target org to retrieve their Salesforce Ids.  Since the external ids are Unique, and Case sensitive, it's a straight 1-1 mapping.
The ids are transformed into a convertLead request, which is a operation available on the partner wsdl.
Any successes are passed back to a third webservice call, which deletes successful conversions from the custom object "queue".  Errors are left in the queue for re-processing
The flow of the operation looks something like this:

![Jitterbit integration flow for converting leads](/images/jitterbitleadconversion.png)

There you have it, a solution for integrating converted leads.
