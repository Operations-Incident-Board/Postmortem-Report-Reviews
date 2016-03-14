# Introducing a new series: Post-Mortem Book Reports

## Dear fellow systems engineers,

Take a moment and think about the past few years in systems outages and public post mortems. 

What were your favorite outages?  What are the post-mortems that you read that stick with you, months or years or years and years later?  What did you learn from them?

If you are in AWS us-east-1, you probably think back to the [Christmas Eve outage](https://aws.amazon.com/message/680587/) of [2012](http://techblog.netflix.com/2012/12/a-closer-look-at-christmas-eve-outage.html) or the long string of [EBS outages](https://aws.amazon.com/message/65648/).  If you were an early user of mongo sharding, I'm betting the [4sq mongo outage](https://web.archive.org/web/20110209190434/http://blog.foursquare.com/2010/10/05/so-that-was-a-bummer/) is etched into your brain.  If you run physical data centers and run [your own networking](https://github.com/blog/1364-downtime-last-saturday) or experience lots of [DDOS attempts](https://github.com/blog/1759-dns-outage-post-mortem), GitHub post mortems are probably high on your list.  

If you love post mortems in general, [Dan Luu's](https://github.com/danluu/post-mortems) collection probably warms your cold little heart.

Why do these outages stand out in our minds?  Possibly because the ripple effects had a big impact on your own systems, but more likely it's because you __learned something__ from the detailed, painstaking, revealing public post mortem that was published in the wake of the outage.

For me, the best incident retrospectives are the one that make me cringe and realize, _"that could have been me."_  The best ones are where I come away with an actionable list of things I have learned and can use to protect my own systems.

With this in mind, we are going to start publishing a series of book reports on both current and classic post-mortems.  These reviews are designed to highlight and recap some of the critical moments in our industry, so we can preserve the knowledge and collectively level up on our incident reporting skills.  Some questions we will attempt to address are:

* How did affect you?  Why does it stand out for you?
* What did you learn from it?
* What was said (or unsaid) that made the post mortem meaningful to you?
* Was something left unclear or dissatisfying?  What do you wish they had disclosed, or been able to disclose?

A couple of book reports are already up: see [@petey5k's piece on the recent GitHub outage](https://github.com/Operations-Incident-Board/Postmortem-Report-Reviews/blob/master/2016-01-28-pshima-github-outage.md), and [@gabinante's review of the classic 2011 EBS outage](https://github.com/Operations-Incident-Board/Postmortem-Report-Reviews/blob/master/2016-03-07-gabinante-AWS-EBS-2011.md).  

We appreciate the effort that everyone puts into post mortems and public retrospectives.  Writing retrospectives is hard!  Pitching the level of detail to the right audience is hard.  We hope that providing feedback on how your retrospectives were meaningful for us will give everyone more insight into how their efforts are valued and received.

Yours in triumph and in tragedy,

[charity.](https://github.com/charity)

P.S. Want to write a book report?  Please do!!  Check out the [template](https://github.com/Operations-Incident-Board/Postmortem-Report-Reviews) and send us a pull request.  :)


