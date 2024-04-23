---
title: "Building a SharePoint SOP Library and Approval System: Part Two"
date: 2024-04-23T14:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["sharepoint","power automate","how to"]
thumbnail: /images/building-an-sop-library-and-approval-system25.png
hidePageThumbnail: true
toc: true

---

I said I'd come out with a part two... last year.  I just never said when, okay!  This is a direct follow-up to <a href="https://www.lindsaytshelton.com/blog/202308building-an-sop-library-and-approval-system/">this blog</a>, so if you haven't read that one yet, I'd recommend it.  It's a good one, I promise.

## The Use Case

Just to quickly remind you, this is the use case in this scenario:

So we have various research support departments that have historically relied on Access databases to store their SOP documents - documents that very clearly lay out how exactly to do a particular action, procedure, or task with relation to their science. These SOPs are referenced regularly and also go through at least yearly review processes. They are also referenced by other departments, and so it’s important that they are kept up to date and are easily accessible.

As a result of working with these departments, they end up with their own SOP document library, hosted on a centralized site but with non-inherited permissions so that they are private to just that department. Each department has a variety of columns to store all sorts of metadata. There are then a series of Power Automate approval flows created, depending on each departments’ needs. The last flow that the manager runs wipes the approval records, stores them in a separate list (I’ll explain why later), and writes a copy of the fresly reviewed and updated SOP into the Institute Documents library for everyone to be able to access.

This could easily be adapted for any organization that has documents that need approval processes, especially a variety of types, around their work.

## Before I Forget Again...

There are a few details in setting up your SharePoint library that I neglected to include in my first post.

First and foremost is versioning - you definitely want to set it up.  To do so, go to your library settings, click "Versioning settings", and say "yes" to "Require content approval for submitted items?".  I'd also recommend turning on major and minor versioning to do with this, but it's not necessary.  Here's how mine are set up:

![Placeholder](/images/building-an-sop-library-and-approval-system1.png)

Then there's the issues of views.  We built pretty basic ones - ones for each category:

![Placeholder](/images/building-an-sop-library-and-approval-system3.png)

We also add a dynamic one for "My Reviews":

![Placeholder](/images/building-an-sop-library-and-approval-system4.png)

And lastly, one for Pending SOPs:

![Placeholder](/images/building-an-sop-library-and-approval-system5.png)

Then, create yourself a few Test SOPs and you are ready to start building your flows!

## The Flows

There are three main components to each approval flow that I build:

* Using a "For a selected file" SharePoint trigger - this allows the manag4er to manage the flows from the document library itself.
* Error handling - for every action that updates the SharePoint metadata.  Seriously.  This is because the document will inevitably be locked for editing at various points in the process, and you need to be able to handle that.
* Approvals - Update the SharePoint metadata with the results of the approval action either way (accept or reject)

A few caveats about the "For a selected file" trigger that I've learned in these 2-3 years:

* You can’t use the new designer with this trigger yet (only sometimes, sad, I know)
* You can only use this type of flow in your default environment (actually sad, I know)
* You can still use it in solutions and use environment variables, even in the default environment (yay!)

![Placeholder](/images/building-an-sop-library-and-approval-system6.png)

Notice that I added one input here - for the first flow, I'm just calling it "Reviewer Email".  This is what it looks like to actually trigger this flow:

![Placeholder](/images/building-an-sop-library-and-approval-system7.png)

![Placeholder](/images/building-an-sop-library-and-approval-system8.png)

Note: I’d recommend a service acct owning these flows and connections.  Otherwise it shows as your name updating the SOP database, which is bad practice.

For the Error Handling, it’s VERY annoying, but you can’t update any of the metadata fields (aka progress in the flow) if someone has the file open anywhere.  So, every time you make a modification to the SharePoint doc library, you need error handling (and training on this for your users).

For every time you update SharePoint, you will do an updateMetaData variable and a Do Until loop:

![Placeholder](/images/building-an-sop-library-and-approval-system9.png)

![Placeholder](/images/building-an-sop-library-and-approval-system10.png)

See my blog about <a href="https://www.lindsaytshelton.com/blog/202306handling-locked-m365-files-in-power-automate/">error handling</a> on locked files for more information on how exactly to do this.

As for M365 Approvals, they are awesome!  You can approve things from Teams, Power Automate, or Outlook, and all three sync together, so it knows if you approved in one place and will show it in the others.  It’s a great way to keep track of what’s been approved and what hasn’t.

I use the two-part Approvals action – Start an approval and separately, Wait for an Approval.  It just provides more flexibility in how you do your flow.  For example, I add the approvers’ names to SharePoint between starting the approval and then the wait action (which has a limit of 30 days).

![Placeholder](/images/building-an-sop-library-and-approval-system11.png)

ReviewerEmail comes from the input of my trigger, and it can contain a comma-separated list of emails.

## ...Everything Else

The rest of my flow depends on whether the SOP was rejected or not – the actions are the same regardless, the text of the email that gets sent and what gets recorded in SharePoint is what varies based on the condition:

![Placeholder](/images/building-an-sop-library-and-approval-system12.png)

The link to the most basic version of this flow is in the <a href="https://github.com/pnp/powerautomate-samples/tree/main/samples/request-review-and-approval-for-a-selected-file">PnP Samples Gallery</a> for anyone to download!

## The Approval Group Vote

This flow is a beast.  Luckily only one group is big enough to need this, but they have enough users that they are in different SharePoint groups like I mentioned in my first post.  This is the flow that calls each of those groups (or at least which ones were selected) to vote on the SOP.  It would be an entire blog post to tell you exactly how to set this up, but I can tell you that it’s a lot of variables and a lot of repetition.

![Placeholder](/images/building-an-sop-library-and-approval-system13.png)

![Placeholder](/images/building-an-sop-library-and-approval-system14.png)

Basically, I have a multi-select choice input on my trigger, and then a switch case for each of my SharePoint groups.  I repeat the same actions in each switch case (probably should have made child flows, but this was my first big project and I didn’t know about them yet).  In each switch case, I do an HTTP call to SharePoint to get the group members, Parse JSON that output, append their User Principal Names to a string variable, and add the group name to another variable to track which groups were selected to vote on this SOP.

![Placeholder](/images/building-an-sop-library-and-approval-system15.png)

![Placeholder](/images/building-an-sop-library-and-approval-system16.png)

![Placeholder](/images/building-an-sop-library-and-approval-system17.png)

The rest of the flow structure is similar to a simpler approval flow, with lots of error handling of course.

## The Annoying Extra Flow

This flow is used immediately after the group approval – it clears out the fields and writes the data to a second SharePoint list because reasons.  Those reasons are that SharePoint metadata fields get angry when you have more than 255 characters in them.  I actually had to build a Manual Fix flow that grabs whatever is in the metadata and write it to this second list because when even one field gets too long, it won't let you edit and save the document.  It's another very annoying thing I've learned along the way.  But it's not all just to do this - this also updates the Completion and Review Date fields.

![Placeholder](/images/building-an-sop-library-and-approval-system18.png)

![Placeholder](/images/building-an-sop-library-and-approval-system24.png)

## The Final Flow!

This is the last flow in the process, and it makes a PDF version of the SOP (so no edits can be made), checks if it’s a new SOP or updating an old one, and adds it to the Institute-wide document library along with the Category and Department info

![Placeholder](/images/building-an-sop-library-and-approval-system19.png)

## Wait, There's One More

I almost forgot about the one flow in this process that isn't manually triggered.  It's a scheduled flow that runs every morning and checks for flows that are 30 days out from the Review Date value.  It sets their Content Approval Statuses back to Draft, resets all columns back to Pending in case the Annoying Extra Flow wasn't run, and could easily be used to send an email reminder that this SOP needs to be reviewed (my groups opted to not have this feature).

![Placeholder](/images/building-an-sop-library-and-approval-system20.png)

![Placeholder](/images/building-an-sop-library-and-approval-system21.png)

## What'd I Do For Fun?

Partly for fun for me and partly to ease their transition out of their old database, I built them a Power App that they can use to view all of their SOPs modeled after their old database solution.

Old version:

![Placeholder](/images/building-an-sop-library-and-approval-system22.png)

Lindsay's version:

![Placeholder](/images/building-an-sop-library-and-approval-system23.png)

## Conclusion

What would I do differently, looking back a few years later and many flows smarter?

* Make a child flow for the SharePoint error handling, or at least apply it more uniformly across my flows
* Write the flow approval records to a separate SharePoint list from the start to avoid having to use the manual fix flow I built or the annoying extra flow

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>