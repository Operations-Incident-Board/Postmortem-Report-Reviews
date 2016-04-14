# Report: Netflix Christmas Eve Outage 2012
Link to Postmortem: http://techblog.netflix.com/2012/12/a-closer-look-at-christmas-eve-outage.html

## Company Overview
Netflix is a global streaming provider which instantly delivers TV and Movies to millions of subscribers, as well as operating a mail-in service that is on the decline as on-demand distribution picks up. They had 6.77 billion in revenue in 2015 and are one of the fastest growing content distributors in the world.

## Incident Overview
Netflix experienced a partial streaming outage which gradually grew in scope throughout the afternoon, affecting playback for many users in the Americas (but not in Europe). This was caused by an AWS incident which affected a portion of the Elastic Load Balancers used by Netflix's infrastructure. Netflix website usage was not impacted. 

## Incident Updates / Postmortem
The Netflix team basically disseminates the AWS postmortem, as they took no action themselves to remediate the issue. It's actually not apparent if the netflix team was aware of the outage while it was happening. The short version of the incident: At 12:24pm, a developer at AWS accidentally deleted a bunch of state data from production ELBs. This caused high error rates and API failures, but AWS operations staff did not understand the root cause for several hours. At 5PM, AWS had identified the issue but didn't reach resolution until 5:40AM on the following day, and didn't have every ELB back in service until 10:30AM. Interestingly, AWS report conflicts with Netflix here (Netflix's postmortem states that service was restored at 10:30PM on christmas eve, whereas AWS says 10:30AM on Christmas day. For the purposes of this report, I'm assuming AWS's account is correct.) This gives the impression that the Netflix team was not fully apprised of the impact that the AWS outage had on their systems. 

## Remediation / Future Work
The main callout that Netflix makes is the need to improve cross-region redundancy in their service. This is an initiative that they had already been investigating, but outages such as this highlight the need to build graceful failover between regions. The current state as of 2012 was two isolated regions in the Americas and Europe, which prevented this outage from affecting their European infrastructure. The future state that they propose is running netflix in multiple AWS regions, allowing them to route traffic around potential network issues.

## Additional thoughts
Although the impact to Netflix's service was caused by an AWS issue, they take responsibility and look for ways to prevent future occurrence. This is an interesting case because they actually took no action - they were impacted by the incident and had to wait for AWS to resolve the problem, but they still published a document detailing the problem and analyzed ways to improve their service.  It would be easy to blame this one on the vendor and note the steps that AWS is taking to prevent the issue (creating a change management process to prevent manual changes like the one which caused the outage), but they go a step further - realizing that they can't depend solely upon AWS for redundancy and uptime, they want to architect their service to be resilient even when their provider has an outage.

One thing that they did not call out was their monitoring. Were they notified of the outage? Was there something that their operations team could have done if they knew as soon as the issue occurred, such as taking affected ELBs out of service and commissioning new ones? AWS notes in their postmortem that newly provisioned ELBs were not affected by the issue, as it was caused by deleting of state data on the ELBs. It's interesting that Netflix didn't try to remediate the problem despite having a service degredation for up to 22 hours. It seems likely that their monitoring was insufficient and they weren't aware of the scope of the outage - or their staffing on Christmas eve was not sufficient to deal with an outage. Despite most of the blame likely lying with AWS in this instance, I feel that there is a gap in this postmortem - what action could Netflix have taken?


## Additional Reading:

Amazon's postmortem:
http://aws.amazon.com/message/680587/

## About the Author
My name is Gabe Abinante and I am an SRE at ClearSlide. You can reach me at gabe@abinante.com or @gabinante on twitter


###### \#partial-outage #degraded-service #infrastructure #technical
