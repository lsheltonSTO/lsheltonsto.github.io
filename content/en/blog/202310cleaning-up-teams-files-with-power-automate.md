---
title: "Cleaning Up Teams Files with Power Automate"
date: 2023-10-13T12:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "microsoft teams", "how to"]
thumbnail: /images/cleaning-up-teams-files-with-power-automate1.png
hidePageThumbnail: true
toc: false

---

One of my power users, Lisa, reached out to me with an interesting use case this week.  She's very self-reliant and had done her due dilligence at searching for an answer to her problem before admitting defeat and coming to me (and thus to Power Automate).  When she said no one had a good answer for this in her searches, I knew that not only did I have to help her, but that I'd finally have a good blog post idea if I found the solution!

## The Use Case

Lisa is in a group for the wonderful administrative assistants at our organization, and people tend to post files in the main channel.  Lisa was getting really bothered by the fact that these files would just pile up in the General files section without folder structures (I can relate to Lisa a lot).  She wanted to know if there was a way to automatically move files posted in the main Teams file to a folder designated for this type of file.

I first checked all of the Teams triggers and actions and didn't find anything having to do with file management.  Then I remembered - it's ALL SHAREPOINT!  (But really, it is).  The file structure surfaced in Teams really lived in SharePoint, which meant that I could theoretically accomplish this using SharePoint triggers and actions.

## The Solution

This one is a quick win - one trigger and one action.

Before you do that, go into Teams and make a new folder for the files to be moved into.  Just make sure it's under the General folder.

Next, you will want to make an automated cloud flow and search for the trigger "When a file is created (properties only)" - it's a SharePoint trigger.  Once you've selected that, you will need to fill in the site address, which is just your Teams group name at your SharePoint address.  The Library Name is going to be "Documents" if no changes have been made to the default.  For the Folder, navigate to Shared Documents -> General.  This is the folder that files are posted to in the main channel of your Teams group.  Again, your mileage may vary if someone has changed the default folder structure.

![Placeholder](/images/cleaning-up-teams-files-with-power-automate2.png)

Then you are going to just add one action - "Move file" (it's also a SharePoint action).  

Current Site Address is going to be the same you selected in the trigger.  File to Move requires some dynamic content - it's way down in the list for me, so I'd suggest searching for "Identifier" - that's what you want to add in that box.  Destination Site Address will presumably be the same as the Current Site Address unless you want it moved to a different group.  For Destination Folder, navigate to Shared Documents -> General -> the new folder you've created to store these files in.  I called mine "Folder for Neatening".  Lastly, pick what you want to do with the file if it already exists in the destination folder.  I chose to move it with a rename, but you could also choose to replace it or fail the flow.

![Placeholder](/images/cleaning-up-teams-files-with-power-automate3.png)

And that's it!  Seriously!  Go test it out!

![Placeholder](/images/cleaning-up-teams-files-with-power-automate4.png)

Thanks to Lisa for the inspiration, and happy organizing!