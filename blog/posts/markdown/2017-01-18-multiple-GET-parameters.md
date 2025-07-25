---
published: true
---
An interesting aside about GET parameters, particularly of the multiple variety.

Solr accepts, where appropriate _expects_, the same `GET` parameter multiple times.  e.g. the `fq` parameter:

    http://example.org/solr/search?q=goober&fq=foo:bar&fq=foo:baz
    
Pardon an oversimplification, but in this scenario Solr is using a custom parser to parse the multiple `fq` `GET` parameters.  It is custom, in a sense, because [RFC 3986](https://tools.ietf.org/html/rfc3986#section-3.4), which serves as a specification for generic URLs and parsing parameters, doesn't explicitly discuss how to handle multiple `GET` parameters.

But they exist.  And Solr is a great example.

Further speculating under the hood in Solr, you can divine that it also allows nesting of values in `GET` parameters, as demonstrated with fields like `facet.field` which, in addition to being repeatable, also exists next to a frighteningly similar field `facet`.  When solr parses a URL such as:

    http://example.org/solr/search?q=goober&fq=foo:bar&fq=foo:baz&facet=true&facet.field=foo
    
we can assume that anything with a `facet.` prefix, like `facet.field`, is probably getting grouped as a nested structure Solr-side.

But how do other systems handle this?

There is a convention, not a specification, that I stumble on from time to time that can be a bit of a headache.  Some libraries fallback on using square brackets `[]` affixed at the end of a field to tell future parsers that this field is repeating, and should be slotted into some kind of list or array, instead of overwriting a key / value pair previously seen in the URL.

This is great, and works for well for back and forths between systems, but can be complicated when those parameters eventually need to slung over to Solr.  Python Flask, for example, out of the box, only handles repeating `GET` parameters when they come in with the `[]` suffix. e.g. 

    http://example.org/route?fq[]=bar&fq[]=baz
    
This means, before you can scuttle over to Solr, you'd need to rename these `fq[]` keys to `fq`, as Solr does not know what to do with `fq[]`.  

Just one of those things.  But interesting, and perhaps telling, that the HTTP protocol and parameter parsing is getting pushed to it's logical limits this day in age.
    










