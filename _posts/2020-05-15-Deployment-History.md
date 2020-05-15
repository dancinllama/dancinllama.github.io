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

(/images/changesetbeta.png)

**Author / Editor's Note** I thought I was clever when I came up for the title of this blog.  Obviously, it's a play on Stephen Hawking's title.  However, a quick google search on Continuous Integration and Continous Delivery lead me to this blog, which shares a very similar title: https://circleci.com/blog/drawn-out-conversation-featuring-circleci-principal-software-engineer-pat-shields/.  Great minds think alike?
{: .notice--primary}

## Introduction
I joined the Salesforce ecosystem in April of 2010.  The API version was API v18.  Change Sets were just announced as a Beta Feature, and deployment strategies were just a blip on the radar and sandboxes still had a new car smell.  Since then, the entire Salesforce platform has evolved.  

Today devops and deployment strategies are a concern, source control is getting better adoption, the Salesforce developer flow has even pivoted with the advent of Salesforce DX.  Let's take a look at how we got here, using a wider-than-just-Salesforce lens, and maybe get a glimpse of what's to come.

## 2001 - Build Servers
In 2001, a simple cron-based build server called Cruise Control was released.  CruiseControl allowed allowed DevOps teams (before they were dubbed DevOps) to run builds on a scheduled basis.  The builds, using ANT scripts, could build an application, deploy it to different environments, and even run unit tests.  CruiseControl would end up inspiring more robust Continuous Integration (CI) systems in the near future like Hudson / Jenkins and eventually lead to Continuous Delivery as well.

As a contractor at IBM, I worked on a product called Fix Central.  We maintained several System i machines, which ran a WebSphere application.  In order to support deployments and automated unit testing, I implemented CruiseControl and became an instant fan of Continuous Integration, which was in its infancy at this time. [[1]](#1)

## 2006 - Sandboxes

(/images/dontalwaystestmycode.jpg)

Speaking of environments, in the Winter 2006 release, Salesforce also saw a need to move development from production to a sandbox environment and introduced "Salesforce Sandbox".  Salesforce Sandbox allowed administrators to copy Production to a sandbox or test environment, and allowed them to test changes before promoting to production.  Although Salesforce was not on my radar in 2006, I can only imagine how painful those deployments must have been. 

Shortly after, in the Summer 2007 release, Salesforce then followed the announcement by adding multiple sandboxes.  Now you could have a true development, UAT, and production environment. Salesforce deployments were now cooking with fire.  With the addition of sandboxes, Salesforce transitioned from being a CRM tool to being a platform. [[2]](#2)

## 2008 - Rise of Continuous Integration
At the start of 2008, Continuous Integration was a thing, but it wasn't very popular.  DevOps really wasn't a term found on Job Search sites yet.  That was about to change with the push for Agile methodolgies, however.  Although it had been around for awhile, around 2008 is when Agile really caught steam, at least in my personal experience.  With agile development also came the need for development tools and Dev Ops teams.  In order to support this need, Continuous Integration tools like Rational Team Concert, Hudson / Jenkins evolved. [[3]](#3)

**Author / Editor's Note** My team at IBM decided to give Agile a shot around 2007-2008, and it was a disaster.  I remember having 30 people on one scrum 3 times a week.  The scrum meetings typically took 60 minutes if not 90 minutes.  Any certified scrum master will tell you to limit the "chickens and pigs" of the scrum calls to 10 or less, and to keep the scrum meeting to no more than 15 minutes.

During one of these 90 minute scrum meetings, I had spilled a bottle of Mountain Dew on my phone.  I put the phone on hold and ran to the restroom to grab some paper towels to clean up my mess.  When I get back from the office, I noticed that other folks that were on the scrum had hung up and left their offices too.  Come to find out that they heard a very high-pitched noise and then the conference call hung up.  So, I welcomed them for ending yet another absurdly long scrum call on their behalf.
{: .notice--primary}

## 2008 - Metadata API, Apex, Apex in "Appexchange Package"
That same year, during the Spring 2008 release notes, you'll find three important features that relate to Salesforce deployments.  In Spring 2008, Salesforce announced Apex, the ability to use Apex code in "Appexchange Packages", as well as the Metadata API.  The importance of Apex goes without saying, but the Metadata API would be used for many years to come as well.  In particular, the Metadata API is used by any Salesforce IDE out today, in addition to the Force.com Migration Toolkit as well as many internal and external Salesforce tools.

## 2010 - Change Sets are Beta
Until 2010, Salesforce deployments were very limited and typically involed an administrator asking a developer for help deploying metadata.  In fact, there were four options, and none of them were ideal:

1. Deploy through Eclipse.
2. Use an unmanaged package?
3. Write an ANT migration script, utilizing the ant-salesforce.jar and package.xml.
4. Redo whatever you had already done by hand.

Enter change sets.  Change sets allowed an administrator to create deployment connections between sandboxes and production, and then a (better than nothing) interface for selecting which pieces to deploy.  Change Sets are still in wide use today (2020), and are a good choice for one time deployments 

**Author / Editor's Note** We just won't talk about how the interface hasn't changed in the last decade, nor the many tears shed over having to delete a component that starts with "Z" from a 1,000 component change set
{: .notice--danger}

## 2013 - Tooling API and Developer Tools
Between 2008 and 2012, tooling around Salesforce development was very much stagnant.  Around late 2012, into 2013, we started seeing an improvement in the tooling, and it was in very large part due to Joe Ferrero's amazing work on MavensMate.  By this time, MavensMate had reached critical mass and was used by the majority of the Salesforce developer community. 

In the Spring 2013 release, Salesforce launched the Tooling API, which filled some gaps left behind by the Metadata API, primarily in areas like calculating code coverage, running unit tests, and other areas.

## 2017 - Salesforce DX Beta
Between 2013 and 2017, the progression of development and deployment tools was heavily driven by the community. Just some of the community led initiatives during that time include: 

- Additional MavensMate extensions for Atom and VS Code
- Andrew Fawcett and myself writing Apex wrappers for Metadata and Tooling API integrations. [[]]
- Patrick Connelly and Scot Floess developing Solenopsis, a command line tool for deployments [[]]
- Apex PMD for statically analyzing Apex code [[]]

...And then Dreamforce 2016 hit. [[]]

I remember walking around the [i]Developer Forest[/i] at Dreamforce that year, trying to complete my Trailhead Quest for yet another stuffed plushie, when I walked by the Salesforce DX booth.  I had been following Greg Wester for awhile on Twitter, but the Salesforce DX demo he gave really caught my attention. That was the first time I had heard of Salesforce DX.  "This is amazing, and it's about time Salesforce developers had this type of functionality in place," is a thought that crossed my mind over and over. The goal for Salesforce DX was to pivot Salesforce development in 180 degrees, from an org-centric to a source-central development model.  

What is org-centric development? It's when you start development right within a Salesforce org.  Source control is rarely even in a discussion with this model, unless you're at an enterprise level.  Development may even utilize features such as Salesforce CPQ, Communities, Financial Services Cloud, Analytics, or packages, which proves very difficult.  To make the switch from org-centric to source-centric thinking and development, Salesforce DX brought in a concept with which called scratch orgs.  For more information  on scratch orgs, read here: https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs.htm.  

Scratch orgs, along with amazing enhancements to the Salesforce CLI really boosted the productivity of Salesforce development and deploys.  

## 2017 - Enter Containers and Continuous Delivery
Around the same time that Salesforce DX was dropped on the community, Docker containers and pipelines became popular, especially in the context of devops and Continuous Delivery (CD) deployments. With the combination of docker and Salesforce DX, I could now:
1. Start a Salesforce project from Salesforce DX source.
2. Commit changes to that repostory
3. On commit:
  - Run unit tests
  - Lint any Javascript code, to check syntax errors
  - Use prettify to format that JS code and commit it right back to the repository
  - Automatically deploy that code to 





or who  you would develop something within Salesforce, maybe even in production, and then bring that function down to a Sandbox.  Rarely was source control ever in the picture.


In my opinion, Salesforce really switched from being a CRM system to a Platform back in 2

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


## References
<a id="1">[1]</a>https://en.wikipedia.org/wiki/CruiseControl
<a id="2">[2]</a>https://en.wikipedia.org/wiki/CruiseControl
<a id="3">[3]</a>https://en.wikipedia.org/wiki/Hudson_(software)

 https://github.com/solenopsis/Solenopsis
 https://pmd.github.io/latest/pmd_rules_apex.html
 https://developer.salesforce.com/dreamforce/2016-recap
 
 
<a id="1">[1]</a> Atlassian - What is DevOps?


Cheers, - James / @dancinLlama
