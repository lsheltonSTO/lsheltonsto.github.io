---
author: Lindsay Shelton
title: Power Automate 101
date: 2023-08-14
description: For MPPC23
keywords: ["power automate", "how to"]
type: about

---

## Exercise 1

Pre-Requisite: Make a SharePoint list with the columns First Name, Last Name, Favorite Color
1. Go to make.powerautomate.com
2. Click Create
3. Click Automated cloud flow
4. Click “When an item is created (SharePoint)” (search for it if necessary) and Create – don’t worry about giving it a title, it will populate a generic one
5. In your trigger, for Site Address, you will select the address where you made your pre-req list
6. For List Name, select the name of the list you created
7. Click New step
8. Search for “Send an email” – this is an action
9. Click on “Send an email (V2)”
10. For “To”, put in your email address
11. For “Subject”, put in “New SharePoint List Addition”
12. For “Body”, put “First Name: (Title) Last Name: (Last Name) Favorite Color: (Favorite Color)” with what is in parentheses being dynamic content
13. Click Save
14. Once you’ve gotten an email, you can click Test -> Automatically -> With a recently used trigger -> Succeeded -> Test and run it to get a second email/see how it runs
15. Add an item to the SharePoint list and see if you get an email. If you did, you built the first flow correctly!

## Exercise 2

1. Go to make.powerautomate.com
2. Click Create
3. Click Automated cloud flow
4. Click “When an item is created (SharePoint)” (search for it if necessary) and Create – don’t worry about giving it a title, it will populate a generic one
5. In your trigger, for Site Address, you will put in the site you created the list from exercise 1 at
6. For List Name, select the list name you created for exercise 1
7. Click New step
8. Search for “Approval” and select “Start and wait for an approval”
9. For the Approval type, put “Approve/Reject – First to respond”
10. Give it a descriptive title
11. Assign it to someone else in the room (or yourself if you are doing this on your own).  You can assign it to more than one person separated by semicolons
12. For Details, put in some labels and dynamic content from the SharePoint list like we did previously. i.e. Favorite Color: (Favorite Color)
13. For Item link, search for “Link to item” in the dynamic content menu
14. For Item link description, just put “Link to the list item”
15. Click Save
16. You can add an item to the SharePoint list to trigger

You don’t need to stop where we did!  For example, what would make sense to do after an approval or rejection in Exercise 2?

## Additional Learning

* https://learn.microsoft.com/en-us/training/modules/get-started-flows/?wt.mc_id=DX-MVP-5005237

* https://app.pluralsight.com/library/courses/microsoft-power-automate-getting-started/table-of-contents 

* https://training.powerapps911.com/courses/power-apps-and-power-automate-getting-started 


