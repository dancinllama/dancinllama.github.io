---
title: "Salesforce.com Integrating Products and Pricebooks"
excerpt: >
  This post discusses how to integrate / use triggers with products and pricebooks.
categories:
  - Salesforce
tags:
  - Development
toc: false  
---

I've been working on a Oracle BPEL integration lately to upload Product2, PriceBook2, and PriceBookEntry objects into Salesforce. I didn't realize how complicated upserting a PriceBookEntry would be, however.

When upserting a PriceBookEntry, there are no external ids. So, before doing the upsert, you have to log-in to Salesforce and query the PriceBookEntry objects for a list of Ids according to the PriceBook2Id and the Product2Id. Ok simple enough.

Now what about a batch statement with up to 200 PriceBookEntry records at one time? Oy!

So, I took the existing XSLT transformation for a single Id lookup, and converted it to lookup 200 Ids. The query is a little nuts, and I'm hoping that it will come back in time, as SOQL has a strict time limit on returning queries. The query resembles the following:

Select Id,PriceBook2Id,Product2Id From PriceBookEntry where (PriceBook2Id = 'Id1' and Product2='Id2') or (PriceBook2Id = 'Id2' and Product2='Id2') .. etc, up to 200 times.

Now once you get the Id's back, you have to transform them into a Salesforce upsert request. The transform matches the Ids to the PriceBook2 and Product2 Ids and the values passed in through the integration. That part was equally as tricky, and involved some XSLT for-each statements and some recursive xslt-template calls.

Next, I got an error stating something similar to 'Cannot insert Product2 Pricebook without a Standard Price'. Having not dealt with PriceBooks before, I scratched my head on this one. Turns out, the Product2 object needs to be added to the Standard PriceBook before it can be added any other PriceBooks, via the PriceBook entry lookup object.

Here's a nifty little trigger to add a Product2 to the standard PriceBook2 object:

```
trigger InsertProductIntoStandardPB on Product2 (after insert) {
  //Insert the product(s) into the standard price book as a pricebookentry
  sObject s = [select ID from Pricebook2 where IsStandard = TRUE];

  List entries = new List();
  for(Product2 product : Trigger.new) {      
    entries.add(
      new PricebookEntry(
        Pricebook2Id=s.ID,
        Product2Id=product.ID, 
        UnitPrice=0.00, 
        IsActive=product.IsActive, 
        UseStandardPrice=FALSE
      )
    );
  }
  insert entries;
}
```

**Note: It's best practice, in Salesforce, to put your objects into a list before performing DML operations. It reduces the amount of SOQL calls drastically, especially, such as in this case, if 200 Products are inserted as part of a batch operation.
{: .notice--primary}
