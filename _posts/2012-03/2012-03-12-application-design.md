---
layout: post
category : apps
tags : [apps, infrastructure]
---
{% include JB/setup %}

One of the key design decisions often made during agile development is to decouple your apps and keep your infrastructure compartmentalised. As technologies evolve, there will be an ever widening landscape of tools, platforms and languages you will be building apps with. You'll want to work with the most appropriate ones, therefore it's important to loosen the ties between components in you apps to retain this choice and remain flexible. 

It's also best to have the freedom to isolate and re-engineer a component in the system when the time comes (be it improvements to features, technology, or performance), rather than trying to mash in an updated version into your existing app, which may no longer be the best tech for the task. This is reflective of a guiding paradigm of the cloud; building applications that leverage disparate services and APIs to subsequently deliver a service.

Just some of the advantages of building modular services:

* **Isolated.** The app is free of all irrelevant logic, limitations and bugs in other apps.
* **Versatile & independent.** It can be deployed anywhere. It's not tied to any service and doesn't need to be packaged deployed with anything else. Moreover it can be scaled and failed over without affecting other components.
* **Clean foundations.** We can invest our latest knowledge and expertise in a new codebase, without being held back by mistakes made before.
* **Decoupled.** Integration with other services via APIs & protocolled connectors (e.g. AMQP). APIs are perfect mediators between applications. They enable independent evolution on both sides of the API, whilst maintaining a consistent gateway for communication between them (if properly designed and versioned, of course).

The result is that your architecture comprises independent, distributed applications, effectively representing nodes in a graph. These nodes are loosely coupled but interconnected by the use of message queues, pub/sub, APIs and database pools. The nodes are independently deployable, scalable and maintainable and can in turn contain their own hierarchy of failover & durability techniques.

Although I'm solving problems like this on a  day-to-day basis, I recently encountered a good example of this application design pattern. At [GoSquared](http://www.gosquared.com/) we had a PHP application which needed to have consistent user sessions between it and a completely separate, brand new Node.JS application. The idea here being that if a user logs into one app, they would also log into the other.

Rather than extending the existing and rather bloated PHP app, and in light of the above and our rapidly growing experience with Node.js, we decided to build the new app as a separate Node.js service. That way, we were able to circumvent many existing technical limitations plaguing the PHP app. 

On the flipside, the problem was not as simple as just abandoning the PHP app in favour of the new Node.js one. The PHP app still holds thousands of lines of business logic which is not a cheap or quick task to redevelop. Besides, why crush the whole Skoda, when only one of the wheels needs replacing…? Ok, maybe we will crush the Skoda at some point, but not just yet!

Therefore, it became a matter of keeping the apps separate, but having them chat via APIs and connectors. Although PHP session handling isn't pretty, I implemented a shared state, interactive user sessions handler for PHP to bridge sessions between the two applications.

Although these ideas and concepts aren't in any way new, they're much more commonly brought to the forefront of the engineer's mind particularly when designing large or distributed systems for deployment on many hosts or on a cloud-based platform. Hence the rapid evolution of hosting & management services such as [CloudFoundry](http://www.cloudfoundry.com), [Joyent](http://www.joyent.com), [Heroku](http://www.heroku.com), [Linode](http://www.linode.com), [appfog](http://www.appfog.com), along with deployment strategies such as [deliver](http://github.com/gerhard/deliver) and [capistrano](http://github.com/capistrano/capistrano).
