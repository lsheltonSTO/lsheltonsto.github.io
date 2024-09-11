---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio Intro"
date: 2024-09-08T08:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

ServiceNow is a behemoth of a product, but it also has a robust API which means that we can suck it into our clutches... I mean, ingest its data into our M365 ecosystem for a more seamless experience for our users.  I knew all of this, but then I saw this amazing video from <a href="https://www.youtube.com/watch?v=xRsoHOq4oKA">Dewain Robinson here</a>, and when I say it got me started, I mean it gave me the use case, showed me the basics of interacting with Copilot Studio, and inspired me to keep building on that use case to make my own Copilot Studio ServiceNow bot.  So endless thanks to Dewain for publishing this great video, and make sure to check it out.  I'm going to go through the same steps here in my first blog only because the UI keeps changing and it was a blocker for me in following exactly along with Dewain's video, so this will be current for at least another week until Microsoft changes the UI again.

NOTE: If you don't have a ServiceNow dev environment to play with, you can make one for free.99 here!  https://developer.servicenow.com/dev.do#!/home  It does go away if you don't log in every 10 days, and your stuff won't work unless you go log in and "wake it up", so it's a little finicky, but hey, the price is right and it comes with sample data.  The main thing to know about this is, once you've signed up, you need to change your role by clicking on your user icon in the top righthand corner and clicking "Change User Role:

![Placeholder](/images/concept_to_chatbot1.png)

Change it from "App Engine Studio Creator" to "Admin" and then you can access the REST API Explorer and actually look at ServiceNow like you'd expect it to look like.  This took me far longer to figure out than I care to admit.

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

And without further adieu - <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_part1/">here's Part 1</a>!

<a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_part2/">Part 2 - Conversation Start</a>

Want to contact me about your own Copilot Studio ServiceNow bot?  Do so below!

<!-- Read the Formbutton docs at formspree.io/formbutton/docs. See more examples at codepen.io/formspree -->
<script src="https://formspree.io/js/formbutton-v1.min.js" defer></script>
<script>
  /* paste this line in verbatim */
  window.formbutton=window.formbutton||function(){(formbutton.q=formbutton.q||[]).push(arguments)};
  /* customize formbutton below*/     
  formbutton("create", {
    action: "https://formspree.io/f/mqazoelw",
    title: "How can we help?",
    fields: [
      { 
        type: "email", 
        label: "Email:", 
        name: "email",
        required: true,
        placeholder: "your@email.com"
      },
      {
        type: "textarea",
        label: "Message:",
        name: "message",
        placeholder: "What's on your mind?",
      },
      { type: "submit" }      
    ],
    styles: {
      title: {
        backgroundColor: "gray"
      },
      button: {
        backgroundColor: "gray"
      }
    }
  });
</script>

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>