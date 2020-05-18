---
title: "Converting a Change Set to a Salesforce DX project"
excerpt: >
  How to convert a non-Salesforce DX org into a Salesforce DX source format, and work with in Visual Studio Code / Salesforce DX.
categories:
  - Salesforce
tags:
  - Salesforce DX
  - Change Sets
toc: false
---

**Author's Note** This post was written back in 2018.  Since then, the Salesforce extensions for Visual Studio code have added a "Create Project with Manifest" command, which generates a default package.xml for you and let's you bring source down from a non-DX org into a DX source format.
{: .notice--primary}

I *really* dislike the change set UI.  I also had a want / need to start getting my team more involved with source control and Salesforce DX.   A lot of our projects are in existing orgs, so spinning up a scratch org and starting from scratch isn't really an option.  There is an option to migrate existing sandboxes to Salesforce DX  using an unmanaged package, but holy moly.  When you add a few fields to the package, it brings in a TON of dependencies!  I wanted the ability to cherry pick the dependencies I wanted to store in source control.  Reason being?  I only wanted the work that our team is in charge of developing instead of the client's complete metadata.

Enter change sets.  Turns out you can generate a package.xml through a change set (similar to what you would do through Eclipse or other IDEs).  Here are the basic steps I took to convert a change set to a DX project:

1. Create a change set in a sandbox.
2. Populate the change set with any necessary metadata.
3. Validate the change set against, say, production.
4. Rinse lather repeat until you're able to validate the change set successfully.
4. In workbench (workbench.developerforce.com), click on the Migration tab, then click Retrieve.
5. In "Package Names" specify the name of your change set.  You might have to delete / remove old change sets, otherwise, you'll get an error that multiple packages exist.
6. When the retrieve is successful, then click "Download zip file"
  - This zip file will have a root directory of "unpackaged", inside of which the package.xml file is stored.
7. Create a directory for DX project, and add the sfdx-project.json file, along with the config/project-scratch-def.json files, etc.  If you're connecting to a sandbox environment, you'll want to make sure project-scratch-def.json points to test.salesforce.com instead of login.salesforce.com
8. If you haven't already, authenticate with your sandbox (this will pop up an oauth screen in your web browser and ask for permission so that Salesforce DX can connect to it):
```sfdx force:auth:web:login -a HeyThisIsMySandboxAliasAndYouCanChangeIt```

9. Copy the package.xml from step 7/8 into the directory.
10. Issue the following command (It will retrieve the source from your sandbox, using the package.xml and generate a zip file in the root level of your project directory):

```sfdx force:mdapi:retrieve --retrievetargetdir ./  -k package.xml -u HeyThisIsMySandboxAliasAndYouCanChangeIt```

11. Make a temp directory for the contents of the zip file:
```mkdir temp```

12. Unzip the zip file into a temp directory:
```unzip unpackaged.zip -d ./temp/```

13. Change directories into temp.  If you see a folder called "unpackaged", rename this to "src".  Because.. reasons?
```cd temp | mv unpackaged src```



14. Now, convert the source from the "metadata api format" to the "dx format"
```sfdx force:mdapi:convert -r ./temp```

Boom, now you can either work with it in a scratch org, and/or commit it to source control.

