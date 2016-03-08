Report: Amazon EC2 and Amazon RDS Service Disruption in the US East Region, April 21st-April 24th 2011

## Company Overview
Amazon Web Services is a collection of cloud computing services which offers storage, load balancing, on-demand compute, DNS management, and more. One of the key advantages they offer is having on-demand resources available in 12 separate regions across the globe. For this reason and because of the ease of setup and configuration, they are quickly becoming pervasive throughout the web as an infrastructure provider. AWS's Q3 2015 revenue was $2.1 billion. 

## Incident Overview
For this report, we'll be focusing on the "Summary of the Amazon EC2 and Amazon RDS Service Disruption in the US East Region". This postmortem covers an incident which spanned April 21st-April 24th, 2011. The postmortem was published via Amazon's blog on April 29th, 2011. This incident is notable because it was somewhat widespread and resolution took longer than 24 hours - making it one of the largest outages that AWS has experienced. 

The first thing that Amazon does, after briefly explaining the purpose of the postmortem document, is describe the impacted service in detail. This is necessary information to fully understand the incident how it is related in the following paragraphs, and is very important to read first for anyone not well acquainted with the services in question. This piece is likely necessary even for other engineers working at AWS who do not interact with EBS on a regular basis.

Their description of EBS is basically as follows: EC2 relies upon EBS for storage. EBS works in clusters comprised of nodes, and data is replicated across those nodes for redundancy and reliability. If there is any interruption in the connection between nodes, each node assumes that the other node has failed. This means it searches out a fresh node to establish replication with. However, whenever a node is searching for a new partner, it will not allow reads or writes. This basic outline of how node replication works within EBS gives you a good basis for understanding the issue which occurred on the 21st.

## Incident Updates / Postmortem
The incident began when someone improperly executed a network change and shunted a bunch of traffic to the wrong place, which cut a bunch of nodes off from each other. Because so many nodes were affected at one time, all of them trying to re-establish replication and hunt for free nodes caused the entire EBS cluster to run out of free space. At this point, the 13% of EBS nodes were stuck searching out a partner and not being able to accept reads or writes.

Due to the distributed nature of the control pane which accepts user interaction with EBS, other availability zones within the region were also being affected. The AWS team quickly shut off volume creation from the control panel, which caused the other availability zones to stabilize. However, the situation continued to degrade as nodes kept searching for partners where there were none - effectively executing a denial of service attack against their own infrastructure and crashing even healthy nodes due to a race condition bug. Eventually, this caused the issue to seep out into other availability zones once more, until the AWS team shut off all communication between the problem cluster and the control pane. This meant customers couldn’t interact with anything in the affected Availability Zone (even unaffected nodes).

At this stage, the issue was contained to a single availability zone, and AWS was able to focus on fixing the busted nodes. However, EBS clusters do not reuse any failed node until every data replica is re-mirrored. This created huge capacity requirements for the cluster, which was the main bottleneck in recovery. Capacity had to be manually added to the cluster, which was made more difficult by some of the remediation steps taken prior. However, once capacity was added, nodes began to recover. Though some manual work was required to recover a small percentage of nodes, in the end all but 0.07% of nodes were fully recovered.

Relational Database Services affected in addition to EC2, because they also utilize EBS. For users utilizing single-availability zone RDS in the affected region, the exposure to the issue was similar to EC2. For those in multiple-availability zone RDS, there was an additional bug which prevented failover of a small percentage of instances. The description of this bug is not specific - probably the most vague portion of the postmortem. This isn’t surprising considering how close to the vest amazon plays its proprietary database system.

## Remediation / Future Work
The postmortem finishes up with an analysis of the issues and steps to prevent future occurrence. The first is implementing a more robust change process to prevent the mistake that caused the entire outage. The second is expanding capacity to be ready for recovery events in the future, and the third is modifying the retry logic so that the DOS-type behavior (what they call a “mirroring storm”) does not occur.

Amazon also pledges to make it easier for customers to expand across multiple availability zones. It is notable that Twilio and Netflix, both of whom rely heavily upon amazon for their infrastructure, were nearly unaffected by the outage from their customers' perspective. Both of these companies also wrote postmortems of the incident with information on how to keep problems like this from impacting your bottom line.(2)(3) It's apparent that how you choose to utilize a system like AWS can be as much of a determining factor in your reliability as the uptime in their regions.

Finally, Amazon offers an apology and a credit to impacted customers, and promises to recover faster and have better communication for future incidents. They also give a peek of things to come, offering tantalizing snippets of future functionality such as cross-region VPCs and snapshotting stuck/broken EBS volumes.

## Additional thoughts
Amazon's postmortem structure seems to be consistent across multiple events. Many seem to use roughly this outline: 

1. Statement of Purpose 
2. Infrastructure overview of affected systems 
3. Detailed recap of incident by service
4. Explanation of recovery steps & key learnings
5. Wrap-up and conclusion

They likely have an established method for generating postmortems that they review whenever they need to do outbound communication about an incident. This is a good practice that many companies probably do not develop until later stages where processes like this become more formalized; but could be massively beneficial in establishing productive lines of communication with customers.

This postmortem is very technical. It goes into the specifics of how ec2 works and expects the user to have a relatively thorough understanding of computer systems, networks, and storage. The technicality of the postmortem doesn’t necessarily reflect the complexities of the issue (which were considerable) but may be a reflection upon the intended audience. AWS caters almost entirely to engineers and operations people who are highly technical in nature. If this was a product intended for sales teams, this type of postmortem would definitely need to be accompanied by something with very different language. 


## Citations/Additional Reading:

1. https://aws.amazon.com/message/65648/

2. http://techblog.netflix.com/2011/04/lessons-netflix-learned-from-aws-outage.html

3. https://www.twilio.com/engineering/2011/04/22/why-twilio-wasnt-affected-by-todays-aws-issues/


## About the Author
My name is Gabe Abinante and I am an SRE at ClearSlide. You can reach me at gabe@abinante.com or gabinante@twitter

