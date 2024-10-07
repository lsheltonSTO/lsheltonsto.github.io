---
title: "From Concept to Chatbot: ServiceNow with Copilot Studio - Part 3"
date: 2024-10-07T08:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["power automate", "copilot studio", "service-now", "how to"]
thumbnail: /images/concept_to_chatbot6.png
hidePageThumbnail: true
toc: true

---

Head back to the <a href="https://www.lindsaytshelton.com/blog/202409from_concept_to_chatbot_servicenow_with_copilot_studio_intro/">intro post</a> to get the use case and architecture of the Copilot Studio bots I'll be describing in these blog posts.

Next up is a big one - Conversational Boosting.  Again, note that most of this content was adapted from this amazing video from <a href="https://www.youtube.com/watch?v=xRsoHOq4oKA">Dewain Robinson here</a> - this is just what I got working with the UI changes that have happened since the video first came out five months ago (now being October 2024, so who knows when the UI will change again).

## Conversational Boosting Topic

This is where the GenAI puts in the most work, and it also is the most complex topic by far.  It is based on a system topic that was just expanded upon.  

It starts by feeding the question to ServiceNow through an Out of the Box connector (remember, in Part 2 we stored the response to the user question in a global variable - we are going to use it here for our query):

![Placeholder](/images/concept_to_chatbot19.png)
 
The limit is set to 3 due to a Copilot restriction – you can only really take in 3 articles for assessment, so we limit it to 3.  If you want additional filtering, you can change this action to a Power Automate flow or an HTTP action in Copilot Studio, which I had to do for various iterations of this bot.  We will try the path of least resistance for this blog, but let me know if you run into trouble and I can add a 3.5 version of this blog for the advanced scenarios.

Next, this information needs to be formatted properly for generative answers.  So, we click on “Variable management – Set a variable value”.  We want to set a new topic variable called FormattedSearchResults.  You can create a new variable by clicking the right arrow, clicking “Create a new variable”, and then clicking on the “Var1” variable box to rename it to what you want.
 
![Placeholder](/images/concept_to_chatbot20.png)

![Placeholder](/images/concept_to_chatbot21.png)

![Placeholder](/images/concept_to_chatbot22.png)
 
What we want to set the variable to is the most complicated part we are going to have to do because it does involve some PowerFx coding to build the formatted table correctly.  This is the code I'm currently using, but it make take some adjustments depending on your environment
 
![Placeholder](/images/concept_to_chatbot23.png)

ForAll(
    Topic.articles,
    {
        Content: Concatenate(title, " - ", fields.value),
        ContentLocation: Concatenate(
            "https://dev251307.service-now.com/now/nav/ui/classic/params/target/kb_view.do%3Fsys_kb_id%3D",
            Substitute(ThisRecord.id, "kb_knowledge:", "")
        )
    }
)

Make sure you click the “Insert” button to save your changes.

Now we can use the “Create generative answers” node that was already in this topic, but we have to make edits to it.  Note that the Copilot Studio UI is changing frequently, so this accurate as of 10/7/2024.

For input, we want the Global.UserQuestion variable – click the right arrow to open the menu to select it under “Custom”.  Then click the “Edit” hyperlink for “Data sources”:

![Placeholder](/images/concept_to_chatbot24.png)
 
The options we want are hidden currently under a collapsed by default “Classic data” tab – click on “Classic data” to expand it and scroll down to where it says “Custom data”:

![Placeholder](/images/concept_to_chatbot25.png)

![Placeholder](/images/concept_to_chatbot26.png)
 
For “Custom data”, click the right arrow to insert that new variable we made, “FormattedSearchResults”.  This means that this generative answers node will take the user’s question and search the formatted results of our Get Knowledge Articles action results to give us an answer.

Scroll to the bottom and expand “Advanced” and make sure your settings look like this:
 
![Placeholder](/images/concept_to_chatbot27.png)

If we save and test it out, this is how it should work:

![Placeholder](/images/concept_to_chatbot28.png)
 
When you click on the reference link, if you’ve formed your links correctly in the ForAll formula, it will take you directly to the referenced KB.

If the Topic.Answer was not blank, we added in a question:

![Placeholder](/images/concept_to_chatbot29.png)
 
If the user selects “No, I’d like to search again”, that “Connector action” is set up as follows:

![Placeholder](/images/concept_to_chatbot30.png)
 
And this is the step/action in question that it takes the user back to (which doesn’t prompt them again, it will just search again):
 
![Placeholder](/images/concept_to_chatbot31.png)

## Bing Custom Search

If there is no answer found in the KBs, we have what is called a fallback source.  For “All other conditions”, meaning the answer is blank, we have a second “Create generative answers” node.

![Placeholder](/images/concept_to_chatbot32.png)

This source is actually a number of websites that we’ve curated that we want to be searched for an answer, made possible by Bing Custom Search.

Go to https://www.customsearch.ai/ and click “Get Started”.

![Placeholder](/images/concept_to_chatbot33.png)

Sign in with a Microsoft account or create one with a service account.  This will be where you create your “Custom Search Instance” that you can share with other accounts.

![Placeholder](/images/concept_to_chatbot34.png)

If you click on the hyperlinked text “Only you”, it takes you to this interface where you can share the custom search instance with other email addresses.

![Placeholder](/images/concept_to_chatbot35.png)

If you click “Configuration” up top, this is where you can add the websites that you’d like the Bing Custom Search to crawl for your answers.  This is a sample of what I have set up:

![Placeholder](/images/concept_to_chatbot36.png)

List of URLs to copy/paste:
* http://support.microsoft.com/
* http://learn.microsoft.com/
* https://www.dell.com/support
* https://www.samsung.com/us/support
* http://support.google.com/android  

Important note: you can only go two levels deep with backslashes when trying to add addresses, so you can’t get very specific.  The Samsung URL is as deep as you can go into a URL.

Note that you can rank which sites you’d like to be crawled first, and whether or not you’d like to include the subpages (it’s recommended to keep this at “Yes”).  You can also block URLs from the search by clicking the “Blocked” tab or by clicking the circle with the line through it icon next to the URL.

To add additional websites, just paste in a URL where it says “Type in a URL” at the bottom of the previous screenshot.
When you make changes, it will prompt you to press “Publish” in the top right-hand corner.  The value you will need when your Bing Custom Search Instance is ready comes from the “Production” tab at the top.  You need to copy the “Custom Configuration ID” and that is what gets pasted into Copilot Studio as the generative answer data source.

![Placeholder](/images/concept_to_chatbot37.png)

![Placeholder](/images/concept_to_chatbot38.png)

Hint: to get the Bing Custom Search dialog to come up, click the blue text that by default says “Search public websites” and change it to “Search with Bing Custom Search”.  Also note that this feature, as of 10/7/2024, is under the “Classic Data” collapsed tab.

When testing this feature, if there are no ServiceNow KB sources found, you as the developer will see this “Only visible to you” box:

![Placeholder](/images/concept_to_chatbot39.png)

Note that we again have multiple choice options after an answer is given.  Just like before, if the question is answered, we take them to the End of Conversation topic, and if they want to search again, we take them back to the “Get Knowledge Articles” action.

![Placeholder](/images/concept_to_chatbot40.png)

There is a new option if both the ServiceNow and the fallbacks haven’t given them the answer they are looking for, which is “No, I’d like to open a ticket with the help desk”... which will have to wait for Part 4!

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