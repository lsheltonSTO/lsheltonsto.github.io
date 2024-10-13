---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio - Part 4"
date: 2024-10-14T08:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

Head back to the <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_intro/">intro post</a> to get the use case and architecture of the Copilot Studio bots I'll be describing in these blog posts.

Next up is a shorter one for the shorter work week for some of us in the US - what if the user isn't happy with any of the answers that our gen AI has provided for them and they want to open a ticket or incident with the help desk?  This can be as easy or as difficult as your organization makes it, so let's start with the easy version.

## Opening an Incident

Depending on your org, maybe all that your help desk needs is a short description and the caller to get started.  If that's the case, consider yourself lucky, roll up your sleeves, and here we go!

![Placeholder](/images/concept_to_chatbot41.png)

We could get really simple and just feed the help desk the question the user was asking the chat bot/agent, but I knew if it was me, I'd want the chance to add a little more detail before putting in a ticket.  So I added an "ask a question" node where we will save the user's entire response as a topic variable - HelpDeskTicketText in my case.

Just so things aren't too easy (haha, like that ever happens), I threw in an urgency option.  Note that this won't actually work until the action is connected with an account with high enough privileges to set urgency, but I want to at least show you how to add one more thing in than just the caller and the short description (the action comes with over 100 things you can put in.  Seriously).

![Placeholder](/images/concept_to_chatbot42.png)

So we've created a pathway for each urgency level that I've defined in this case.  By default, the urgency is 3 or Low, so we can just use our connector to make the new ticket right away.  If the urgency is 2 or 1, we set a variable to that value before calling the “Create Record” action.  Here I was able to go out of the box with the action - your mileage may vary.

![Placeholder](/images/concept_to_chatbot43.png)

We want to set the only required field on this action, the Record Type, to Incident.  It's okay to hard-code this, it's always going to be an INC.  Notice what we did with the other two urgency pathways, though (which also could have just been done in the action itself) - the action doesn't want to accept integers for some reason (Copilot Studio tries to turn the variable into a number type, which isn’t accepted by the connector), so you do have to convert them to strings, or text(), before putting them into the action.  Don't ask me why, I just work around the error messages.

There are over 100 fields you can populate for the “Create Record” action, but we only fill out a few:
* Record Type: Incident (just as a typed out string)
* Created by:  Global.UserSNSysId (insert the variable)
* Short description:  Topic.HelpDeskTicketText (insert the variable)
* Affected user:  Global.UserSNSysId (insert the variable)
* Urgency (just on Medium and High): Topic.Medium/High (insert the variable).

At the end of the choices, the user is told that their ticket has been created and is given the incident number.  The formula used to get the incident number is: Topic.CreateIncidentResults.number.

![Placeholder](/images/concept_to_chatbot44.png)

## What If This Isn't Working?

Now, in my use case, I kept running into a problem - even though I was sending across the "created by" and "affected user", it was still populating the owner of the connection as one of the fields.

So, if the OOB connector isn't cutting it for you, here's the Power Automate version!

Instead of feeding values into the OOB action, I built a Power Automate flow and feed in three pieces of info:

![Placeholder](/images/concept_to_chatbot45.png)

The three inputs I ask for (in the Power Automate trigger) are the short description, the urgency, and the user's sysId.  These are pretty simple - text, number, then text inputs again.

My secret here?  I'm actually still using the OOB action, but I'm doing it in a flow to reduce the load of what was happening in Copilot Studio itself (I also didn't have this in its own topic at this point and was trying to edit it on a Surface Go, Bad Lindsay!).

![Placeholder](/images/concept_to_chatbot46.png)

I still fill in the exact same fields I was filling in before, just within the context of the flow now.

Then I just do a quick Parse JSON of the output of that action and grab the INC number, which I return to Copilot Studio to then relay it to the user.

![Placeholder](/images/concept_to_chatbot47.png)

Next up, we will talk about the process of creating a request instead of an INC (and why you might not want to actually do it in Copilot Studio).

Want to contact me about your own Copilot Studio ServiceNow bot?  Do so below!

<style>
  /* Form styling */
  form {
    max-width: 600px;
    margin: 20px auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: #f9f9f9;
  }

  label {
    display: block;
    margin-bottom: 10px;
    font-weight: bold;
  }

  input[type="email"], textarea {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  /* Styling for the button */
  button[type="submit"] {
    background-color: #ff69b4;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
  }

  button[type="submit"]:hover {
    background-color: #ff1493;
  }
</style>

<!-- Contact Form -->
<form action="https://formspree.io/f/mqazoelw" method="POST">
  <label>
    Your email:
    <input type="email" name="email" required>
  </label>
  <label>
    Your message:
    <textarea name="message" required></textarea>
  </label>
  <button type="submit">Send</button>
</form>

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>