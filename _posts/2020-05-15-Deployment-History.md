---
title: "A Brief History in Salesforce Deployments"
header:
  overlay_color: "#5e616c"
  overlay_image: /images/galaxy.jpg
excerpt: >
  In this post, titled "A Brief History in Salesforce Deployments", I discuss the evolution of deploying metadata between environments in Salesforce, and provide insight on tools and products that you can start using today
categories:
  - Salesforce
  - Deployment
tags:
  - Deployment
  - Continuous Integration
  - Continuous Delivery
  - Circle CI
  - Github Actions
  - Travis CI
  - Bitbucket Pipelines
  - Pipelines
  - Salesforce DX
  - Change Sets
  - Force.com ANT Migration Toolkit
toc: true  
---

(/images/changesetsbeta.png)

**Author / Editor's Note** I thought I was clever when I came up for the title of this blog.  Obviously, it's a play on Stephen Hawking's title.  However, a quick google search on Continuous Integration and Continous Delivery lead me to this blog, which shares a very similar title: https://circleci.com/blog/drawn-out-conversation-featuring-circleci-principal-software-engineer-pat-shields/.  That being said, I stand by the title and am sticking to it.
{: .notice--primary}

## Introduction
I often find questions on Twitter or Slack or the Success Community asking questions around deployment strategies.  The person asking the question is often looking for a solution or examples of a successful deployment strategy used in the same or a similar vertical.  However, for this blog post I'm going to step back from this question and really dive into the evolution of deployments as they relate to Salesforce development, while providing some insights into the various solutions in the market place today.  


## My Salesforce Story

Before we dive into the history of Salesforce deployments, I'll provide some background on my personal Salesforce journey.  As of 2010, I was working as a contractor for IBM.  Agile was really catching on fire. I was focused on a product called "Fix Central", and I had created a build system for the product utilizing a rudimentary CI or Continuous Integration server called Cruise Control.  

At the time, IBM had been pushing its Rational products hard, including it's own CI solution with Rational Team Concert. However, much like everything else at IBM, Rational Team Concert was a pain to setup, configure, and use.  Hence, I rolled with Cruise Control.  Cruise Control was nothing more than a glorified cron job that ran ANT scripts on a scheduled basis.  It was simple, and it did it's thing well.

Around the same time, my office mate and mentor and started working for a consulting partner on a product called Salesforce.  After my pay was cut at IBM, I decided to join my mentor at EDL Consulting (and CloudCraze).  I had no clue what Salesforce was at this time, but was told that I could pick it up easily, since I had a Java and J2EE background.  

## API Version 18 - Change Sets Go Beta
I first joined the Salesforce ecosystem back in 2010.  The API version was 18.0, and one of the popular issues of that release cycle?  Change Sets are Beta. As consultants, we preached about completing work in Sandboxes and promoting "up the stack" until finally deploying functionality to production.  I didn't realize it at first, but deployments were a huge pain point for Administrators at the time.

Back then, there were four main ways of deploying from a Salesforce sandbox to a Salesforce production org:

1. Deploy through Eclipse.
2. Use an unmanaged package
3. Write an ANT migration script, utilizing the ant-salesforce.jar and package.xml.
4. Redo whatever you had already done by hand.

None of these options were very friendly for administrators.  Hence, change sets were a huge deal when they were first announced, and really continue to be an integral functionality today, regardless of how much you loathe the user interface.

## Building Deployment Confidence
Change Sets were and continue to be a decent, popular mechanism for one off deployments.  What happens though, when you're working through a long term project or you need to keep your Salesforce organizations in synch, and organized? You need a deployment strategy,  and in particular a repetitive deployment strategy in order to build confidence in your application.  

In 2010, Salesforce's support for this was minimal.  As a consultant, I worked on several projects, and at the time, not a single one of the team's I worked on had a deployment strategy or tools to help.  In fact, only a handful of peers in the community were utilizing Continuous Integration, and even less utilzed any sort of source control system, aside from  their own PC or Mac. Of those few peers, they used the Metadata API, and more specifically the Force.com ANT Migration Toolkit.
 
Shortly after 2010, the Salesforce community started seeing some improvement in the development tooling.  The real push for tooling improvement came from Joe Ferrero, creator of MavensMate.  We started seeing improvements to the Metadata API to support community-led products like MavensMate.  Eventually, in 2013, we saw the creation of the Tooling API, which helped address some gaps associated with the Metadata API.

## Salesforce Tooling and Salesforce DX

## Third Party Products

## Continuous Delivery

## What Should We Focus On?

## Release Notes
Most of the research I've put into this blog, was done by cultivating past release notes.  For a bit of nostalgia, you can dig through past release notes yourself!  This post on Salesforce Stack Exchange contains links to all (or at least most) past release notes: https://salesforce.stackexchange.com/questions/40411/where-are-all-past-salesforce-release-notes.  Some release notes date all the way back to 2004. 


Cheers, - James / @dancinLlama
