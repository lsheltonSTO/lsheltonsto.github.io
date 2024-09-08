---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio - Part 3"
date: 2024-09-08T08:29:05-05:00
draft: true
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

Head back to the <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_part1/">intro post</a> to get the use case and architecture of the Copilot Studio bots I'll be describing in these blog posts.

Next up is a big one - Conversational Boosting.  Again, note that most of this content was adapted from this amazing video from <a href="https://www.youtube.com/watch?v=xRsoHOq4oKA">Dewain Robinson here</a> - this is just what I got working with the UI changes that have happened since the video first came out five months ago (now being September 2024, so who knows when the UI will change again).

## Conversational Boosting Topic

This is where the GenAI puts in the most work, and it also is the most complex topic by far.  It is based on a system topic that was just expanded upon.  
It starts by feeding the question to ServiceNow through an Out of the Box connector:
 
The limit is set to 3 due to a Copilot restriction – you can only really take in 3 articles for assessment, so we limit it to 3.  If you want additional filtering, you can change this action to a Power Automate flow or an HTTP action in Copilot Studio.
Next, this information needs to be formatted properly for generative answers.  So, we click on “Variable management – Set a variable value”.  We want to set a new topic variable called FormattedSearchResults.  You can create a new variable by clicking the right arrow, clicking “Create a new variable”, and then clicking on the “Var1” variable box to rename it to what you want.
 
 
 
What we want to set the variable to is the most complicated part because it does involve some PowerFx coding to build the formatted table correctly.  This is the code currently being used, which you can view by clicking the right arrow and clicking Formula (and clicking the arrow to expand it):
 
ForAll(
    Topic.articles,
    {
        Content: Concatenate(title, " - ", fields.value),
        ContentLocation: Concatenate(
            "https://powerengdev.service-now.com/now/nav/ui/classic/params/target/kb_view.do%3Fsys_kb_id%3D",
            Substitute(ThisRecord.id, "kb_knowledge:", "")
        )
    }
)
This might require further tweaking depending on how different your prod and dev environments are, but if they are identical, all that should have to be changed is the start of the URL from “powerengdev” to “powereng” to get production results.  Make sure you click the “Insert” button to save your changes.
Now we can use the “Create generative answers” node that was already in this topic, but we have to make edits to it.  Note that the Copilot Studio UI is changing frequently, so this accurate as of 9/1/2024.
For input, we want the Global.UserQuestion variable – click the right arrow to open the menu to select it under “Custom”.  Then click the “Edit” hyperlink for “Data sources”:
 
The options we want are hidden currently under a collapsed by default “Classic data” tab – click on “Classic data” to expand it and scroll down to where it says “Custom data”:
 
 
For “Custom data”, click the right arrow to insert that new variable we made, “FormattedSearchResults”.  This means that this generative answers node will take the user’s question and search the formatted results of our Get Knowledge Articles action results to give us an answer.
Scroll to the bottom and expand “Advanced” and make sure your settings look like this:
 







If we save and test it out, this is how it should work:
 
When you click on the reference link, if you’ve formed your links correctly in the ForAll formula, it will take you directly to the referenced KB.
If the Topic.Answer was not blank, we added in a question:
 
If the user selects “No, I’d like to search again”, that “Connector action” is set up as follows:
 
And this is the step/action in question that it takes the user back to (which doesn’t prompt them again, it will just search again):
 


![Placeholder](/images/concept_to_chatbot2.png)

## The Use Case

So the use case here started out pretty simply - I wanted to play with Copilot Studio and learn how to use it.  Once I saw Dewain's video, I knew that was going to be a fantastic use case for me to test out for a few reasons.  One reason is that my organization uses ServiceNow extensively in addition to M365.  Another reason is that I'm a millennial, and I'm a pretty stereotypical millennial.  I don't want to talk to customer service people.  If there is a chatbot, I'm using the chatbot, and so do every single one of my friends in my age group that I've asked.  We love avoiding human contact, and so I knew my use case and my key audience - people like me.

![Placeholder](/images/concept_to_chatbot3.png)

I actually proposed this idea to my org's Hackathon that we just finished, so I have both what I worked out on my own and what my team worked out to blog about.  So hold onto yer britches, because I've got a lot of content about Copilot Studio and ServiceNow to share.

## The Architecture

This is the architecture for what I developed on my own (though I couldn't have done the work without the help of greats like Azure McFarlane, Joe Unwin, Troy Taylor, and Jeff Charikofsky):

![Placeholder](/images/concept_to_chatbot4.png)

What we developed for the Hackathon was a bit different.

![Placeholder](/images/concept_to_chatbot5.png)

I will eventually go over and document all of the things, but I can only write so much at a time!  First up in this series - is this my first series? - is going to be the conversational boosting topic (Ask a Question), including some of our struggles with it at my org.  So stay tuned, get your ServiceNow dev environment going, and feel free to poke me on Twitter or LinkedIn if I'm dilly dallying too long on getting the next part out!

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>