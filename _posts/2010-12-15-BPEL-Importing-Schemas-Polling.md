---
title: "BPEL - Importing schemas for Polling orchestration"
categories:
  - Integration
tags:
  - Integration
  - BPEL
toc: false  
---

By default, BPEL orchestrations are started with a client partner link with two activities, one for input and one for output. When creating a polling orchestration, or an orchestration that polls from a database rather then waits for an http request, the first task is to remove the partner link and the two activities. Unfortunately, this renders the client WSDL useless.

Without the client WSDL, there's no other way to import additional schemas into the BPEL orchestration. Or is there? When you create the polling activity in BPEL, it generates an XSD and a WSDL file for the Polling partner link. When you need to add additional schemas to your BPEL process, then add the following into your Polling WSDL. (If the types element already exists, then you can add your schema import there).

![Schemas](/images/schemas.jpg)

After saving the WSDL, be sure to add the declaration of the schema and namespace to your bpel source. Then you should be able to reference the schema in your orchestration.
