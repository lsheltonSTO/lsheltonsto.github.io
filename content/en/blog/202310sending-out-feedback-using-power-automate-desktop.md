---
title: "Automate Sending Out Feedback Using Power Automate Desktop"
date: 2023-10-13T14:49:05-05:00
draft: true
author: "Lindsay Shelton"
tags: ["excel","power automate", "power automate desktop", "how to"]
thumbnail: /images/automate-sending-out-feedback-using-power-automate-desktop1.png
hidePageThumbnail: true
toc: true

---

If you didn't know, I spent the first 10 years of my career as a public middle school teacher.  It was a dark time, but one of the brightest lights of that time was my best friend Emily, who I met halfway through my stint as a teacher.  She's still teaching, and today on the phone she said, "Hey, so I was playing around in Word today and I saw a button that said Power Automate... that's what you do, right?  Can I do things with that?"

With a joyful spring in my metaphorical step, I responded in the affirmative, and together we cooked up a great use case for teachers everywhere.  Emily teaches MTSS and has a ton of data - data that she'd like to be sending out to parents and guardians.  However, it's an overwhelming task to take everything she's got in her Excel spreadsheets and get it out in emails on top of classroom management, professional development, other communication with colleagues and parents, and, you know, teaching.  So, we're going to automate it.

I'm going to walk you through how to set up a Power Automate Desktop flow that will take a list of students, their email addresses, and their grades, and send them an email with their grade and a personalized message.  This is a great way to send out feedback to students and parents/guardians, and it's a great way to get started with Power Automate Desktop.




## The Use Case

So we have various research support departments that have historically relied on Access databases to store their SOP documents - documents that very clearly lay out how exactly to do a particular action, procedure, or task with relation to their science.  These SOPs are referenced regularly and also go through at least yearly review processes.  They are also referenced by other departments, and so it's important that they are kept up to date and are easily accessible.

As a result of working with these departments, they end up with their own SOP document library, hosted on a centralized site but with non-inherited permissions so that they are private to just that department.  Each department has a variety of columns to store all sorts of metadata.  There are then a series of Power Automate approval flows created, depending on each departments' needs.  The last flow that the manager runs wipes the approval records, stores them in a separate list (I'll explain why later), and writes a copy of the fresly reviewed and updated SOP into the Institute Documents library for everyone to be able to access.

This could easily be adapted for any organization that has documents that need approval processes, especially a variety of types, around their work.

![Placeholder](/images/building-an-sop-library-and-approval-system2.png)

Also, for your own sake, uncheck this box immediately every time you go to share the document library.  It's an evil box and should always be unchecked, but that's just facts.

## The Columns

Some of the default columns that I use include:
* Category - choice column, default settings, list the categories of SOPs (that also defines a lot of the views)
* Completion Date - date/time column, default settings, this gets set when the final approval flow is run
* Review Date - calculated date/time column, default settings, this is set to be one year after the Completion Date so [CompletionDate] + 365
* For each type of initial approval: Supervisor Reviewer (people column, default settings), Supervisor Status (choice column, default settings, Yes, No, Pending are the choices), Supervisor Comments (multiple lines of text column, default settings) with wording differing between groups
* For the final vote:  Approved By Vote (choice column, default settings, Yes, No Pending are the choices), Approved By Vote Record (multiple lines of text column, default settings)
* Write to Institute Documents - choice column, default settings, Yes, No are the choices
* Notes - single line of text, default settings, this is for any notes that the manager wants to add to the SOP