---
title: "A Brief History in Salesforce DevOps"
header:
  overlay_color: "#5e616c"
  overlay_image: /images/galaxy.jpg
excerpt: >
  In this post, titled "A Brief History in Salesforce Devops", I discuss the evolution of deploying metadata between environments in Salesforce, and provide insight on tools and products used today
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
  - Gearset
  - Copado
  - Flosum
  - Blue Canvas
toc: true  
---

**Author / Editor's Note** I thought I was clever when I came up for the title of this blog.  Obviously, it's a play on Stephen Hawking's title.  However, a quick google search on Continuous Integration and Continous Delivery lead me to this blog, which shares a very similar title: https://circleci.com/blog/drawn-out-conversation-featuring-circleci-principal-software-engineer-pat-shields/.  Great minds think alike?
{: .notice--primary}

## Introduction
I joined the Salesforce ecosystem in April of 2010.  The API version was API v18.  Change Sets were just announced as a Beta Feature, and deployment strategies were just a blip on the radar and sandboxes still had a new car smell.  Since then, the entire Salesforce platform has evolved.  

Today devops and deployment strategies are a concern, source control is getting better adoption, the Salesforce developer flow has even pivoted with the advent of Salesforce DX.  Let's take a look at how we got here, using a wider-than-just-Salesforce lens, and maybe get a glimpse of what's to come.  Join me as I walk through a timeline of Salesforce Release Notes [[1]](#1) and share my thoughts on related DevOps tools.

![Timeline](/images/timeline.png)

## 2001 - Build Servers
In 2001, a simple cron-based build server called Cruise Control was released.  CruiseControl allowed allowed DevOps teams (before they were dubbed DevOps) to run builds on a scheduled basis.  The builds, using ANT scripts, could build an application, deploy it to different environments, and even run unit tests.  CruiseControl would end up inspiring more robust Continuous Integration (CI) systems in the near future like Hudson / Jenkins and eventually lead to Continuous Delivery as well.

As a contractor at IBM, I worked on a product called Fix Central.  We maintained several System i machines, which ran a WebSphere application.  In order to support deployments and automated unit testing, I implemented CruiseControl and became an instant fan of Continuous Integration, which was in its infancy at this time. [[2]](#2)

## 2006 - Sandboxes

![Dont Test Code In Production](/images/dontalwaystestmycode.jpg)

Speaking of environments, in the Winter 2006 release, Salesforce also saw a need to move development from production to a sandbox environment and introduced "Salesforce Sandbox".  Salesforce Sandbox allowed administrators to copy Production to a sandbox or test environment, and allowed them to test changes before promoting to production.  Although Salesforce was not on my radar in 2006, I can only imagine how painful those deployments must have been. 

Shortly after, in the Summer 2007 release, Salesforce then followed the announcement by adding multiple sandboxes.  Now you could have a true development, UAT, and production environment. Salesforce deployments were now cooking with fire.  With the addition of sandboxes, Salesforce transitioned from being a CRM tool to being a platform. [[3]](#3)

## 2008 - Rise of Continuous Integration
At the start of 2008, Continuous Integration was a thing, but it wasn't very popular.  DevOps really wasn't a term found on Job Search sites yet.  That was about to change with the push for Agile methodolgies, however.  Although it had been around for awhile, around 2008 is when Agile really caught steam, at least in my personal experience.  With agile development also came the need for development tools and Dev Ops teams.  In order to support this need, Continuous Integration tools like Rational Team Concert, Hudson / Jenkins evolved. [[4]](#4)

**Author / Editor's Note** My team at IBM decided to give Agile a shot around 2007-2008, and it was a disaster.  I remember having 30 people on one scrum 3 times a week.  The scrum meetings typically took 60 minutes if not 90 minutes.  Any certified scrum master will tell you to limit the "chickens and pigs" of the scrum calls to 10 or less, and to keep the scrum meeting to no more than 15 minutes.  During one of these 90 minute scrum meetings, I had spilled a bottle of Mountain Dew on my phone.  I put the phone on hold and ran to the restroom to grab some paper towels to clean up my mess.  When I get back from the office, I noticed that other folks that were on the scrum had hung up and left their offices too.  Come to find out that they heard a very high-pitched noise and then the conference call hung up.  So, I welcomed them for ending yet another absurdly long scrum call on their behalf.
{: .notice--primary}

## 2008 - Metadata API, Apex, Apex in "Appexchange Package"
That same year, during the Spring 2008 release notes, you'll find three important features that relate to Salesforce deployments.  In Spring 2008, Salesforce announced Apex, the ability to use Apex code in "Appexchange Packages", as well as the Metadata API.  The importance of Apex goes without saying, but the Metadata API would be used for many years to come as well.  In particular, the Metadata API is used by any Salesforce IDE out today, in addition to the Force.com Migration Toolkit as well as many internal and external Salesforce tools. [[5]](#5)

## 2010 - Change Sets are Beta
Until 2010, Salesforce deployments were very limited and typically involed an administrator asking a developer for help deploying metadata.  In fact, there were four options, and none of them were ideal:

1. Deploy through Eclipse.
2. Use an unmanaged package?
3. Write an ANT migration script, utilizing the ant-salesforce.jar and package.xml.
4. Redo whatever you had already done by hand.

Enter change sets.  Change sets allowed an administrator to create deployment connections between sandboxes and production, and then a (better than nothing) interface for selecting which pieces to deploy.  Change Sets are still in wide use today (2020), and are a good choice for one time deployments. [[6]](#6)

**Author / Editor's Note** We won't talk about how the interface hasn't changed in the last decade, nor the many tears shed over having to delete a component that starts with "Z" from a 1,000 component change set
{: .notice--danger}

## 2013 - Tooling API and Developer Tools
Between 2008 and 2012, tooling around Salesforce development was very much stagnant.  Around late 2012, into 2013, we started seeing an improvement in the tooling, and it was in very large part due to Joe Ferrero's amazing work on MavensMate.  By this time, MavensMate had reached critical mass and was used by the majority of the Salesforce developer community. 

In the Spring 2013 release, Salesforce launched the Tooling API, which filled some gaps left behind by the Metadata API, primarily in areas like calculating code coverage, running unit tests, and other areas. [[7]](#7)

## 2017 - Salesforce DX Beta
Between 2013 and 2017, the progression of development and deployment tools was heavily driven by the community. Just some of the community led initiatives include: 

- Additional MavensMate extensions for Atom and VS Code
- Patrick Connelly and Scot Floess developing Solenopsis, a command line tool for deployments [[8]](#8)
- Apex PMD for statically analyzing Apex code [[9]](#9)
- Heroku apps like Package.xml builder, Perm Comparator, Organization Doctor, and more. [[10]](#10) [[11]](#11) [[12]](#12)

...And then Dreamforce 2016 hit. [[13]](#13)

I remember walking around the [i]Developer Forest[/i] at Dreamforce that year, trying to complete my Trailhead Quest for yet another stuffed plushie, when I walked by the Salesforce DX booth.  I had been following Greg Wester for awhile on Twitter, but the Salesforce DX demo he gave really caught my attention. That was the first time I had heard of Salesforce DX.  "This is amazing, and it's about time Salesforce developers had this type of functionality in place," is a thought that crossed my mind over and over. The goal for Salesforce DX was to pivot Salesforce development in 180 degrees, from an org-centric to a source-central development model.  

What is org-centric development? It's when you start development right within a Salesforce org.  Source control is rarely even in a discussion with this model, unless you're at an enterprise level.  Development may even utilize features such as Salesforce CPQ, Communities, Financial Services Cloud, Analytics, or packages, which proves very difficult.  To make the switch from org-centric to source-centric thinking and development, Salesforce DX brought in a concept with which called scratch orgs.  For more information  on scratch orgs, read here: https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs.htm.  

Scratch orgs, along with amazing enhancements to the Salesforce CLI really boosted the productivity of Salesforce development and deploys.  

## 2017 - Enter Containers and Continuous Delivery
Around the same time that Salesforce DX was dropped on the community, Docker containers, pipelines, and Continuous Delivery and Deployment tools because popular such as Heroku Pipelines, Bitbucket pipelines, Github Actions, Travis CI, Circle CI, and Salesforce community-led Cumulus CI.  In conjunction with Salesforce DX, these tools allowed Salesforce teams to:

1. Create unique development branches to work on specific tasks
2. Start truly utilizing source control.
3. Automate unit tests for both Apex as well as Lightning Web Components.
4. Automate code analysis and formatting
5. Automate deployments

It's no mistake that CD tools like the above, along with Salesforce DX, that we started to see another surge in community participation.  In the Salesforce ecosystem we started seeing new IDEs like Illuminated Cloud, Welkin Suite, and of course Visual Studio Code extensions.  Additionally,  partners like GearSet, Copado, Blue Canvas, and Flosum started thriving as Salesforce teams realized the importance of Salesforce DX and Devops.

## 2020 - Current Day
And that brings us to current day Salesforce DevOps.  Salesforce DX has really spurred the continued growth of Salesforce Devops.  Partners continue to invent new tools or enhance existing tools.  At the same time, DX has allowed teams to take full advantage of modern day CI and CD tools outside of Salesforce.  In fact, if you'd like to get started with Salesforce DX and Github Actions, you're in luck.  Feel free to use my Github template repository: https://github.com/dancinllama/sfdx-template.  Please fork the repo and send in any pull requests you might have.

Where do we go from here, you might ask? It'll take a long time, but we'll start seeing even wider adoption with Devops, and in particular Continuous Delivery.  There's still a huge gap with Salesforce customers not even adopting Source Control.  However, we'll see that gap narrow, it's just a matter of how long and how much at this point.  Salesforce is even rolling out the following feature in Summer 2020:  [https://releasenotes.docs.salesforce.com/en-us/summer20/release-notes/rn_sandboxes_source_tracking.htm](Track Source changes in Sandboxes Automatically).  This will help with source control adoption, but also I'm curious how it'll play into a DevOps strategy. 

## Conclusion
What did you think of this article?  Anything I missed?  Have a favorite CI / CD tool or Salesforce DevOps tool that you use?  Is your Salesforce org or are your clients utilizing source control?  Comment below, I would love to hear!

Cheers,
- James

## References
- <a id="1">[1]</a> https://salesforce.stackexchange.com/questions/40411/where-are-all-past-salesforce-release-notes
- <a id="2">[2]</a> https://en.wikipedia.org/wiki/CruiseControl
- <a id="3">[3]</a> http://resources.docs.salesforce.com/142/latest/en-us/sfdc/pdf/salesforce_winter06_release_notes.pdf
- <a id="4">[4]</a> https://en.wikipedia.org/wiki/Hudson_(software)
- <a id="5">[5]</a> http://resources.docs.salesforce.com/152/latest/en-us/sfdc/pdf/salesforce_spring08_release_notes.pdf
- <a id="6">[6]</a> http://resources.docs.salesforce.com/164/latest/en-us/sfdc/pdf/salesforce_spring10_release_notes.pdf
- <a id="7">[7]</a> http://resources.docs.salesforce.com/182/latest/en-us/sfdc/pdf/salesforce_spring13_release_notes.pdf
- <a id="8">[8]</a> http://solenopsis.org/
- <a id="9">[9]</a> https://pmd.github.io/latest/pmd_rules_apex.html
- <a id="10">[10]</a> https://perm-comparator.herokuapp.com/
- <a id="11">[11]</a> https://packagebuilder.herokuapp.com/
- <a id="12">[12]</a> https://orgdoctor.herokuapp.com/
- <a id="13">[13]</a> https://developer.salesforce.com/dreamforce/2016-recap






