---
published: true
layout: post
---
## Archival Material Ingest Workflow

It has taken quite some time, but we've honed in on a workflow for ingesting archival materials from the Reuther library -- aka, University Archives here on campus -- into our Fedora Commons based repository.  Turns out, the following is all there is to it!

![archival_ingest_workflow.jpg]({{site.baseurl}}/assets/images/archival_ingest_workflow.jpg)

Rest assured, there are more to those arrows than immediately meets the eye.  Let's dive in.

Our goal was to take digitized archival materials from the Reuther Library and provide preservation and access via our [Digital Collections](http://digitalcollections.nypl.org/about) infrastructure.  At a very high level, we were imagining **one digital object in Fedora for each digital file coming from the archives**.

But we were realistic from the get-go, it was going to be a much larger enterprise than file-by-file.  How would we manage ingest and description at scale?  To answer that, we need to look at the systems in play: [Archivematica](https://www.archivematica.org/en/) and [ArchivesSpace](http://archivesspace.org/).

**Archivematica** is a series of tubes.  To be more precise, micro-services.  Archivematica takes groups of files, runs them through a host of preservation micro-services -- virus scanning, file format normalization, checksumming, etc. -- and ties them up together in a tidy bow with an over-arching [METS](http://www.loc.gov/standards/mets/) file.  Archivematica is inspired by the venerable [OAIS model (Open Archival Information System)](https://en.wikipedia.org/wiki/Open_Archival_Information_System), and as such, speaks in terms of [AIPs, SIPs, and DIPs](https://en.wikipedia.org/wiki/Open_Archival_Information_System#The_functional_model).  

We are using Archivematica as means to get actual, discrete digital files from the Reuther into a format that we can batch process them for ingest.  Additionally, we get all the preservation friendly treatment from the micro-services, and begin a paper trail of metadata about the file's journey.  It's quite possible we'll dig deeper into affordances and functionality of AM (it's time to shorten), but for now, it's primarily a virus checking, checksumming, METS writing, server/building spanning networked pipeline for us.  And it's going to be great!

The next dish in the cupboard is **ArchivesSpace**.  ArchivesSpace, save a passionate and exploration here of just what ASpace is and represents, it's safe to think of it as the next generation of archival software used to handle description, management of information around materials, discovery, and much more.  Our partner in crime, the Reuther Library, is slowly switching to ASpace to handle their description and information management.  It is a database driven application, that still also exports finding aids for archival collections in EAD.  We'll be using those, with plans to leverage the API once deployment has settled down a bit.

Our involvement with ArchivesSpace is limited primarily to our metadata librarian who takes a manifest of the files as processed by Archivematica, an EAD of descriptive and intellectual organization metadata about the collection as exported from ArchivesSpace, and **creates a new METS file meant to enrich / augment the original Archivematica METS file**. 

Whew!

I've perhaps nestled myself too deeply in the weeds here, so lets zoom out.  We...

1. take files from the archives via Archivematica
2. these come with an AM generated METS file that represents the "physical" hierarchy and organzization of the digital files on disk
3. we then take an EAD from ArchivesSpace that contains "intellectual" hierarchy and description about the materials, and synthesize a new METS file that represents the intellectual organization of the files - something we refer to as "AEM METS", for "Archival Enrichment Metadata (AEM)"
4. with the original digital files, AM METS, and AEM METS, we create bags on disk
5. finally, ingest!

Where, and how, does this happen?

This occurs in an increasingly substantial corner of our adminitrative middleware, [Ouroboros](https://github.com/WSULib/ouroboros), called the "Ingest Workspace".  This Ingest Workspace, the intent of this blog post and which I've managed to bury pretty far down here, is where we take these collaborative bits of information and assimilate them into sensible digital objects we can ingest.  It's the green box in the diagram above.

This process has taken a considerable amount of time, as it's a complex process!  So much so, that wonderful folks at the [Bentley Historial Library](http://bentley.umich.edu/) [received a grant and to fund research into wiring these platforms together](http://archival-integration.blogspot.com/) to aid in these kind of ingest workflows (as you dig down, the details are different, but the end goals share many similarities - moreover, their blog and presentations at conferences have been a huge help for thinking through these processes).

The difficult and complexity come, in large part, to reconciling **physical** and **intellectual** arangement of archival materials, or any materials for that matter.  A quick and dirty example: a postcard from a friend is in a shoebox, in a drawer, in my desk.  That is the stellar physical arrangement I have chosen.  However, I may have it intellectually organized under *meaningful materials --> postcards --> international*.  And that's an easy example where the hierarchical levels align.  How, then, might we digitize this item and provide access to a user, while also trying to contextualize the item within its intellectual and physical place?

We decided to drop one: **physical**.  I should point out here that even within the "physical" hierarchy, we are actually often referring to **digital** and **analog** versions of the resource.  The Reuther library has made the *very* wise choice of organizing their digital files where possible to mimick their physical hierarchy, which makes this considerably easier.  But suffice it to say that we retain both, in form or another, such that we can work backwards and figure out where the original digital or analog version lives.

To wrap up, we are choosing to organize and contextualize the files primarily based on their **intellectual** arrangement as suggested by ArchivesSpace.  Which, in fine fashion to finish, explains the need to interwine information from Archivematica *and* ArchivesSpace!  This post comes on the heels of an early successful ingest of this workflow, expecting all kinds of interesting twists and turns as we proceed -- newly digitized materials, updates to metadata, pointing back to ArchivesSpace (which we have in our back pocket with ASPace identifiers), etc.











