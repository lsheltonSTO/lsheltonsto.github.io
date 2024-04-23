---
title: "Brightening Neurospicy Days: Affirmations and Dad Jokes Delivered by Alexa"
date: 2024-04-10T08:29:05-05:00
draft: true
author: "Lindsay Shelton"
tags: ["personal","how to", "power automate", "api", "power apps", "neurospicy", "alexa", "smart home"]
thumbnail: /images/BrighteningNeurospicyDaysAffirmationsandDadJokesDeliveredbyAlexa.png
hidePageThumbnail: true
toc: true
---
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>

## Extending My Neuropsicy Toolkit

![Placeholder](/images/BrighteningNeurospicyDaysAffirmationsandDadJokesDeliveredbyAlexa.png)

So, I had all of these great flows to send me all sorts of helpful emails for all sorts of moods, but I love a challenge and I didn't feel like I was done yet.  I must admit an even darker personal secret to you now - this was a Google home for a time, and I own two Google Home devices.  However, their API might as well be a black box, because I understand nothing about it.  However, upon searching, I did find connectors that already existed for their competitor, Amazon's Alexa.  Because sometimes I make bad choices and because I have Amazon Prime, within 24 hours of the idea, my little Echo Dot had arrived (along with a new dual printer/scanner, but that's a story for another day).

What followed was a weekend-long journey of frustration, occasional joy, frustration again, and then, eventually, victory.  I'm goign to spare you all of the twists and turns that I went down and just share with you the end solution that works.

## Setting Up Voice Monkey

I used a third party service, <a href="https://voicemonkey.io/">Voice Monkey</a>, rather than the direct API because it was free for what I wanted to accomplish.  Spoiler alert, I ended up paying the exorbitant $2.50 a month to add a farting noise after Alexa told me a dad joke.  I make my own money, I decide how I spend it.

By far the hardest part was getting my device to be recognized by Voice Monkey.  

![Placeholder](/images/delivered-by-alexa1.png)

Get logged into Voice Monkey and you will want to go to Assets - Devices and click the green "Add a speaker" button.  There's instructions within Voice Monkey that are pretty clear, but I kept running into trouble.  <a href="https://voicemonkey.io/docs#getting-started">Starting off was fine using their docs</a>.

![Placeholder](/images/delivered-by-alexa2.png)

These are Voice Monkey's instructions for creating a device and linking it to Alexa, so step 3 of the overall setup, but here's what I learned from troubleshooting when I couldn't find my speaker in the Alexa app:

![Placeholder](/images/delivered-by-alexa3.png)

You don't want your phone to look like this when you've added a speaker in Voice Monkey (this was step 5 I was stopped at).  It should have been giving me the option to select "Lindsay Echo2" as a speaker, or whatever you name it, but it wasn't. 

One thing to check that was causing me troubles is I think I was somehow signed into a different Amazon account because I was trying to do this on my work computer.  I had to sign out of Amazon and sign back in with my personal account... and it still didn't work.

I opened a ticket with Voice Monkey, and this is what they suggested if I was still having trouble:

"Could you try going to the Alexa app ... More... Skills and Games. Then try disabling the Voice Monkey skill and re-enabling it. Once re enabled it will attempt to discover the devices and hopefully may find your Voice Monkey device 🤞"

It was now discovering my device I had created, but it gave me an error:

![Placeholder](/images/delivered-by-alexa4.png)

However, once I started from scratch and created a new speaker (thus mine now being Lindsay Echo2), it worked as the directions instructed, and I was able to complete all of the steps.

Now, you could press the "Test" button on this page in Voice Monkey if you want, but I'm warning you now - the sound they decided to use to test the connection is a really loud monkey shriek.  I was not prepared for that, and I'm warning you now so you can be.

## Playing With the API

I don't recall if you needed to set this up or if it was already set up for me, but if you go to Settings - API Credentials, you can find your secret API token.   You will need this for your call in Power Automate, and I'd recommend putting it in an environment variable in your solution.  If it gets compromised, you can regenerate it here.

In the left nav, you can go to API v2 - Announcement API, as that is the one we want to play with in this scenario.  This is where you can get the full API call you need, and it includes your API token for you, so you can test it out with a phrase right away.  This is also where I'd recommend testing it out at all, as that monkey noise is scary.

![Placeholder](/images/delivered-by-alexa5.png)

You will notice as you make changes in the left, it changes the API URL for you, which gives you really useful insight into what you will need to do in Power Automate.  For example, when I typed in the phrase "Hello beautiful", you will notice that it became encoded in the URL, aka adding those %20s.  Luckily there's an expression that will do that for us in Power Automate.  You can also change the voice, the language, add some default audio sound effects (all in the free version!), and even add your own media if you fork out the $2.50 a month.  I just have an Echo Dot without the screen, so I didn't get too deep into all of these settings, but just know that as you modify them or add them on the left, it will update the API call for you so you know what Power Automate needs to know to do that same call for you.

## The Power Automate Flow

So I was able to reuse part of my flow from sending positive affirmations to my email and simply removed a step/added a few new ones.  I won't go into great detail about getting the positive affirmations from the SharePoint list - go to <a href="https://www.lindsaytshelton.com/blog/202404the_power_of_positive_affirmations_and_power_automate/">my previous blog post</a> to see how to do that.

So take that flow and remove the "Send an Email (V2)" action, since now we want to send a message to Alexa with this flow.  As a side note - this will now make this a premium flow due to the HTTP action, so make sure you have the appropriate licensing.

If you have dug around in Power Automate, you might have noticed this:

![Placeholder](/images/delivered-by-alexa6.png)

Feel free to try them, but I couldn't ever get them to work.  They are premium anyway, so there's not much difference between using these connectors and just using the HTTP call.

You want to add another "Compose" action.  Add the following expression to it:

```html
UriComponent(body('Parse_JSON')?['Title'])
```

This is going to take whatever is in your "Title" field for SharePoint (so modify based on which column your values are in) and encode it for the API call.

Next, add an "HTTP" action.  You will want to use the "GET" method, and you will want to put in the API URL from Voice Monkey.  Remember, your API token needs to be secret, so I recommend putting it an environment variable in your solution and referencing that in your HTTP call.  I'd also recommend going to the "..." on the HTTP action, clicking "Settings", and turning on "Secure Inputs".  This means that anyone who looks at your flow run can't see your token that you are sending.

![Placeholder](/images/delivered-by-alexa7.png)

This is what my HTTP action looks like, with an environment variable for my token and the Output of my Compose 3 action for the text value.

That's it!  Save it and trigger it from either your Power App or manually, and you should hear Alexa calmly affirming you within seconds.

## Advanced Options, aka Fart Noises

Now the call for Alexa tell me a Dad Joke is almost identical - take my email-version flow, remove the "Send an Email (V2)" action at the end, add a compose action with the uriComponent() expression around your value (which we are getting from a separate HTTP call in this case), and add a second HTTP action.

However, my favorite thing to do after someone tells a really bad joke, aka a Dad Joke, is make a fart noise with my hand and mouth.  So, I shelled out the $2.50 a month to be able to add my own media to Voice Monkey and found the appropriate fart noise I was looking for on the internet.  

![Placeholder](/images/delivered-by-alexa8.png)

I first went back to Voice Monkey, clicked Assets - My Files, and uploaded the right squeaky fart noise I wanted.  I then went back to the API Playground in Voice Monkey and selected Media Library and selected my new file by clicking "My Files" and clicking the green checkmark.

![Placeholder](/images/delivered-by-alexa9.png)

Now in my API URL section on the right, I should have a new parameter (you may need to add announcement text first to generate what you need).  Copy everything you see starting with "&audio=" and ending with ".mp3".  This is what you will need to add to your HTTP call in Power Automate.

Now make another GET call and construct your URL the same way - use your environment variable for your token, make sure your device name is in there right, make sure that "&text=" is set to the output of your "Compose" action, and add "&audio=" and the URL you copied from Voice Monkey.  This is what my second HTTP action looks like:

![Placeholder](/images/delivered-by-alexa10.png)

If you did it right, you might be so lucky as to enjoy <a href="https://photos.app.goo.gl/wdTGr8BJCCTyPcSX6">something like this</a> now in your day.