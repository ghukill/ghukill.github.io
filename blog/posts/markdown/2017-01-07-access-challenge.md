---
published: true
---
![](https://digital.library.wayne.edu/loris/fedora:wayne:UniversityBuildings25708%7CUniversityBuildings_25708_JP2/full/full/0/default.jpg)

Stumbld on an interesting access challenge for our Digital Collections today.

When [searching for the keyword, `library`](https://digital.library.wayne.edu/digitalcollections/search.php?q=library), all records in the Digital Collections are returned because `library` exists for each record somewhere in the metadata (probably embedded somewhere like `Wayne State University Library`).  

Though Solr's stellar ranking boosts relevant records to the top of the results -- items that have library in the title, or prominently in the description -- it's still a little unnerving that it returns so many positive results.

An option would be to limit particular Solr fields from being indexed.  BUT, should we start accepting records from other institutions, with varying metadata, that might become an important field to search and facet on.  I'm sure there are workarounds for this scenario, but interesting all the same, and indicative of the iterative nature of tuning search and discovery systems.
