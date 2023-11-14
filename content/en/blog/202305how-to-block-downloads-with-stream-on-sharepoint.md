---
title: "How To: Block Downloads with Stream on SharePoint"
date: 2023-05-24T14:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["stream classic","how to","stream on sharepoint", "sharepoint"]
thumbnail: /images/how-to-block-downloads-with-stream-on-sharepoint11.png
hidePageThumbnail: true
toc: true

---

Anyone else stuck in a slow migration nightmare? That may be a tad dramatic, but…

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint1.jpg)

I gotta be me, y’know?

Anyway, a big hurdle came up in our Stream Classic migration with a user who had a really good question. How can we block downloads with this new-fangled Stream option? It was a setting within Stream Classic that was being used by this department extenstively, and it was actually a legal requirement. No pressure, right?

Luckily, the great John Moore came in clutch with the answer (check out <a href="https://twitter.com/john_moore">his Twitter</a> and <a href="https://t.co/mClkS3tu4n">his YouTube</a>! He’s a fellow Kansas Citian too! Check out <a href="https://kco365.wordpress.com/past-events/">his session</a> from the KCM365 User Group about Stream Migrations!) He also so kindly gave his permission to let me whip up a quick blog on how to do this! Me being me, I’m going to make it longer than strictly necessary, but the short answer?

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint2.jpg)

So, the short answer is – use restricted view permissions. Here goes the longer answer…

## Setting Up a Dedicated Document Library
As I’m sure is not unique, our sites tend to not have the simplest permissions, and I didn’t want to screw up what was in place anyway for this express purpose. So, for the purposes of this, I created a dedicated Stream Videos document library.

1. Go to your SharePoint site.
2. Click +New
3. Click Document library to make a new one.

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint3.png)

4. Give your document library a descriptive name, decide whether to show it in the site navigation or not, and click Create.

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint4.png)

A side note – it’s considered best practice when making a new list or library to leave any spaces or special characters out of the name, so as to create a clean URL. Then you can go back and edit the name very easily, just by clicking the gear in the top right corner and clicking Library settings.

5. Click on the gear in the top right-hand corner of your new, empty document library.
6. Click “Library settings”.
7. Click “More library settings” (after potentially fixing your library name).

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint5.png)

Now you are in an older part of SharePoint. Don’t be scared! Just click on “Permissions for this document library”.

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint6.png)

9. You might have some stuff on your site by default, whatever your admins (or you) set up. We don’t care about that stuff right now, because we are going to live dangerously… and STOP INHERITING PERMISSIONS.

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint7.jpg)

10. It’s fine, really. Click that button that says “Stop Inheriting Permissions”. This simply means we can change the permissions on this document library WITHOUT breaking the permissions for the rest of the site. Neat, right?

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint8.png)

11. What you do from here is unique for each site and each use case. The main thing you need to accomplish is: the people who shouldn’t be able to download the video need to be given the Restricted View permission type.

It might look something like this when you click the Grant Permissions button:

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint9.png)

Use whatever groupings make sense. For me, I wanted to keep my original groups intact (which already included Everyone except external users in one of the groups), so I just changed the permissions from Edit and Read (Members and Visitors respectively) to Restricted View and… Restricted View. Again, this doesn’t mean that Members can no longer Edit other things on the site – this is JUST for our Stream Videos document library.

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint10.png)

And that should be it! Test it with another user, but it worked like a charm for mine, and then we just started migrating the videos as we pleased into this specific document library.

For those curious, here’s what it looks like when someone with Restricted View permissions looks at a Stream video:

![Placeholder](/images/how-to-block-downloads-with-stream-on-sharepoint11.png)

So there are just simply options missing, but the experience is still intact and your users will likely not realize what they are missing out on!

Thanks again to John Moore for coming through with that initial tip!

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>