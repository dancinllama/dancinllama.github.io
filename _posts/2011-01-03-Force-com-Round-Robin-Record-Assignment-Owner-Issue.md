---
title: "Force.com Labs Round Robin Record Assignment - Case Owner issue"
categories:
  - Salesforce
tags:
  - Force.com Labs
  - Appexchange Labs
toc: false
---

There's an existing defect in the Salesforce package for Round Robin Record Assignment.  (http://appexchange.salesforce.com/reviews?listingId=a0N3000000178fsEAA).  

The Force.com lab unmanaged package assigns Cases to an owner/queue when available.  The package contains two triggers which assign the case to an owner; caseRoundRobin.trigger and caseOwnerUpdate.trigger.  The two triggers inadvertently fight with eachother.  

The fighting causes a Case to be re-assigned to its original owner when it is first reassigned ownership. Say you have a case w/ owner John, and you want to reassign it to owner Mary.  In the Salesforce UI, after you save the ownership, it reverts back to John on the first attempt.  A second attempt will get you to the right owner, however.  

The defect arises in caseOwnerUpdateTrigger.  After the code loops through the effected cases in the Trigger, via Trigger.new, it adds the cases to a temp list and then does a SOQL query to retrieve them once more via Id. The second loop does a query against uncommitted data and returns the yet unmodified owner of the Case, which would be John from the example above.  The case is then updated w/ the former Owner and appears the Case never changes hands.  

The following change to the trigger should clear things up, consolidating everything into one loop, with no 'dirty', or uncommitted reads.

Two notes here: 
1. Since the case in the loop is in process of being edited by the Trigger, it is locked and in read-only mode.  Therefore, a temp case is created and added to the update list.
2. The update call is made on the list of cases rather than on individual cases in the loop.  This is a Salesforce best practice, in order to mitigate SOQL queries and DML.

```
trigger caseOwnerUpdate on Case (after update) {

    List updateCS = new List();
    Map cases = new Map();
    
    for (Case cs : Trigger.new){
        if(Trigger.isUpdate) {  
            System.debug('&gt;&gt;&gt;&gt;&gt; Owner ID: '+cs.ownerId+' Temp Owner ID: '+cs.TempOwnerId__c);
            if(cs.TempOwnerId__c &lt;&gt; null &amp;&amp; cs.TempOwnerId__c &lt;&gt; '') {
                if(cs.OwnerId &lt;&gt; cs.TempOwnerId__c) {
                    Case temp = new Case();
                    temp.OwnerId = cs.TempOwnerId__c;
                    temp.TempOwnerID__c = 'SKIP';
                    updateCS.add(temp);
                }
            }           
        }   
    }
  
    System.debug('Update Cases: '+updateCS);
    
    //
    //Update last assignment for Assignment Group in batch
    //
    if (updateCS.size()&gt;0) {
        try {
            update updateCS;
        } catch (Exception e){

        }
    }
}
```
