---
published: true
---
We're getting into advanced search.  With topic modeling and other alt-metadata searching around the corner, we want to make sure we've got real estate established in the new front-end for the digital collections we've been working on, for unique / beta / interesting / experiemental search options and interfaces.  

So, decided to start wiring up an "advanced search" page to augment our poweful, but relatively simple search page.  Like anything interesting, this candy bar has split higher up than expected, and I'm left chewing much more nougat than anticipated.  Great adventure

A first step -- er, middling step -- was to investigate what "advanced search" looks like in other online, search interfaces.  Both surprising, and unsurprising for reasons hopefully touched on later, the world of "advanced search" has shrunk in the age of facets and intelligent, powerful, omnipotent back-end search engines such as Solr.  The days of crafting the perfect boolean search are dwindling as other search strategies become more popular.  This is not to discount a single, surgical search.  Not at all.  For literature reviews, or general prudent thoroughness, a well-crafted, complex search is still incredibly powerful.  And is actually still possible through many single-searchbox interfaces.

Take Google for example.  With keywords, you can define quite particular searches.

Search only websites on `digital.library.wayne.edu`:

    site:digital.library.wayne.edu winter czar

Or even limiting words:

    seattle -space needle
    
But many users don't know these little tricks.  Since the dawn of online search interfaces, there have often been accompanying "advanced search" interfaces that provide a handful of boxes and inputs that users can use to refine their search.  Our own [library catalog](http://elibrary.wayne.edu/search/X) is a nice example:

![Screen Shot 2017-01-17 at 2.09.05 PM.png]({{site.baseurl}}/assets/images/Screen Shot 2017-01-17 at 2.09.05 PM.png)

But as I started to dig into what other advanced search interfaces looked like for digital collections or archives, I started to notice a trend.  Something I'm referring to as the **mighty four** in my internal running monologue.  They are variations on the following four "advanced" search options:

* all / any these words
* this exact word or phrase
* none of these words

Some will differentiate between **all** and **any**, some do not (yet in my simple mind, they still get the **mighty four** moniker).  But they share a likeness I hadn't really stopped to consider until this undertaking today.  Here are some examples:

![Screen Shot 2017-01-17 at 1.58.00 PM.png]({{site.baseurl}}/assets/images/Screen Shot 2017-01-17 at 1.58.00 PM.png)

Google Books

![Screen Shot 2017-01-17 at 1.58.21 PM.png]({{site.baseurl}}/assets/images/Screen Shot 2017-01-17 at 1.58.21 PM.png)

Google Advanced Search

![Screen Shot 2017-01-17 at 1.58.30 PM.png]({{site.baseurl}}/assets/images/Screen Shot 2017-01-17 at 1.58.30 PM.png)

Advanced Search from the lovely [University of North Texas (UNT) Digital Library](https://digital.library.unt.edu/) (whom I turn to often for inspiration and sanity checks, thanks UNT!).

I, and I'm sure anyone even tangentially involved in the library, archives, and information realm, could write a small novella on the evolution and state of online search interfaces.  They are as important to us as the tensile strength of fishing line for deep sea fishers, the Tin / Lead ratio to stained glass makers, the fat / acid ratios for cooks, and so on and so forth.  However, given the myriad of insight and perspectives around search interfaces, it's hard to ignore that **facets** have taken on a prominent role in search interfaces.  Think of Amazon, eBay, and the list goes on.  Facets are incredibly powerful, and have changed some thinking on search.  **Instead of high-powered, laser-focused boolean searches, we are looking more to iterative refinement in our searching.**

For the most part, I think this is a good thing.  I'm a big fan of doing 10-20 searches in what-interface-have-you, mentally sifting and collating what you learned, rinse and repeat.  As compared to toiling and banging your head over a perfect set of logical operators what will reveal your images of [booze running from Canada to Detroit in the 1920's](https://digital.library.wayne.edu/digitalcollections/search.php?q=booze&fq[]=rels_isMemberOfCollection:%22info:fedora/wayne:collectionvmc%22&start=0).  

But advanced search has a place, particulary when new and interesting search options that hinge on things like community-generated metadata, or machine learned topics are nigh at our disposal.


    

