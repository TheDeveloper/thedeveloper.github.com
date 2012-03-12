---
layout: post
category : infrastructure
title: Resolve ELB connectivity issues
tags : [amazon, elb]
---
{% include JB/setup %}

<img src="http://static.gosquared.com/images/liquidicity/09_12_06_geoffdev_790x100.jpg" alt="Dev with Geoff - Development time with our CTO Geoff Wagstaff" width="790" height="100"/>

We've recently been running into a recurring issue with AWS' Elastic Load Balancing service. The problem manifests itself in vastly increased latency or eventual dropout of requests against the load balancer.

We verified the health of our instances in each availability zone, and concluded that it was not a problem at the instance level. Second, we confirmed that our DNS queries were being routed to the ELB correctly. They were. So the issue was at the ELB level.

As it turns out, there appears to have been/still is a bug where ELB will suddenly decide that there are no available instances in a particular availability zone, or at least it will have trouble connecting to instances in that particular zone. Even if the instances are perfectly fine, and pass all health checks.

The solution was to try to identify the suspect zone, and de-register that zone and all of its instances from the ELB. Once re-registering the zone & instances completed, the ELB was back up to full functionality again.

Hope this helps next time you run into any ELB issues!

P.S. We're big fans of Amazon Web Services - if you haven't already, check out our recent <a href="http://aws.typepad.com/aws/2011/07/summer-startups-gosquared.html" title="GoSquared and Amazon Web Services - real-time web analytics in the cloud" target="_blank">feature on the AWS Blog</a>.
