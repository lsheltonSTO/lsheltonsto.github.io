---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio - Part 1"
date: 2024-09-08T08:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

Head back to the <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_part1/">intro post</a> to get the use case and architecture of the Copilot Studio bots I'll be describing in these blog posts.

In this post, I'll be giving a general overview of Copilot Studio, Topics which are the main thing I utilized (though there's much more that can be used), how I figured out troubleshooting the bot, and the Power Automate flows used in the initial POC I built.

## Introduction

Copilot Studio is a low-code platform within Microsoft's Power Platform, designed to enable the creation of intelligent apps and automation using natural language processing, AI, and automation capabilities. It empowers users to build custom chatbots, applications, and workflows without needing extensive coding knowledge. 

Key features include:
1.	Natural Language Integration: Copilot Studio uses conversational AI to understand natural language, making it easy to build chatbots that can interact with users in a human-like manner.
2.	Low-Code Environment: The platform provides a visual interface where users can design workflows, chatbots, and integrations by clicking to add and remove components. It uses PowerFx, a formula-based language, for building logic and managing data.
3.	Integration with Microsoft Power Platform: Copilot Studio seamlessly integrates with other Power Platform services such as Power Automate, Power Apps, and Power BI, allowing chatbots to trigger workflows, retrieve data, and display information.
4.	AI-Driven Automation: The platform allows users to create chatbots that automate tasks by responding to user requests, retrieving information from databases, and even connecting to external systems like ServiceNow, which is what you will see in this chatbot.
In summary, Copilot Studio is designed to streamline chatbot creation, making it an ideal tool for businesses looking to implement conversational AI without heavy coding. Its integration capabilities and natural language processing make it highly adaptable for automating various processes.

## Topics
In Copilot Studio, a "topic" is a fundamental concept used to define the specific conversations or tasks a chatbot is designed to handle. Topics are essentially the building blocks of the chatbot's conversational flow, guiding how it interacts with users and responds to their inputs.

Key Aspects of Topics in Copilot Studio:
1.	Defining Conversations: A topic represents a particular subject or intent the chatbot is trained to manage. For example, in a ServiceNow chatbot, there might be topics like "Check Ticket Status" or "Create a New Incident". Each of these defines a unique conversation path.
2.	Trigger Phrases: Topics are activated by trigger phrases, which are specific words or sentences a user might say to initiate that topic. For example, the topic "Check Incident Status" could have trigger phrases like "What's the status of my incident?" or "Track my ticket." The chatbot listens for these phrases and, when detected, activates the corresponding topic.
3.	Dialog Flow: Once a topic is triggered, it defines the dialog flow, or the step-by-step conversation the chatbot follows. This includes asking the user for necessary information, performing actions such as retrieving data from external systems, and responding with relevant details.
4.	Actions and Automation: Within a topic, you can define actions the chatbot should take, such as calling external APIs, triggering workflows in Power Automate, or retrieving data from a database like Dataverse. For instance, in the "Check Incident Status" topic, the chatbot can retrieve incident details from ServiceNow based on user input.
5.	Multiple Topics: Chatbots in Copilot Studio can manage multiple topics simultaneously. If a user asks for a variety of tasks, such as checking an incident and then creating a new one, the chatbot can handle transitions between topics smoothly, maintaining context throughout the conversation through the use of global variables.
6.	Customizable and Expandable: Users can create custom topics to fit their business needs. This flexibility allows the chatbot to cover a wide range of use cases, such as answering FAQs, providing support, or automating complex processes.
7.	Fallback Topics: Copilot Studio also supports fallback topics to manage situations when the chatbot doesn't understand the user's intent. In this case, the chatbot can either ask the user to clarify or provide alternative suggestions, ensuring that the conversation doesn't abruptly end.

## Topics in this POC
There are two categories of topics in Copilot Studio – custom and system.  Both can be modified, but the key difference is that system topic triggers tend to have triggers for scenarios that happen with chatbots, such as the user providing a phrase similar to but not exactly the same as a trigger phrase you’ve provided in a custom topic trigger.  For example, the “Multiple Topics Matched” system topic, which is automatically created, prompts the user with, “To clarify, did you mean:” and a list of actual topic options that you’ve created to select from.

These are the custom topics for this POC that I've built (with Goodbye, Greeting, Start Over, and Thank You automatically created when you build a new bot):

![Placeholder](/images/concept_to_chatbot7.png)
 
These are the system topics, with most of the modification made to the first three topics:

![Placeholder](/images/concept_to_chatbot8.png)

## Power Automate Flows

One lesson learned is that, when trying to do certain actions or manipulate data, it was much easier at this point in Copilot Studio’s maturation to call Power Automate flows instead, which you can easily do in the context of a Copilot Studio topic, both feeding it inputs and receiving outputs from the flow.

A few tips on these flows:
* To create a new one, click “Call an action” and then “Create a flow” so that it connects with the chatbot.  However, there is some weird behavior with their default trigger and end actions:

![Placeholder](/images/concept_to_chatbot9.png)
 
So, on each of the flows that were built for this POC, these were replaced with the legacy trigger and end action:
![Placeholder](/images/concept_to_chatbot10.png)
 
They work almost exactly the same, but the newer ones sometime seem to require extra authentication steps inexplicably.
*	Don’t leave the flow name as the default “Run a flow from Copilot” – it is recommended to leave that prefix there so that you can easily tell which flows are connected to a chatbot, but it’s best to add context to the title for what that flow does so that you can tell them apart in the menu:
![Placeholder](/images/concept_to_chatbot11.png)
 
There were six flows created for this initial project that are in the final POC, and they are all premium flows because both the ServiceNow connector and the HTTP connector, which ultimately proved to be more reliable and maneuverable, are premium connectors.  The six flows are here, and they will be thoroughly documented in later parts of this series:
![Placeholder](/images/concept_to_chatbot12.png)
 
## Troubleshooting the Bot

The troubleshooting options aren’t fantastic in Copilot Studio yet, but here are a few tips:
*	Use this “Refresh” button liberally.  Once you save changes, refresh your test window to see the changes reflected/start over.
*	You can turn on “Track between topics” when using the “Test your copilot” option:
![Placeholder](/images/concept_to_chatbot13.png)
This allows you to follow along with the topic and nodes (the blocks) as the conversation is happening, so if there’s a weird conversation turn, you can track where it went wrong.  You can also click on a block in  your conversation to go to that node/topic.
*	You can download “Save snapshot” (see screenshot above) to get this:
![Placeholder](/images/concept_to_chatbot14.png)
*	As the copilot is running a test conversation, click “Variables” and click “Test” to see the values of the variables (both global and topic) as you go.  You can’t see system variables this way, so those you would just have to put into a “Send a message” node to see their values temporarily.
![Placeholder](/images/concept_to_chatbot15.png)


<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>