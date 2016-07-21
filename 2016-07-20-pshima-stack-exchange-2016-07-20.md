# Report: Stack Exchange Network Status Outage Postmortem July 20, 2016

Postmortem link: http://stackstatus.net/post/147710624694/outage-postmortem-july-20-2016

## Company Overview

The customer base of Stack eExchange is largely of technical users looking for solutions to problems and/or to participate in the large community and knowledgebase present in this network.

From the Stack Exchange about page:

Stack Exchange is a network of 150+ Q&A communities including Stack Overflow, the preeminent site for programmers to find, ask, and answer questions about software development. Founded in 2008 by Joel Spolsky and Jeff Atwood, the company was built on the premise that serving the developer community at large would lead to a better, smarter Internet. Since then, the Stack Exchange network has grown into a top-50 online destination, with Stack Overflow alone serving more than 40 million professional and novice programmers every month. The broader Stack Exchange Network has expanded to cover topics as diverse as Mathematics, Home Improvement, Statistics, and English Language and Usage.

## Incident Overview

I was not personally impacted from the event but the post mortem suggests that this was a major or full outage.  It does also not describe what sites were impacted.  As Stack Exchange is a network of sites, this would suggest that every site on the network was impaired to the point of being completely unavailable.  The target audience of this is likely the people impacted from it so it may be omitted intentionally and the tweets regarding it do clarify this information.  This was later clarified that it was only Stack Overflow that was offline, other sites were only impacted by high CPU usage on the webservers.

The overview also provides some interesting details that give us some insight to where the 34 minutes time was accrued.  10 minutes of identification, 14 minutes to write the fix and 10 minutes to deploy.  This is an interesting point of detail that is often not included in post mortems and I feel it gives the reader more understanding of how the event unfolded and where time was spent.

The next paragraph is where things start to get interesting as it touches on several of the root causes of the issue.  The initial one was high CPU usage on the webservers which was caused by the regexp, and this had the knock on effect of slowing down all web requests, which had the knock on effect of causing what I am guessing is time outs on the load balancer health checks, which caused them to deregister all the webservers, which then caused the load balancers to have no available web servers to send traffic, which sent 503 responses to users.  Some specific details are left to the user but are not material in understanding the cascading impact of the events that unfolded.

The lower Technical Details section goes into specifics on why the regular expression caused so much havoc and gives the reader confidence that the Stack Exchange folks have a deep understanding of the problem.

## Incident Updates/Postmortem

The incident was first posted to twitter via https://twitter.com/StackStatus/status/755778941600882688 at 14:58 UTC, just 14 minutes after the time noted in the post mortem, retweets were then on the account from https://twitter.com/Nick_Craver, with updates noting the fix was being deployed and that it was back online. There is actually considerably more detail from Nick's twitter than from the official Stack Exchange twitter including a graph of CPU usage: https://twitter.com/Nick_Craver/status/755793398544601088 and a tweet describing that they are using HAProxy as a load balancer and that health checks can be enabled/disabled in realtime: https://twitter.com/Nick_Craver/status/755795805798330368.

Overall the response was pretty timely and the retweets gave users updates as to what was occuring and multiple follow ups were added even though the outage window was only 34 minutes.

## Remediation / Future Work

* Audit our regular expressions and post validation workflow for any similar issues

This seems like a solid follow up task.

* Add controls to our load balancer to disable the healthcheck – as we believe everything but the home page would have been accessible if it wasn’t for the the health check

This was clarified as a control that an operator could use to disable the health checks if the same issue reoccurred.  It does not solve the larger fleet utilization issue but it would have prevented this issue from reoccuring in which all the hosts were marked as unhealthy on the load balancer.  Despite other sites possibly being slow from the utilization, they still would have been "up".

* Create a “what to do during an outage” checklist since our StackStatus Twitter notification was later than we would have liked (and a few other outage workflow items we would like to be more consistent on).

This is an interesting action item and I love seeing action items around improving process.  This suggests such events rarely happen at Stack Exchange and when the issue occurred there wasn't a 'what to do during an outage' checklist.  If that was the case then this is an impressive recovery time from the events without a procedure.  It also shows that while it was 14 minutes to post, they believe they can improve on that time window and response to customers.

## Extrapolations / analysis / additional thoughts

It is great to see this level of visibility from Stack Exchange!  The details in the post mortem give the reader a decent understanding of the issue and what caused the full outage.  The tweets would suggest Nick played a big part in writing the post mortem and also in recovery of the issue and I would like to thank Nick and the Stack Exchange team for providing this visibility and for releasing the post mortem so quickly after a major incident.

It is unknown to me if there was a specific site with the post in the Stack Exchange network that caused the issue but without further detail it would suggest that there is a very large blast radius for Stack Exchange sites and a bug on any of the sites could cause a full network outage.  This is not a negative, only an observation from this post mortem.  It does leave the reader wondering more.

I would have loved to see some additional information on how the event was discovered and more around improving resiliency of the web server fleet.  The root cuased was identified fairly quickly, and more knowledge here could give the reader additional confidence that while this specific issue was resolved, future bugs or unknowns could also be resolved in a similar manner.  This is an nit pick of this post mortem and I would consider this post mortem as a good example as a follow up to a major issue.  It was later clarified by Nick that details were intentionally left out because the tools they use legally cannot be shared.  The clrmd tool (https://github.com/Microsoft/clrmd) was referenced as something to look at.

The speed at which the issue was resolved, combined with the communication and immediate follow up to the issue gives the reader confidence that future issues will be resolved in a similar manner and this is a major plus for any good post mortem.  I would celebrate this post mortem as a success!

## Citations / Additional reading

* https://twitter.com/Nick_Craver

## About the Author

My name is Pete Shima. me@peteshima.com - petey5k@twitter

## \#outage #regexp #bug #technical #loadbalancer






