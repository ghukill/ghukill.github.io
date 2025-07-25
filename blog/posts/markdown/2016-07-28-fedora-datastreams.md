---
layout: post
title:  "Fedora Commons and Datastream Dis-Contents"
date:   2016-07-29 15:00:00 -0400
tags: fedora commons, plotly
---

Had a good headscratcher recently.

Ingest times for ebooks going into Fedora Commons were becoming suspiciously long.  Specifically, they would slow down over the course of a single book's ingest: the first 0-20 pages would fly along, then would become noticeably slower, until they were but crawling along.

At the time of these slowdowns, our ebooks were modeled as a _single_ digital object in Fedora (more on this below), with the following approximate structure for pages in the ebook object:

```
- IMAGE_1
- IMAGE_1_JP2
- HTML_1
- ALTOXML_1
...
- IMAGE_n
- IMAGE_n_JP2
- HTML_n
- ALTOXML_n
```

So a 100 page book might have approximately 400 datastreams.  This was **v2** of our ebook modeling.

Our first, **v1** model for ebooks was a bit more abstracted out, with different file types for each page broken out into their own objects.  A lofty model below shows these relationships, where circles represent objects in Fedora:

![Ingest Times for Gulliver's Travels]({{ site.baseurl }}/assets/images/WSUebook_v1.png){:class="img-responsive"}

In that model, any one of the "child" objects would still have as many datastreams as there were pages in the book, which could be 100, 200, 1000+.  It was, in retrospect, better than **v2**, but not optimal.

We eventually abandoned that model in favor of the **v2**, single-object approach for simplicity's sake.  We were pushing back a bit on over-engineering and abstracting components of complex digital object in Fedora.  For management / preservation purposes, we had but one object to shepard!  It seemed great in the long-term.  This is, of course, a philosophical debate in-and-of-itself, but suffice it to say we had talked through many of the pros and cons, and the single-object approach was working nicely for us.  We had a few hundred large books already ingested during **v1**, and after switching to this **v2**, single-obect model, we began in on ingesting a couple runs of serials.  These serials, importantly, contained "books" with only 15-50 pages per.

And this is why we didn't notice the many datastreams per object problem for some time.  Until, that is, we started ingesting books with 400, 500, 1000+ pages and started noticing the "Great Slowdown."

The graph below represents the time each datastream type took to add to a single digital object for an example **v2** book representative of the problem.  The **y-axis** is time in seconds, the **x-axis** each datatream (page), and the different lines differnt kinds of datastreams (`IMAGE`, `ALTOXML`, `HTML`).  As you can pretty clearly see, particularly in the blue `IMAGES` line which amplifies the trend, each datastream took increasingly long to add to the object:

![Ingest Times for Gulliver's Travels]({{ site.baseurl }}/assets/images/Gulliverb21570504_ingest.png){:class="img-responsive"}

While I could tell they were slowing down during ingest, it wasn't until graphing it out that the unformity of the slowdown was so evident.  Thinking it might have been related to the large TIFF images we were converting to JP2, or the hundreds (sometimes thousands) of `RELS-INT` and `RELS-EXT` relationships we were creating, I thought it might be related to garbage collection in Java for Fedora, something I wasn't terribly knowledgeable about.  Even [posted to StackOverflow](http://stackoverflow.com/questions/38459380/application-slows-down-over-time-java-python), hoping someone might recognize that familiar curve and have insight or advice.  Though I appreciate the suggestions and places to look next for tuning GC for Tomcat, it wasn't the problem.

Curious if it was related to the `RELS-INT` or JP2 conversion background tasks, I tried iteratively adding thousands of nearly empty, plain text datastreams to an object -- timing those too -- as a test.  And wouldn't you know, there was that curve again:

![Ingest Times for Tiny Datastreams]({{ site.baseurl }}/assets/images/datastream_creation_test_ingest.png){:class="img-responsive"}

That's when I became convinced it was the sheer number of datastreams causing the slowdown, with other tasks and processes amplifying the trend to varying degrees.

With that hypothesis in mind, did a bit more digging on the [Fedora Commons listserv](https://groups.google.com/forum/#!forum/fedora-community) and [struck gold](https://groups.google.com/forum/#!searchin/fedora-community/datastream$20limit/fedora-community/BMASQyMZtio/5KSTeRfHUpoJ).  Turns out, **one should not add that many datastreams to an object in Fedora**.

```
"...to have more than a few dozen datastreams in a content model is very unusual and implies the possibility of useful refactoring..."

"...I don't mean to imply any criticism, but I do wonder about any Fedora-based architecture featuring objects with thousands of datastreams. It can be objectively said that such an architecture is not at all idiomatic."
```

We have done quite a bit of work with Fedora Commons over the past 3-4 years, with lots of time spent thinking about the pros and cons of various modeling approaches, but had not encountered good reasoning behind why a large number of datastreams for a given object was a bad idea.  Yes, you could make the case from an architectual, idiomatic point-of-view, with tendrils and reasoning buried in good preservation practice (e.g. migrating file formats of certain pages but not others, etc.).  But, it seemed you could make a case for storing all the datastreams in a single object as well, leveraging `RELS-INT` for relationships between those datastreams, with the reduced complexity as a digital presrvation gain.  The reasoning for not many datastreams per object seemed limited to approach, not code effeciency and performance.  At least I had not stumbled on those factors.

All in all it was a _most_ interesting quest, and we came out victorious.  Our **v3** model for ebooks aligns more closely with the community -- and PCDM, as we slowly move along towards FC4 -- and [our ebooks have been migrated](https://digital.library.wayne.edu/digitalcollections/search.php?&fq[]=rels_hasContentModel:%22info:fedora/CM:WSUebook%22&start=0).  Though the elements and clues of the mystery ([plotly](https://plot.ly/) is amazing, by the way) were fascinating, the entire episode has left a more enduring and interesting thought upon the brow: that best practices / approaches for doing something may have a diverse web of reasoning behind it.  Too many datastreams were frowned upon, but most listserves and literature made it sound like it was for modeling and/or preservation reasons, not a technical limitation of Fedora to efficiently handle a large number of them per object.  That said, I would have to assume it is _because_ that many datastreams per object is not "idiomatic" use of Fedora that it is not efficient.

There is a strange chicken-or-the-egg thing happening here, perhaps better left for another time... where a high-level architectual decision has performance implications downstream, but because those performance effects are not the "root" of why that approach is favored, they are not mentioned much in literature... 


 

























