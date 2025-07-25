---
published: true
layout: post
---
![bag_validation.png]({{site.baseurl}}/assets/images/bag_validation.png)

Something didn't happen today, and it was incredibly validating.  No validation pun intended here.  Honestly.

We've been ingesting ebooks into our digital collections platform for quite some time now, and one perennial problem we face is that of malformed books.  This can come in many forms, and it hinges on how we structure our digital ebooks.  At a very high level, each physical page gets about five digital representations:

* the page image, as a TIFF file
* raw text from page, as a text file
* coordinates of words on the page, as [ALTOXML](https://en.wikipedia.org/wiki/ALTO_(XML)) file
* PDF of the page, with words overlaying the page image
* HTML of the page, including some limited layout (this one is not great, may be deprecated soon)

So, for a 100 page book, you might -- should -- end up with 500 distinct files: 001.tif, 001.pdf, 001.xml, 001.txt, 001.html, 002.tif, you get the point.

Before we had any ebook specific checks in place, it was not uncommon for a book to enter the ingest workflow missing a singular PDF, XML, or other file relating to a page.  Or even, an entire page (001,002,004, no 003).  This would result in ebooks ingested, but missing key components that would rile things down the pipeline in highly annoying and unforeseen ways.  From a preservation standpoint, it was also not ideal to allow missing derivatives to slip through.

But those days are mostly over.  We have included some bag validation for each different kind of content type in our digital collections, that look for specific properties.  For example, an Image object *should* roll through with an original image, a JP2 derivative, a thumbnail, etc.  If one of those is missing, it fails the validation on that datastream.  For ebooks, we're looking for parity of derivative file counts for each page.  If a page comes through missing something, we get notified in our Ingest Workspace (fodder for another post), and that bag (object) is prevented from getting ingested.

In this way, we can let 44/45 perfectly good books get ingested, then diagnose the rogue baddie.  The wheels of digitization and access roll on, and we identify things that need fixing.  The screenshot above shows this check firing on a batch today, long since forgetting about putting it in place.  Great huzzahs!