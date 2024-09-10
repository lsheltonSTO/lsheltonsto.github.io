---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio - Part 2"
date: 2024-09-10T09:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

Head back to the <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_intro/">intro post</a> to get the use case and architecture of the Copilot Studio bots I'll be describing in these blog posts, and <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_part1/">head to part 1</a> to get a general overview of Copilot Studio, Topics, how I figured out troubleshooting the bot (the best you can at this point in Copilot Studio), and an overview of the Power Automate flows used in the initial POC I built.

Today's post is about starting a conversation, aka the Conversation Start system topic.  This is the first topic that the user will encounter when they start a conversation with the chatbot.  It's a good idea to have a greeting and a prompt for the user to start the conversation.

Note that this behavior you see when testing will not behave the same way if and when the bot is deployed to Teams (at least at the time this blog was published).  The user can ask it whatever trigger phrases they want, but they won’t get the menu to pick from upon first interaction for whatever reason. 

We start out in this topic by inserting a sneaky flow as the very second node of our Conversation Start Topic - "Run a flow from Copilot - Get sys_id".  This flow is a premium flow that calls the ServiceNow API to get the user's sys_id based on their email address, System.User.Email, an automatically generated variable.  This is important because we will use this sys_id in our HTTP calls to ServiceNow to identify the user instead of having them log into ServiceNow, as that experience is not the most user-friendly at this point.  This way, they are silently logged in and we get the information we need from them without them having to do anything but open the bot in Teams or a canvas app.

This sys_id is an important global variable because we will use it in almost all of our future HTTP calls to ServiceNow to identify the user.  As soon as we get the user’s sys_id, we ask them what they would like help with today.  They can also achieve this experience by entering phrases, which we will get to later. 

![Placeholder](/images/concept_to_chatbot16.png)

This is what it looks like to the end user: 

![Placeholder](/images/concept_to_chatbot17.png)

If they ask a question, it prompts them to ask the question before redirecting them to the “Conversational boosting” topic, and storing that question in a global variable (Global.UserQuestion).  For each other type, they are immediately redirected to the appropriate topic. 

## The Power Automate Flow - Get sys_id

We add one trigger text input - Email.  When you go back to Copilot Studio and actually add in the flow, the value you will enter for this is System.User.Email.  This is the email address of the user that is interacting with the bot.  This is the only input we need for this flow.

This flow just has two actions in it after the trigger - a premium HTTP call and the "Return value to Power Virtual Agents/Respond to Copilot" action.

![Placeholder](/images/concept_to_chatbot18.png)

This is the entire flow!  The hardest parts are getting the HTTP call right, getting your authorization working, and the formula to return just the sys_id, but I've got you covered for all three.

* HTTP Call: https://(insert your instance name here>).service-now.com/api/now/table/sys_user?sysparm_query=email=@{triggerBody()['text']} AKA https://(insert your instance name here)service-now.com/api/now/table/sys_user?sysparm_query=email=(dynamic content for Email input).  This is a GET call.  Set your headers just like mine in the picture.
* Authorization: I used Raw as the method (expand "Show advanced options") and put my username and password into one string like this: username:password.  I then took "username:password" and used a Base 64 encoder website to encode the value.  Then for the Raw value, you just put the word "Basic " and then your encoded username/password.
* Return value to Power Virtual Agents/Respond to Copilot: This is where you will put the formula to return just the sys_id.  This is what I used: body('HTTP')?['result'][0]['sys_id']

Go back to Copilot Studio and refresh the flow action.  You should now see your input and your output as options.  Set your sysid output as a Global variable that I called Global.userSysId.  Now we can use it in all of our topics.

I'll see you in part 3, where we get into the really cool stuff - conversational boosting!


<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>