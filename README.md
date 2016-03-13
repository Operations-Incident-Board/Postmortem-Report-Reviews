# So you want to write a book report?!

This repo exists to house reviews of published post mortem reports.  By publishing our own thoughts and interpretations of companies' publicly available post mortems, we hope to provide a variety of unique perspectives on events that were felt throughout the industry.

If you have a favorite post-mortem, whether favorite-in-a-good-way or favorite-in-a-way-to-learn-from, please contribute!  Pull requests welcome!!

Book reports should follow the template outlined below.  Please focus on what made the outage meaningful to you, what you learned, and how it impacted you and the way you do your job.

## Filename 

The filename of your postmortem book report should follow this format, for easy sorting:

$DATETODAY-$YOURNAME-$COMPANY-$SERVICE-$OUTAGEDATE

Please include a link to the post mortem you are reviewing at the top of the document.

## Company Overview
This section should include a brief description of the company which produced the postmortem. This should include a brief note on their customer base.  Who was this post mortem primarily targeted towards?  (Engineers, consumers, the nontechnical press ...?)

## Incident Overview
This section should describe the incident and the customer impact thereof.  Brief is fine, since the original post mortem contains all the details after all.

## Incident Updates / Postmortem
This section includes a recap and analysis of any outbound communication that the company made during or after the incident. This can be broken out into multiple sections, if it was lengthy and/or multi-stage.

## Remediation / Future Work
What steps does the company claim be taken to prevent future occurrence?  What assurances were provided?  Did they prove correct, or did the same outage happen again?  If *you* were affected by the outage, what steps (if any) did you personally take to make sure you would be unaffected by the next outage?  Overall, what was learned?

## Extrapolations / analysis / additional thoughts
Offer your own opinion on what occurred, include any additional thoughts about the postmortem process or method of communication that the company used. Overall analysis from your perspective belongs here.

## Citations / Additional reading
Please include a link to any other resources (beyond the relevant postmortem) that you utilized to complete your report, or any documents that would give additional context about the outage or the postmortem.  Seeing multiple recaps of the same event can be really useful and cool.

## About the Author
Add details about the writer.  This is optional, but can provide useful context on the perspective of the writer -- e.g. a software engineer may have very different takeaways from a report than a networking engineer.  If you choose to include personal information please follow this format:

#####My name is $name and I am a $jobtitle at $company. You can reach me at $contactinformation, $twitter $plug.

## Meta Tags
categorize the incident and the postmortem for searchability and organization. Use the following tags (more may be added, but please add them to the readme before you use them in a review):

#####\#outage #partial-outage #degraded-service #network #bug #technical #non-technical #external-vendor #failover 
