---
title: "Passing null values for xml elements"
categories:
  - Integration
tags:
  - XML
toc: false  
---
I've been working on a Salesforce webservice class to create cases. After generating the wsdl, I ran into a couple issues.

First issue: The order of the child elements in the method's parent need to match the webservice method you're calling. Found this out after changing some elements around and getting 'invalid format for xsd:dateTime' when it was a string or an Integer instead. This also means you need to specify all the elements as well. Which leaves me to my next issue.

Second issue: I kept receiving 'invalid type '' for xsd:dateTime when attempting to pass nulls into the element. ,, all return the same error. Instead, you have to declare the schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance and include xsi:nil="true" in your element. After I changed all my optional dateTime elements to , I was able to invoke my web service.

Now, why I have to include a separate schema just to include null values in my xml is beyond me.
