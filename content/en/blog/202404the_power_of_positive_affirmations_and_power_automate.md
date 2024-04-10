<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>

---
title: "The Power of Positive Affirmations (and Power Automate)"
date: 2024-04-10T08:29:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["personal","how to", "power automate", "api", "power apps", "neurospicy"]
thumbnail: /images/PowerofPositiveAffirmations.png
hidePageThumbnail: true
toc: true

---

## My Recent Neurospicy Journey (TW: obsessive thoughts)

![Placeholder](/images/PowerofPositiveAffirmations.png)

It started when I was driving to my uncle's house on the highway to watch a Chiefs playoff game.  It probably started way earlier than that, but that's the first time I remember it happening.  I was driving, and suddenly I was stricken with the thought, "How do I know how to stay in the lanes of the highway?  What would happen if I drifted out of my lane?"  I was obsessed with this thought pattern, and I almost pulled over, it was such a strong feeling.  I didn't, and I made it to my uncle's house and carried on like normal, but the next time I drove on the highway, the same thing happened.  "What if I just veered into oncoming traffic?  What if I did that instead of just staying in my lane like I know how to do, except do I really know how to do that?"  However, I wanted to watch the Chiefs keep winning playoff games, and my superstition about needing to watch each game at the same place beat out these new intrusive obsessions I was having.  I was unknowningly treating myself by virtue of being a single woman who just has to do crap on my own if I want to do it.

Other thoughts like this were starting to invade my brain - on the days I went into the office, in the afternoons, I started to obsess over the thought that my house might be on fire and my cats were dead inside.  I would check the cameras I had, see that everything was fine, but then convince myself that the fire or harm was just out of the camera's lens.  I began to worry about even walking around my house because Charlie, my charming tuxedo cat, loves to be underfoot and I began to obsess over the thought of accidentally stepping on him and breaking one of his legs.

It came to a head one really bad night that I won't go into detail about here, but suffice it to say, I knew I needed to get help.  I had been seeing a therapist already, a great short-term benefit offered by my employer, and she was on-site to boot.  Betweeen getting some of these worries out of me and talking to my long-time psychiatrist, she suggested that I might have OCD and referred me to an OCD specialist.  Within the first meeting with this new therapist, she quickily diagnosed me with something I'd never heard of before: "harm OCD".

I won't pretend to be an expert, but <a href="https://www.treatmyocd.com/blog/what-is-harm-ocd-guide-to-ocd-subtype">this website</a> was the first one I found after getting out of my appointment, and I felt immediate relief upon reading just the first paragraph.  Other people had these types of obsessions and intrusive thoughts, too!  It wasn't just me!  And not only was it not just me, but we were actually less likely to reach out and do any of the things we were obsessing about than the average person!  Not only that, but there was a way to treat it!  A way that totally, totally sucks, but a way nonetheless.

If you didn't know, the standard treatment plan for OCD is exposure therapy - making you sit with those uncomfortable thoughts and pushing through them.  I've begun using the metaphor of a toddler throwing a tantrum.  If you give them candy to calm them down (aka comforting thoughts), they'll calm down, but they'll do it again and maybe worse the next time.  If you ignore it and let them throw their tantrum, they'll eventually calm down on their own.

## So What Does This Have to do with Power Automate?

When you have obsessive, intrusive thoughts on repeat in your brain, it's hard to know what else to think about.  I said, "Well, I try to remind myself that the cats are okay and safe probably" and my new OCD therapist informed me that I was doing the exact wrong thing - I was giving the toddler candy for their tantrum.  I wasn't allowed to comfort myself, but she did say I was allowed to repeat positive affirmations to myself, and she gave me a few examples that I wrote down, "Keep doing this", "You can do hard things", "It will be worth it", and "It will pay off".

I was already cooking up something special related to the rest of my neurospicy adventures, so I thought, why not see what I can do to help me feel better using Power Automate?

## The Positive Affirmation Email

I had to give the trigger some thought.  Even though it looks like changes to this are rolling out now, a recurrence trigger at the time wasn't going to work because I didn't necessarily need hourly affirmations all day, every day.  The specific time I was struggling with the most was in the afternoons just on the days I went into the office, so a little too specific for a recurrence trigger.  I ended up building a Power App with some additional secret functionality (secret for now, don't worry), and went with a PowerApps (V2) manual trigger.  I added an input of "Email Address" so tht other people could use the Power App, but it's easy enough to hard-code the email value.  You could easily just use the "manual" trigger if you don't want to use Power Apps at all and just run this flow from Power Automate whenever you need it - totally up to you how fancy you want to get.

For the positive affirmations, I found "inspirational quote" APIs that could do the job if you want to cut down a little on the manual labor part (https://zenquotes.io/ was a great one I found).  So, I did some searching and found a <a href="https://www.happierhuman.com/affirmations-intrusive-thoughts/">mega-list of positive affirmations</a> for people with OCD (your mileage may vary, the content can be whatever works for you here).  I then made a SharePoint list and just started putting in my favorites.  To my flow, I simply added a "Get items" SharePoint action and pointed it to my new list I'd made.

Next, I  use a "Compose" action, with the formula borrowed so thankfully from <a href="https://powerusers.microsoft.com/t5/Power-Automate-Cookbook/Get-a-random-item-from-a-SharePoint-list/td-p/462817">Rob Elliott</a>.  This formula gets a random value from my "Get items" action:

```html
body('Get_items')?['value'][rand(1,length(body('Get_items')?['value']))]
````

Run your flow once to get all of that lovely JSON data from the "Compose" action.  It should look something like this:

![Placeholder](/images/the_power_of_positive_affirmations_and_power_automate1.png)

Copy the output of your "Compose" action and add a new action - "Parse JSON".  This one allows us to fetch exactly what we want from this "Compose" JSON data without having to write another expression.  You'll need to add the schema, which you can get by clicking "Use sample payload to generate schema" and pasting in the output of your "Compose" action.  Then, you can add the "Compose" output to the "Content" field of the "Parse JSON" action.  It will end up looking like this:

![Placeholder](/images/the_power_of_positive_affirmations_and_power_automate2.png)

I did not type in all that fancy schema, remember - I just pressed that "Generate from sample" button and pasted in the outputs of my "Compose" action's run, and it generates it for us!  Now we will have all sorts of new dynamic content to choose from for our last action, "Send an email (V2)".

If you ended up using an input to get the email address from Power Apps, you would put that dynamic content in for the "To" field.  If you just want to hard-code, meaning just type in your own email address because you are the only one using it, just type it into the "To" field as a custom value.

For the subject field, put whatever you want.  The body can also be very simple - all I put in was my dynamic content for my positive affirmation value, which I just left in the "Title" field.  So my "Send an email (V2)" action looks like this:

![Placeholder](/images/the_power_of_positive_affirmations_and_power_automate3.png)

Then, when I press a button in my Power App, I now get as many emails just like this, but with random positive affirmations each time, as I need in an afternoon:

![Placeholder](/images/the_power_of_positive_affirmations_and_power_automate4.png)

Then, as I'm rocking back and forth with my emotional support squishmallow in my corner cube, I have something that can calm me down a bit as I repeat it to myself - "I aim for progress, not perfection", and every day that I make it until 5 without checking my cameras or going home early is progress.

## Conclusion

I hope this helps someone else out there who might be struggling with similar thoughts.  I'm not a doctor, but I am a Power Platform enthusiast, and I'm happy to help you out if you need it.  Just reach out to me on Twitter or LinkedIn, and I'll do my best to help you out.

And keep your eyes peeled, because this isn't the only thing I've been cooking up lately to help those with neurospicy brains.  I've got more than a few more tricks up my sleeve (and my partner in crime's sleeve) that I can't wait to share with you all, but I couldn't wait to share this one.  I hope it helps you as much as it's helped me.

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>