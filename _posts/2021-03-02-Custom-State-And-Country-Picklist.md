---
title: "Creating a custom, dependent state and country picklist."
excerpt: >
  Have you needed to use the standard state and country picklists that Salesforce provides?  Did you need this to reside outside of standard address functionality?  Read on for more info.
categories:
  - Salesforce
tags:
  - Development
  - Declaritive
toc: false  
---

Salesforce provides us with a standard state and country picklist that is useful, but only in a limited context.  The state and country picklists, are at the moment, unavailable outside of standard objects / standard address fields.  So how do you mimmick this functionality for custom fields?

I've created a simple Apex app that you can use at the following repository: <a href="https://github.com/dancinllama/StateAndCountryPicklistGenerator" target="_new">https://github.com/dancinllama/StateAndCountryPicklistGenerator</a>

The "app" consists of two Apex classes and allows you to copy the standard state and country picklists into a global value set and custom fields of your choosing.

The app uses the Metadata API and Apex Describes to copy not only the names, and values, but also the dependencies between states and countries.

There's  one small gotcha though:  Instead of the typical state value (like MN for Minnesota), the values will be prefixed with the country  (e.g. US_MN for United States / Minnesota).  This is because of a unique constraint with the picklist, in that it cannot have multiple entries with the same value. (Same states across different countries have same value).

Enjoy!
- James Loghry / dancinllama

