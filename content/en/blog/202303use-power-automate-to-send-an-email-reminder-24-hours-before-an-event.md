---
title: "Use Power Automate to Send an Email Reminder 24 Hours Before an Event"
date: 2023-03-27T14:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["onenote","power automate"]
thumbnail: /images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event1.png
hidePageThumbnail: true
toc: true

---

![Placeholder](/images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event1.png)

I couldn’t believe this wasn’t a template already when I searched, so I figured – blog post it is! Two in a month, I did it again!

## The Use Case
This one is pretty simple. A user reached out and said she had a idea for how to use Power Automate, but she wasn’t sure how to make it happen. This immediately spikes my adrenaline, and I start searching triggers and actions right away.

Basically, when she had a group event coming up, she was typically making herself a calendar event 24 hours in advance to remind people, but wouldn’t it be nice if Power Automate could just remind people for her, like EventBrite does? Yes, yes that would be nice, I said, and away we went!

## The Flow
This might actually be a short one for me. The flow is only 4 blocks long, so how long could I possibly ramble for?

![Placeholder](/images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event.gif)

Anyway, the tricky part for me was (I was wrong, in hindsight) figuring out the trigger. Because this was a group event, we were going to go with a different trigger, but the calendar ID on an Office 365 Outlook trigger that did something very similar was giving me headaches, so we went with the O365 Groups trigger “When there is a new event”. All you need for that is the group ID, which you can fetch from Azure AD.

Once the trigger was in place, I came across my next hurdle – this trigger returns an improperly formatted date/time for the end action I wanted – send an email (V2).

![Placeholder](/images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event2.png)

So, I actually used the formula wizard to help me with getting a formula that worked for me, and I put it in a compose block for easier use in my next block.

```html
formatDateTime(parseDateTime(triggerOutputs()?['body/start/dateTime']), 'yyyy''-''MM''-''dd''T''HH'':''mm'':''ss''Z''', 'en-US')
````

Is all of that really necessary? Heck if I know, but it works and that’s what matters!

So, once I have my properly formatted (it looked fine before, who knows) date and time, it’s time to bring out the Delay Until block. Here I needed to use another formula, so it was easier for me to be able to refer to already formatted date and time.

For this use case, we want it to email 24 hours in advance, so I used addDays -1. Feel free to adapt to the time period you want, just with playing with that integer of -1. No idea if partials work (.5, etc).

```html
formatDateTime(addDays(outputs('Compose_formatted_datetime'),-1),'yyyy-MM-ddTHH:mm:ssZ','en-US')
````

I renamed my Compose block to “Compose formatted datetime” if you are like me and hate having to change things to work for your own formulas.

I was getting lots of wrong results before I made the hours capitalized and added the locale at the end (have I mentioned on this blog lately how much I hate working with calendaring?), but finally we got a reliable result.

Last block was whatever you want to happen at the 24 hour mark! In my use case, it was Send an email (v2).

Final flow looks like this:

![Placeholder](/images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event3.png)

![Placeholder](/images/use-power-automate-to-send-an-email-reminder-24-hours-before-an-event4.jpg)

It’s short for me, okay? I’ll go pet you now, Charlie (yes that’s one of my kittens).