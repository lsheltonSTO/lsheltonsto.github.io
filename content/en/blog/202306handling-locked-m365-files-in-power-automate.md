---
title: "Handling Locked M365 Files in Power Automate"
date: 2023-06-26T14:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "how to", "error handling", "SharePoint"]
thumbnail: /images/handling-locked-m365-files-in-power-automate3.png
hidePageThumbnail: true
toc: true

---

Sometimes, late at night, I awaken, covered in sweat, fresh from another nightmare about Power Automate flows that interact with SharePoint document libraries.  Because what happens when you try to make changes to that document library, oh so innocently, when yet another innocent person has - gasp - the file open?  Then Power Automate and SharePoint both respectively freak out at you and nothing can get done until that document is cleared out.  SharePoint just gets up and faints at the thought of letting you edit a column of that library, and then Power Automate just walks right out the door, never to be seen again.  Chaos ensues.

Okay, maybe that's too dramatic of a retelling for the post that is re-launching my blog.  I will say this: if you try to do anything to a SharePoint document library while someone has the document open, just stop because you can't do anything.  Anything at all.

So, even though I'd love to get over this recurring nightmare, I can't help you with a certain basic level of freaking out.  I can, however, let you know that SharePoint is freaking out and prevent losing any of your data or information, through this error handling pattern in Power Automate!

## The Background

So I have built three SOP document libraries now in SharePoint for various groups at my work.  Each of those SOP libraries runs on a series of Power Automate flows to handle reviewing, voting on, and approving these SOPs whenever appropriate, in addition to being a repository for the SOPs for reference.

When one of our users runs an approval flow, there are certain things recorded in the SharePoint doc library for record keeping purposes - who was this sent to once it's sent out, and then what was their verdict, comments, and when did they approve or rejct it.  I need to record all of this, but WITHOUT error handling built in, the Power Automate flow would just STOP as soon as it encounters a file that is "locked for editing".  

The person with the file open isn't purposely locking the file - that's just the default behavior when someone has the file open.  Now because the flow would stop when it tries to make a change to the SharePoint doc library, I'd lose all of that data I was trying to copy, in addition to the fact that the flow has stopped running and I'd have to restart the whole thing.  All in all, not an ideal user experience.

Thus, error handling!

## The Basic Structure

For every SharePoint action that I'm going to utilize, I am going to need to initialize a variable.  This isn't fancy - it's just a boolean set to "false" using the expression editor (so not just typing in the word "false").  In this example flow, I've got two that I will call updateMetadata:

![Placeholder](/images/handling-locked-m365-files-in-power-automate1.png)

When I say to use the expression editor, I literally mean in the "value" field, switch from Dynamic content to Expression, type in the word "false" with no quotation marks, and click the blue OK button.

![Placeholder](/images/handling-locked-m365-files-in-power-automate2.png)

This value is set to false and will remain set to false until an action interacting with SharePoint is successful.  Basically, each updateMetadata gets its own Do Until loop.

![Placeholder](/images/handling-locked-m365-files-in-power-automate3.png)

Everything that is in the loop will repeat, sort of (wait for the Configure run after pieces!) until something in the loop turns the value of updateMetadata from false to true.  Once the value is set to true and it reaches the top of the loop again, the loop will break or stop and the flow will continue on as desired.

## Breaking It Down

So, I have the loop in this picture present because of the Update file properties action.  That action, if the file in question was open somewhere, would throw an error.

Thus, the next action, Send an email (V2) that the file is locked.  Notice that the arrow into that action is red and dashed.  That indicates that there is a Run after configured.

Click the three horizontal dots and click "Configure run after".  Your screen will then look like this:

![Placeholder](/images/handling-locked-m365-files-in-power-automate4.png)

By default, every action is configured to run after the previous action is successful.  We can change that.  Uncheck the box next to "is successful" in this case and check the boxes next to "has failed" and "has timed out".  Make sure to click the blue Done button.  

Now if that SharePoint action fails or times out, this action will trigger and an email will be sent.  If I am able to update SharePoint with no problems?  No email is sent and that action is skipped.  With me so far?

For your reference, here's the email I send out.  It goes to the user who triggered the flow and lets them know the name of the file that appears to be locked:

![Placeholder](/images/handling-locked-m365-files-in-power-automate5.png)

My next action is another SharePoint action that doesn't initially appear to do anything (Update file properties).  

![Placeholder](/images/handling-locked-m365-files-in-power-automate6.png)

That's fine.  I just want to know if the file is still locked.  You could add a delay in front of this one if you wanted, but I have a delay right after that accomplishes the same thing.

Click the three horizontal dots next to the Update file properties SharePoint action and click "Configure run after" again.  For this one, check all four boxes.  Whether the email is sent or not, I want it to check SharePoint again to see if I can edit the doc library yet.

On the first loop especially, this is probably going to fail, which is what the next two actions are for.

![Placeholder](/images/handling-locked-m365-files-in-power-automate7.png)

I put in a 10 minute delay - make yours longer or shorter as you desire, but keep in mind that those emails will start to get annoying.  10 minutes has been a good median time for us - it doesn't totally stop the workflow but it does give people time to check the file and ping people to get out of it.

For the Configure run after for the Delay action, only check "has failed" and "has timed out".  We only want to wait the 10 minutes if we couldn't update SharePoint.

Then, lastly, there's a Set variable action for that updateMetadata variable.  We now want to set it to true (remember to use the expression editor), but ONLY if the previous Delay action was skipped.  So, for the Set variable Configure run after, set that to just "is skipped" with no other boxes checked.  Yours should be like this for the last two actions:

![Placeholder](/images/handling-locked-m365-files-in-power-automate8.png)

Click the blue "Done" button on both and get ready to test (might want to set the Delay shorter for testing).

I've been using this error handling loop with great success for 2 years - I hope it also brings you success!