---
author: pank4j
comments: true
date: 2013-11-03 16:38:07+00:00
layout: post
slug: dirty-harry-hacking-for-free-drinks-in-singapore
title: 'Dirty Harry: Hacking for free drinks in Singapore'
description: 'Hacking Appy Hour iOS app to win free drinks'
wordpress_id: 358
categories:
- iOS
- Web Application Security
tags:
- Appy Hour
- free drinks
- Harry's bar
- iOS
---

[Harry's bar](http://harrys.com.sg/) is one of the most popular bar chains in Singapore. It has an iOS app "Appy Hour" that lets users spin the Harry's "wheel of fortune" to win free drinks. The way it works is that a user would visit one of their strategically located bars and check in using the app. He would then spin the wheel, and if he is lucky, he would win a free drink. A time limit is imposed so that a user can spin the wheel only once in 24 hours while signed in a particular bar.

Now there may not be any such thing as a free lunch, but there are free drinks! Let's see how.

<center><img src="/public/win240x360.png" /></center>
<p></p>

## The 'luck factor'




<cite>"... you've got to ask yourself one question: "Do I feel lucky?" Well, do ya, punk?"</cite>


The app communicates with ``www.exhost.se`` over HTTP. When started, the first thing the app does is to define the "luck factor" of the user, by downloading an XML file containing probabilities of various prizes. The probabilities are set in way such that 52% of the time a spin would yield 'Better Luck Next Time'.

{% gist 1fd3fb43e9c7ed639b52 %}


## Time Restriction

Once the user has checked in the bar, the app checks if the user has already tried his luck in the past 24 hours in the same bar. It does so by sending a request with the iPhone's UDID, a timestamp and the bar's id. The response simply consists of ``true`` if the user is allowed to spin the wheel, or ``false`` otherwise.

{% gist 7af63a6d30a1ac868c65 %}


## Getting past everything

Well, the simplest way to get past everything would be to make the app communicate with another server instead of ``www.exhost.se``. First we will crawl ``www.exhost.se`` to get all the required files. The link ``http://www.exhost.se/harrys_1_1_beta/`` does not contain a default ``index.html`` or similar file, which makes crawling possible.

{% gist bbb9df9ffc9032ac418f %}

{% gist c326edb435bee1dfca4a %}

Once we have the crawled files, we can edit the ``venues.xml`` file to change the probabilities. We can even change the drinks! ;-) Also, the file ``fetchdata.php`` should contain ``true`` for us to spin any number of times. Now these files can be hosted on a different server and these URLs can be changed in the app mach-o binary. Changing only the above two URLs is sufficient to make it work without any restrictions. The strings utility comes handy when trying to find out the location of these strings in the binary. These strings can now be edited using a hex editor. 

![Strings as seen in the disassembler after modification](/public/strings.png)

Once edited, we can run the app any number of times while signed in the same bar, and win a drink on every spin!

Cheers!


