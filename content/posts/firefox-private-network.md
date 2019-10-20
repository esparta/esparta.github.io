---
title: My Journey using Firefox Private Network
date: 2019-10-04T10:35:00-07:00
categories: [security]
tags: [firefox, opinion]
images: [https://static.esparta.co/images/posts/0002/ff_private_network.jpg]
slug: "0002"
description:
 Here's my Journey using Firefox Private Network, a kind of new Mozilla product
---

## Prelude

One day of September I learnt about [Firefox Private Network][ffpn], as one of
the [Firefox][firefox] fan
[I took the decision of use it for a while and also document][tuit_sept_2019]
what it is, and what are the good and bad things about it.
I will be taking different perspectives: as a user and also as an engineer who's
looking for a better and more secure internet.

![FIrefox Private Network Image][ffpn_hero]

This writing may have a lot of bias because my affinity to Mozilla, and my
neutral view about Cloudflare, Mozilla's partner in this project.

--

### So, What's this Firefox Private Network?

Mozilla says on the main page of [Firefox Private Network][ffpn]:

> Firefox Private Network is a desktop extension that helps secure and protect
> your connection everywhere you use Firefox

For a technical person, there's also a more [in dept definition][ffpn_help]:

> Firefox Private Network is an extension which provides a secure, encrypted
> tunnel to the web to protect your connection and your personal information
> anywhere you use Firefox

So, basically it's a proxy, and the [TOS][tos] confirms this:

> Firefox Private Network is a proxy service that helps you browse more privately
> by placing an intermediary between your Firefox browser and the internet to
> obfuscate your IP address. A Firefox Account is required.

The use case is also clear: It's a product delivery to you as a Firefox
extension meant to be used as an extra protection while you are browsing
the internet using Mozilla Firefox. The key features were described on [their
announcement][ffpn_ann]:

- Protection when in public WiFi access points
- Internet Protocol (IP) addresses are hidden so it’s harder to track you
- Toggle the switch on at any time

### What's NOT Firefox Private Network?

- It's not a [Virtual Private Network][vpn]: Since it's only works within Firefox
- It's not an Anonymizer system or protocol. Since at least two known actors
  will know bits of your traffic here and there \[it's [documented][privacy]
  [twice][privacy_cloudflare]\].

### Installing it.

The first thing to note after installing it is the new icon, and a big blue
button `Sign in`:
![after installing the extension][ffpn_after_install]

This is a hint on why is not anonymizer.

## First impresion: This is REALLY fast, y'all.

One of the complains about VPN and Proxies is the overhead and slowliness it adds to
the normal operation. This is NOT what it happens using Firefox Private Network.

So, How Fast it is? 1 - 1 FAST.
I mean, is literally as you are not using nothing at all. This is the report
done by [Speedtest.net][speedtest] on my network at work:

![ffpn stats from work][ffpn_stats_from_work_before]

And this is using Firefox Private Network from that same node.

![stats from work using FFPN][ffpn_stats_from_work_after]

As a table is probably better to see:

| -- | Ping --   | -- Download (Mbps) --  | -- Upload (Mbps) --  |
| ------------- |:-------:| :----------------:| :--------------:|
| Normal   | 3 ms    | 372.57  | 280.84  |
| FFPN     |  0 ms   | 361.12  |  281.82 |

## Location. Location. Where am I?

The first to notice is the change of the [IP address][IP], from my Level 3 network to
the Cloudflare Warp Network. That means we will probable have something cool
here: The extension will be using the closest node geographically speaking
(another hint on no-anonymized)

In order to prove the above point I ran another set of test when I was connected
in Japan[^1] and these were the results:

![ffpn from japan][ffpn_from_japan]

Can you see that? The connection was done from a close location, Tokyo to be
exact.

Maybe it was a coincidence, so later on I did the same from Netherlands [^2]:


![ffpn from nl][ffpn_from_nl]

The connection is now from Amsterdam, a location closest from my node now.

**Location conclusion**: Yes, it connects to the closets Cloudflare Warp node.
This can be a good thing or bad thing depending on who see it. For a user
perspective it probably doesn't matter, from others maybe it's a big thing (more
about that later). Also Firefox Private Network honors their promise: "Your IP
is hidden from everybody", so is difficult to be tracked because for the one
seeing your IP you are inside a Cloudflare network, the same as hundreds of
other users.

## It may block connection to sites

As part of the protection package, the extension may partially block a site
doing not so normal things to your browser. This appeared when I was checking
this website: https://am.i.mullvad.net


![Firefox PN blocking mullvad.net][ffpn_blocking_site]

In my opinion that's a nice thing to have in general, in other cases it will
be not a good thing like doing a Video Call. It's [documented][videocalls] you
may need to turn the extension off.

## About DNS

Cloudflare would be our connection to internet while using the extension, since
it's a tunnel (proxy) that would mean they are also serving our DNS. So I ran
a set of test to see how it works.

First, I ran a test about the [DNS Entropy][dnsentropy]:

![DNS Entropy][ffpn_dns_entropy]

:notbadobama:, since I was running the test multiple times I found something
kind of weird, or at least unexpected: the IP of the DNS was changing, almost
randomly.

So, I ran another series of test, this time using [DNS Leak Test][dnsleaktest]
with their extended version of their test, finding Firefox Private Network would
be using up to 18 different DNS servers per node, and was not a surprise that
those DNS servers were also close to the given location.

![DNS in JP][ffpn_dns_japan]

Of cuorse this technique is not new, the Cloudflare's [1.1.1.1][one] was also
doing this since day one. One of the reason is per security concerns: the one
checking on who's your DNS provider would only find that you browser is just
one of the millions of Cloudflare direct or inderect connections.

**DNS conclusions**: I was not able to find any concerns on what is doing
related to DNS, it's the same protection provided by Cloudflare since a long
time ago.

## Use and Abuse

We all know what's the first thing people would try to do: Use Firefox Private
Network as a way to break region locks.


![Use it only on USA][ffpn_only_in_usa]

It literally takes 2 minutes to figure out you can activate the extension even
when you are not physically on the United States of America [^3], and
after the activation you can practically use from everywhere as my screenshots
above shown. Or you can use a VPN to be in USA and then use the extension to
bypass streamers locks.

Since the main target of break-region-locks is to stream content, those people
will be using a VPN to _be_ in USA and then use the extension to bypass the
the streamer's locks [^4]. Tons of [bandwidth][bandwidth] would be using in that way
within the extension, the stream providers would probably not very happy about
it, having some consecuences: either they will block all Cloudflare IPs, or
Cloudflare will block connection/use? At this point I'm sure about exactly
what would be the case.

The above is not a technical problem, and will be difficult to solve with an
_Acceptable Use_ document (sorry, I didn't find it). Or maybe the extension
would need some kind of _practical limit_ (around 2 GB per month?) of data
transfered using the extension.

## Minimal problems

In all the time I was using Firefox Private Connection I had problems only once.
The extension did appear as disconnected and when I try to connect again it
was asking to login again, but the Firefox network was down and I have no
option but not use it that day.

![FFPN issue connecting][ffpn_issue_connecting]

Yes, I did try to turninf it off, and then on again the extension, even I did
the same with the computer. Still didn't work.

## Conclusion

After a week of using Firefox Private Network I would say it's good enough for
what is meant to be used: an extra protection while using a (free?) public
network you *should* **never** trust (as that open WiFi everybody uses).
Yes, you are having an intermediary in order to have that protection, but it's
way better than just connect and use as in nothing can happen.

[^1]: No, I did not travel to Japan, but that would be cool.
[^2]: I have never been in Netherlands, but that also would be cool.
[^3]: I'm well aware the TOS prohibits their use when you are not physically in USA.
[^4]: That's why we can't have nice thing. And is worst when are free.

[firefox]: https://mozilla.org/firefox
[tuit_sept_2019]: https://twitter.com/esparta/status/1174020190129901568
[ffpn]: https://private-network.firefox.com/
[ffpn_hero]: https://static.esparta.co/images/posts/0002/ff_private_network.jpg
[ffpn_after_install]: https://static.esparta.co/images/posts/0002/ff_private_network_02.jpg
[ffpn_stats_from_work_before]: https://static.esparta.co/images/posts/0002/ff_private_network_10.jpg
[ffpn_stats_from_work_after]: https://static.esparta.co/images/posts/0002/ff_private_network_07.jpg
[ffpn_from_japan]: https://static.esparta.co/images/posts/0002/ffpn_from_japan.jpg
[ffpn_from_nl]: https://static.esparta.co/images/posts/0002/ffpn_from_netherlands.jpg
[ffpn_blocking_site]: https://static.esparta.co/images/posts/0002/ffpn_protect_connection.jpg
[ffpn_dns_entropy]: https://static.esparta.co/images/posts/0002/ffpn_entropy.jpg
[ffpn_dns_japan]: https://static.esparta.co/images/posts/0002/ffpn_dns_japan.png
[ffpn_only_in_usa]: https://static.esparta.co/images/posts/0002/ffpn_only_usa.jpg
[ffpn_issue_connecting]: https://static.esparta.co/images/posts/0002/ffpn_cant_connect.jpg
[ffpn_ann]: https://blog.mozilla.org/blog/2019/09/10/firefoxs-test-pilot-program-returns-with-firefox-private-network-beta/
[uninstall]: https://support.mozilla.org/en-US/kb/how-installuninstall-firefox-private-network
[ffpn_help]: https://support.mozilla.org/en-US/kb/firefox-private-network-browse-securely-wifi
[vpn]: https://en.wikipedia.org/wiki/Virtual_private_network
[ip]: https://en.wikipedia.org/wiki/Internet_Protocol
[dnsleaktest]: https://www.dnsleaktest.com
[dnsentropy]: https://www.dns-oarc.net/oarc/services/dnsentropy
[one]: https://1.1.1.1
[bandwidth]: https://en.wikipedia.org/wiki/Bandwidth_(computing)
[speedtest]: https://speedtest.net
[tos]: https://www.mozilla.org/en-US/about/legal/terms/services/
[videocalls]: https://support.mozilla.org/en-US/kb/video-calls-fpn
[privacy]: https://www.mozilla.org/en-US/privacy/firefox-private-network/
[privacy_cloudflare]: https://www.cloudflare.com/mozilla/firefox-private-network-privacy-notice/
