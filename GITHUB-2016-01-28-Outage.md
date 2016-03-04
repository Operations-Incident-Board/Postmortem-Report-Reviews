# Report: Github Outage on 2016/01/28

## Company Overview
Github is an incredibly popular and widely used social coding service with customers that range from a single person hobbyist to large enterprise.  Customers can store and share their source code using their services.  A large number of users and/or companies rely on Github as their primary code repository and in some cases, the only central repository for source code.  Because of this, Github's availability is critically important for operations for these users or businesses.  Being unable to make changes to your source code, or run builds, or deploy, or other functions that require access or mutations to source code can be devastating for developer or production operations.  Some companies or users come to a complete halt during these issues with no backup plan, again illustrating the importance of availability of the services.

Github sets an example for others on displaying public availability through their status site: https://status.github.com.  This has significantly more information than other service providers and gives you a good idea about how the service is operating, but beyond that there is no hiding behind "invisible" downtime issues.  The transparency here increases customer confidence.  The availability of the service and operations are good enough to display to customers 24 hours a day, 7 days a week.  

## Customer Experience
The customer experience during this outage was a full outage of github.com and associated services.  Users browsing to the site would experience a page displaying a unicorn.  Github was known for using unicorn internally (https://github.com/blog/517-unicorn) and while the error message was confusing for some, it reflected some of the technical aspects of their architecture and undertones of the company culture.  The 503 page can still be viewed here: https://github.com/503.html.  

As per the first update on the issue, the start of the event was January 28th, 2016, 00:23 UTC.  Communication to customer's was done through primarily the status.github.com site and also through twitter.  The status site was updated at 00:32 UTC, 9 minutes after the initial outage time stating: "We're investigating connectivity problems on github.com"  This update reflected reality of the user experience.  14 minutes later on the status site the outage was acknowledged as a significant network disruption effecting all services.  There was a small grammatical update on 'effecting' vs 'affecting' which was corrected in future posts beyond this point.  While subtle, this leads us to believe that a human typed this post over an automated system and it was done during the incident, and was quickly corrected.  This is merely speculation on how the status update was constructed.

Github did a great job on continued updates out to customers during the outage and immediately following the first post.  Updates were done near a 15 minute interval or better until recovery started.  Despite this, near the end of the second update there is emphasis that communication could be better.  This is likely in further detail on the issue other than network issues, but even the continual updates helped customers understand status.

## First Update

Github updated its customers the day following the incident with a public blog post: https://github.com/blog/2101-update-on-1-28-service-outage.  The immediate update following the event is important.  It acknowledged that there was a major outage, and it extended its apologies for the outage recognizing how important the service is for users.

Beyond acknowledgement of the issues, some high level details are also shared about the outage.  This is important for several reasons.  First, this shows that Github understands what happened at a high level.  This helps customers gain trust in Github and their ability to respond to complex issues that cause downtime.  Second, related to the first, this gives customers a piece of mind that things are under control and service is stable.  If the update here was there was no idea what happened it would further increase panic for customers.

As the last part of the update it is also again noted how important availability of the services are and that more details will be shared from the investigations.  While short in detail, this is also important.  It gives confidence that multiple parties are taking a close look at what happened and there continues to be active work on improving reliability of the service if this is to reoccur.

The post itself is short and to the point.  For a first update this is more than sufficient and gains trust from customers that although there was an outage, it is under control and there are people engaged on looking very closely at it. More information will be shared with me, the customer, following that.  With the transparency seen on the public status site, this falls in line with the level of transparency.

The unspoken bits on this post are that it is described in a way that anyone can understand or relate to.  This is important for a level of comprehension.  People can relate to power outages happening and causing disruption and this is an important detail in the post for human understanding.  This again leads the user or customer to a point of empathy, everyone has had that time where the power went out at home or at work.

Hats off to the Github team for this early update.  Many do not view outages as an opportunity to gain customer trust despite the circumstances, but this illustrates how this can occur.  Outages happen; help customers understand that you know what you are doing and are able to react quickly.

## Second Update

The second update to customers was on February 3rd, 2016.  This is 3 business days following the outage.  Reading between the lines this would suggest this was a pretty serious event in which the investigation lasted several days with review of the incident report also taking considerable review.  This is speculation about internals on Github procedure.  This also shows the update was important for Github and for its customers, responding in just a few days following the event likely required many parties involvement in the investigation.

The post starts out emphasizing that they understand how important availability is for their customers, and that they are truly sorry about the outage.  The paragraph is written in a way that is very human.  In reading it I feel like the author truly is sorry about this and it is not some canned response.

I want to take a moment to highlight an important formula I have found helpful in post mortems which I see reflected.  Mark Imbriaco highlighted this in a tweet and in a blog post: https://twitter.com/markimbriaco/status/623146854084124672, https://www.digitalocean.com/company/blog/inside-digitalocean-mark-imbriaco/

To quote:

1. Apologize for what happened
1. Demonstrate you understand what happened.
1. Explain what you will do to reduce the likelihood of it happening again.

In this second update and in the first we see something similar to this formula being used starting out with a very human apology.  There is a missing overarching theme that needs to apply to post mortems - and that is that the update should give your customers confidence.  This is an opportunity for your company to shine during adversity.  The updates should also generate as little questions on more detail and this comes back to comprehension.  Display the information in a way that can be understood by people unfamiliar with the infrastructure or architecture.  This is not easy.

As we move on to the understanding of what happened during this event more is described about the power outage.  The detail shared here is that just over 25% of the the Github stack and several network devices rebooted as a result of the the power issues.  Load balancing equipment and "a large number of our frontend application servers" were unaffected.  There is some ambiguity here, the problem seems to be manifested by back-end systems as the front end was largely unaffected according to this information.  But it seems bizarre that 25% or less of back ends could cause a full outage.  The previous post indicates a cascading failure, was that caused by a consistent hash ring or sharding that then took enough capacity out of service to overload the whole fleet?  Was it a specific back-end service or multiple?  The specific details may not be necessary as part of this outage but it is hard for me to comprehend exactly what the technical issue was in this case.  Was it probability that 25% of total servers that lost power contained 50% or greater of a single critical service?

The second paragraph on the event gives more detail that is relevant to these questions.  The ChatOps systems were on services that had power issues which is used to manage these events and it also addresses why the initial status dashboard post was eight minutes after the site became inaccessible.  There is some important detail in this that gives me confidence as a customer.  First, that they were prepared to respond to a major outage and second that 8 minutes was considered unacceptably slow.  Again hats off to Github on this.

The next few paragraphs talk about how they began to troubleshoot using Redis as a leading indicator of problems.  They followed standard procedure that it may be a DDoS and engaged their DDoS shields.  I can only imagine Github is a high target for DDoS attacks.  This also gives some interesting insight into how troubleshooting was going during the incident.  Redis was having issues which was a leading indicator of DDoS, but then someone noticed that "our application processes were not starting up as expected".  This suggests multiple avenues were being explored during the outage, and that why the DDoS operation was common and executed without issue, another party was investigating application issues. 

After the Redis connectivity issues were discovered the post notes the core of the issue, that the redis cluster was offline and the cause of that was hosts not coming back online after the power outage because physical disk drives were no longer being recognized.  Immediately after this it notes that one group of engineers split off to work on the drive issues.  A small note here that reiterates multiple groups were engaged and operating on different resolution paths.  This level of orchestration to problem solving is also not easy under a situation where your entire service may be down.  There is a hint of incident resolution sophistication throughout this post, obviously not Githubs first outage rodeo.

The rest of the event section talks about using multiple avenues to recovering the downed systems.  The team could have waited for the group to recover the hosts with the unrecognized disks, but pursued multiple avenues of resolution by bring up new hosts and restoring data.  Monitoring was also in place to immediately know of recovery, suggesting that this monitor was firing when the incident occurred but that there may have been so many alarms going off that it was hard to determine the dependency tree through this alerting.  This is not uncommon and dependency tree alerting particularly in a full outage can be very challenging to get a clear picture of the issue.  This is again speculation on what internal alerting was like during the event.

## Unwritten Notes

There are several things that could be inferred from this post.  I want to reiterate that this is only speculation from reading the post.

- Power outages involving more than a few hosts are uncommon for Github infrastructure.  I imagine if these power outages happened weekly or daily the issue would have been easier to diagnose and further mitigations would have been in place.  
- Network diagnostics are hard.  With network devices rebooting, hosts being offline and the application logs saying there were connectivity errors to Redis this would have been difficult to diagnose.
- A dependency tree had not been looked at deeply in some time.  Redis as a core dependency for the service comes off as a surprise in the post.
- A "cold" service boot had not been tested in some time.  This may have identified Redis as a boot dependency but again that is speculation.
- DDoS can manifest itself in irregular ways.  At a sign of networking issues engage DDoS protection.  This is probably always a good first step if it is quick, but leads the reader to wonder more about border versus internal network semantics at Github.  If internal service A cannot connect to internal service B, how does external DDoS impact that network path?  This is a complicated issue and is a deep question without further understanding of internal network topology.
- Internal core services were having some issues over the period due to the power outages (ChatOps, perhaps provisioning)

## Future Work

Future work first talks about complex systems and that the necessary work has been done to identify weaknesses in the dependency tree.  It again talks about the cascading failure that led to the outage, but the information above does not reference the cascading failure, only a complete failure due to the power outage.  As a reader this leads me to wonder was the redis cluster just offline, or was there a cascading failure due to capacity constraints from the existing hosts being unable to boot after the outage.  Could it have been something different altogether or just a different use of the "cascading" term?

The disk identification issue is called out specifically as a firmware issue with the mitigation of opening issues for the team when new firmware revisions are available with change logs.  For me this does not close the loop on the issue.  It talks about forcing staff to review changelogs, but does not go into testing force reboots of the system to ensure that systems reboot properly, or fixing the issue at a higher level than at a single host.  These are only suggestions based on reading of the post without knowing more about internal operations or procedures.  Perhaps filing an issue will prevent reoccurence.

The next paragraph goes over testing service dependency.  This contains good detail on some actions they will take to ensure there are not surprise dependencies on application boots.

The next point may be trying to address the system reboot but it is slightly vague.  It talks about reviewing availability requirements of internal systems and that systems must be available for internal operation.  There is a subtleness here suggesting that the host provisioning system was having issues over the period and they must be hardened for outages such as this.  As a reader, I don't know how to interpret this.  What problems did the provisioning issue cause?  Did it delay recovery?  By how much?  Was it the provisioning of Redis servers as part of the recovery steps?

The last paragraph on future work covers procedural issues such as escalation strategies for a full outage moment.  This suggests that outages of this level occur rarely at Github which is slightly refreshing.  It is incredibly difficult to get good at something that does not happen very often.  The better systems get, or the more the company grows the harder it can be to respond to unexpected circumstances.

## Closing

I want to congratulate Github on this public post mortem.  As a customer it gives me confidence that reliability at Github is taken very seriously and work is in progress to ensure that events like this, if they do happen again, cause little or no customer impact.  There were plenty of opportunities for things to go wrong during the event or through the post mortem material and this should set an example for others on how to communicate understanding and confidence in running large scale, business critical services.

Also I want to note the level of transparency that Github provides is above and beyond what some would consider acceptable for such a large scale critical service and while the post opens itself for some questions, as a reader I have a basic understanding of what occurred and how it will be corrected and I have confidence that this incident will be handled better in the future.  This as the core points of the post mortem make this an incredibly successful example.

## About the author of this review

My name is Pete Shima and I am an SRE at HashiCorp.
me@peteshima.com - petey5k@twitter
