---
title: "Connections in Power Automate (and How to Take Control of Them)"
date: 2023-02-23T14:49:05-05:00
draft: true
author: "Lindsay Shelton"
tags: ["connections","power automate"]
thumbnail: /images/connections-in-power-automate1.png
hidePageThumbnail: true
toc: true

---

Do you have a flow running, proud as can be, except… the emails it sends out says they are from you? Did you get help on a flow and now it says that the SharePoint library is being updated by your coworker?

All of this is due to *~*connections*~* in Power Automate. I’m going to attempt to quickly introduce them, go over how to change and then remove them, and explain a best practice with connections.

## What Are Connections?
So, every time you make a flow in Power Automate and you create a new category of action, like a SharePoint action or an Outlook action, you create a new connection. That is, a connection from your email address to gain access to that resource. This is good and great. If I’m making a flow, I expect it to be able to do the things I’m able to do, and connections allow for this.

The way to see who owns a connection on an action is to click the three dots next to an action and look at the bottom

![Placeholder](/images/connections-in-power-automate1.png)

In this screenshot, you can see that for my Send an email (V2) action, I have… well, a lot of ls2527 connections. I need to clean some up. That doesn’t really matter – what matters is that I know the connection is mine and therefore I know that the email will come from me.

If I were to click at the bottom, “+ Add new connection”, I get a prompt to sign into my Microsoft account, and now I have 12 million instead of 11 million ls2527 connection options. Oops.

This is a useful function, and we will get back to why. It’s just not that useful if it’s a flow that you alone built and are using.

## But What If It’s a Shared Flow?
Ah yes, now we are getting into the good questions. What if it is a shared flow? Well for one, we have some discussion.

- Who should “own” the connections in the flow? The original creator? Me? A service account? (The answer is ‘a service account’ whenever possible, but we will get back to that)
- How do I change who owns what connections?
- How do I keep the person I’ve shared a flow with from using my connections with malicious intent?

The first question is really one that you have to decide amongst yourselves (but really, get a service account if you can). The other two, we can handle.

Let’s say that my coworker, has shared a flow with me, but I’m now meant to take ownership of this flow and the connections. We already went over how you can change the connection – click “+ Add new connection” at the bottom, add your own connection, and check your email address instead of theirs.

![Placeholder](/images/connections-in-power-automate2.png)

So in this screenshot, this is a flow that someone else, “Red”, built. All I need to do is either click “+ Add new connection” if I don’t see my name, or click one of the twelve occurrences of my email address to change it to mine.

DON’T STOP THERE! Go back to the main flow info screen. In the top right corner, there’s a box that says “Connections”. Click “Edit” and go take a look.

![Placeholder](/images/connections-in-power-automate3.png)

When you are in the edit screen, you will see the flow’s Owners at the top. Don’t worry about that for our purposes here. Go to the “Embedded connections” section.

![Placeholder](/images/connections-in-power-automate4.png)

Let’s say that this was a flow I was giving to another coworker, “Red”. Well, I’ve clearly missed a flow action, because you can see that my email is listed as one of the connections, in addition to her own connections. I could just click the “x” and get rid of it, but that then breaks the flow. Really, I want her to go in and change the connections on all actions to her username.

THEN, don’t forget this last step! There will be a separate section at the bottom that says something like “Connections not actively in use”. You will want to remove your connection from there so that it can’t be used in the flow. Then you’ve fully passed it off!

## Best Case Scenario
The best case scenario is to get your flows running on at least one service account. At least in my org, our build process is:

- build the flow in my account, test, etc.
- share the flow with the service account
- move all connections over to the service account
- remove my connections from the “connections not actively in use” screen

And voila! You have emails and notifications coming from a nice, generic account, and your name is off of everything. You can still remain a shared owner of the flow, even if your name isn’t on any of the connections.