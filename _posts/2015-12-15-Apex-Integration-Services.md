---
title: "Introducing the newest Trailhead Peak: The Apex Integration Services module"
excerpt: >
  Introducing the newest Trailhead Peak: The Apex Integration Services module
categories:
  - Salesforce
tags:
  - Salesforce
  - Trailhead
  - Integration
toc: false
---

As an integration architect, I spend a substantial amount of time with clients discussing how to integrate with their systems external to Salesforce.  Most times, fortunately, clients have a pre-existing system with a set of available APIs that are exposed either as RESTful or SOAP APIs.

A prime example of this, is the work I helped with on a hi-fidelity music service.  In order to get licensing information, track and album details, and other information, we made several RESTful calls out to a music content provider.  Additionally, we used SOAP services to handle payment processing, tax calculations, and address verification.

In order to work with these external systems, Salesforce provides mechanisms like WSDL2Apex, JSON and HttpRequest utilities that developers can utilize in order to interact with external systems.  If you're not familiar with how this works yet, I strongly urge you to check out the newest Trailhead module "Apex Integration Services".

The new module, which can be found here: https://developer.salesforce.com/trailhead/module/apex_integration_services, contains a lot of useful information on how to develop and test callouts to both REST and SOAP services.  The latter part is key, because the module requires you to use mock callout classes in order to unit test your callouts.  In days past, developers would skip over their callout unit tests using the dreaded if check of !Test.isRunningTest().  However, I'm glad that the module teaches new developers on how to properly test their callouts.

I have to be honest here though, and ask that when you walk through the challenges for the Apex Integration Services module, that you take a bit of heed.  For instance, you might notice that the check for your REST challenge might fail if you don't use this exact line when constructing the HTTP instance:


```Http http = new Http();```

One boards poster found the following line caused in issue with the rules engine when checking their challenge:

```Http h = new Http();```

Also, when you start the SOAP services module, be very careful on how you import the WSDL file.  After you click the "Import WSDL" button, you need to rename the Apex class from the defaulted "parksServices" text to "ParkService".  The case sensitivity along with the correct name is vital to ensuring your challenge is completed successfully.

With those caveats in mind though, the instructions are laid out well and once you pass the Apex Integration Services, you'll have a better understanding of how to integrate Salesforce with external systems.

Happy Trails!
