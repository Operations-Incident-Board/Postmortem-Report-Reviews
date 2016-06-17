# Report: Google Compute Engine Incident #16007: Connectivity issues in all regions
Link to Postmortem: https://status.cloud.google.com/incident/compute/16007?post-mortem

## Company Overview
Google is Google.  You may have heard of them.  Google Compute Engine is an IaaS offering virtual machines with per-minute billing.

## Incident Overview
Google Compute Engine instances suffered increasingly higher latency in internet traffic, ultimately culminating in an 18-minute total internet connectivity outage.  Edge routers in Google's network began to stop sending BGP announcements for all of GCE's IP blocks.  As the incorrect configuration spread to more edge routers, traffic began taking increasingly less-optimal routes until finally no GCE IP blocks were announced by any edge router.

This incident is especially interesting because it was caused by a combination of three bugs in Google's networking management software, at least one of which was previously unknown.  The first bug caused a valid configuration to be rejected, and the second bug caused the system to respond to that rejection by removing *all* GCE IP blocks from the configuration.  The third bug caused the canary of this erroneous configuration to be accepted despite the failure.  Subsequently, the system slowly deployed the erroneous configuration to edge routers, causing the progressive impact.

## Incident Updates / Postmortem

### Updates During the Incident
Google initially posted that they were investigating an issue with Cloud VPN in asia-east1.  Subsequently, Google reported "severe network connectivity issues in all regions", followed fairly quickly by an update indicating resolution of the issue.

### The postmortem
About 36 hours after the incident, Google released a lengthy postmortem.  It explains BGP at a high level and details the confluence of bugs that resulted in the outage.  Google also apologizes multiple times and offers a blanket service credit of 10% off GCE (vm) charges and 25% off VPN charges for the month.  

### Timeline

Given the updates and the times mentioned in the postmortem, we can assemble this timeline:

Time (Pacific) | Description
--------------:|------------
14:50          | Benign configuration change released by GCE engineers.
18:14          | Postmortem states that Cloud VPN impact started now.
18:25          | Status post reports that Cloud VPN impact started now.
18:51          | Initial status post reports issue with Cloud VPN in asia-east1.
19:00          | Second status post states impact started at 18:25.
19:07          | Last edge router stops announcing GCE IP blocks.
19:08          | Internal alerts generated due to significant traffic loss.
19:09          | Engineers responding to asia-east1 issue reverted the network configuration change.
19:21          | Status post indicates severe network connectivity in all regions.
19:27          | Impact resolved as per subsequent status post.  Postmortem agrees.
19:45          | Status posting at this time indicates impact resolved at 19:27.

From this timeline, we can see that impact started earlier than the status posting indicated.  The postmortem indicates that engineers began responding to the asia-east1 issues at 18:14, but the first status post was made at 18:51, a lag of **37 minutes**.  Additionally, there was a lag of **13 minutes** between the time that engineers became aware of total connectivity failure and an update to that effect was posted.

## Remediation / Future Work

In the postmortem, Google stated that they had identified 14 distinct remediations in prevention, detection, and mitigation.  They also indicated that they are seeking review by engineers elsewhere in Google that may yield further remediation items.  Three of the 14 remediation items were explicitly listed in the postmortem.

## Additional thoughts

This is the best postmortem I've seen in a long time.  It starts and ends with sincere apologies that leave me feeling that Google truly understands and regrets the impact caused by the incident.  This is underscored by the following passage:

> [...] we are offering GCE and VPN service credits to all impacted GCP applications equal to (respectively) 10% and 25% of their monthly charges for GCE and VPN. These credits exceed what we promise in the Compute Engine Service Level Agreement (https://cloud.google.com/compute/sla) or Cloud VPN Service Level Agreement (https://cloud.google.com/vpn/sla), but are in keeping with the spirit of those SLAs and our ongoing intention to provide a highly-available Google Cloud product suite to all our customers.

The postmortem gives a concise, detailed explanation of impact with exact times.  It explains just enough about BGP for an unknowledgeable reader to understand the root cause, while avoiding overcomplicating the postmortem.  It includes enough detail that readers familiar with BGP will understand exactly what went wrong and why.  Finally, it avoids pointing the finger at any one engineer or system in an effort to shed blame for the incident.

The incident itself is cringe-worthy.  As is common in any major incident, multiple distinct failures conspired, resulting in an incident despite built-in safeguards:

* The (valid) configuration change failed the automatic consistency check due to a race condition.  It is unclear whether this failure mode was previously known.
* The system should have rejected the configuration after deeming it inconsistent but instead created an empty configuration.  This bug was previously unseen.
* The configuration failed the subsequent canary test, but this failure was not reported back and the network management system deemed the configuration to have passed.  The postmortem implies that this was also a previously unseen bug.

The system proceeded to deploy the configuration gradually across the entire infrastructure.  The Internet's built-in redundancy masked the issue by continuing to route GCE-bound traffic to edge routers that had not yet applied the new configuration.  This made detection of the problem much harder.

After resolving impact, the incident response continued as engineers attempted to understand enough about the root cause to be sure that it would not recur.  The postmortem was released impressively quickly, indicating a high priority placed on incident followup and external communication.  This emphasis on followup can also be seen in the high-quality remediation items listed in the postmortem.

The incident itself is rather concerning, as Google mentions in the conclusion of the postmortem.  It spanned all regions in GCE, which means that users could not avoid impact simply through a multi-region deployment.  Adding additional safeguards and improving detection and response would help, but to truly avoid this kind of problem in the future, Google would need to split the network management layer by region so that erroneous configurations could not cross the region boundary.  This would allow connectivity to a region (through other regions) even if all of that region's edge routers stopped announcing all GCE IP blocks, albeit with increased latency.

In summary, I consider this a model postmortem because it was released very quickly and contains:

* heartfelt, convincing apologies
* detailed impact description
* an appropriate level of detail (enough for experienced engineers but not too much for others)
* details on the incident response
* a precise timeline
* remediation items

It should be noted that there was a lag in public communication during this incident.  It was not particularly severe, but the total connectivity outage was only reported shortly before it was resolved.  Improvement here would help customers to deal with such severe incidents in the future.

## About the Author

My name is Lex Neva and I am an SRE at [Heroku](https://heroku.com). You can reach me at `lex at sreweekly.com` or @SREWeekly on Twitter.  Subscribe to my newsletter, [SRE Weekly](http://sreweekly.com), via Twitter, RSS, or email.


###### \#outage #network #bug #technical
