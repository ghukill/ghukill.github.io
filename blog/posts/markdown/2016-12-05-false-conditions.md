---
published: true
---
![](https://digital.library.wayne.edu/item/wayne:vmc78617/thumbnail/)

Was reminded this morning of a lesson that drifts in and out of working on systems with lots of moving parts: **all improvements are inextricably based to the current condition of supporting infrastructure**.

Said another way: **anything you do, anything you change, is probably based on information available to you at the time**.  

But this isn't the lesson.  The lesson is that that kind of decision making is often flawed.  I'm sure this is of no surprise to many, but I'm uncomfortable how each iteration of an improvement to a particular part of the system brings this same lesson home.  Fool me once, shame on you, fool me twice, yada yada.

A concrete example might help.  

We have series of pipes and routes in our server-side API that abstracts routes for images from our [IIIF-based](http://iiif.io/) [Loris image server](https://github.com/loris-imageserver/loris).  So, we can ask for <code>http://foo.bar/item/goober:tronic/thumbnail</code> and get back a thumbnail at the more complicated URL path, <code>http://foo.bar/loris/fedora|goober:tronic/full/full/0/default.png</code>.  The latter is not semantically meaningful to many, and contains hardcoded infrastructure such as <code>loris</code> in the URL.  Our image server may change, and our goal is to have [Cool URIs](https://www.w3.org/TR/cooluris/) for things like thumbnails, metadata, etc.  As always on this blog, over-simplification for the sake of idea exploring.

Recently, we had the rare and supremely delicious surprise of server kernel patching *improving* server-side rendering of images in our python-based image server, Loris.  Dramatically.  We are still exploring precisely what explains the speed increase (perhaps fodder for another post), but suffice it to say, it's great.  However, thumbnails started to break.  The reason, one of our image proxies was [streaming the results with the <code>requests</code> python library](http://docs.python-requests.org/en/master/user/advanced/#body-content-workflow).  When speeds / rendering / IO was slower on the server, this sped up the load time for thumbnails.  But when the server speed increased, it revealed what I'm assuming was some kind of race-condition as the bits jumped through these proxied hoopes.  Again, this is all speculation at this point, but the fact remains that removing the streaming flag from a particular request has fixed the problem, *and*, the thumbnails load even faster.  

Our original design to stream a response sped up the load-time with a particular set of server conditions.  Now that the conditions have changed, that decision is no longer correct.  How interesting, that a decision once correct, becomes flawed over the passage of time.  Such is a day in the life of managing a system with lots of moving parts.


