---
published: true
---
In everything gloriously complex, it can be difficult to ascertain when you've reached a level of understanding that might merit debrief and reflection.  In my forays into machine learning, this is probably as good a point as any.

I've been interested in Machine Learning for quite some time now.  Since I first started stumbling on ImageNet and how it helped image processors classify images, to actual tinkering with an Alpha release of Google Vision, through the heady days of self-driving cars, and backing up even further, initial go's at deriving meaning and topics from text with Python libraries like [NLTK](http://www.nltk.org/).

"Quite some time," being relative.  It's a rapidly developing area of computer science, philosophy, mathematics, humanities, and their intersections.

Recently I've embarked on a project to try and derive topics from a corpus of academic articles in PDF form (around 700 of them), then by re-submitting one to the model, asking what other articles are "similar".  After quite a bit of trial and error, researching these emerging areas, and honing in on workflows that I might actually whiddle into working, I'm thrilled to have some results coming back that are -- genuinly -- spine-tingling.  

The guts of this project falls on the python library, [gensim](https://radimrehurek.com/gensim/).  A masterful library meant to humanize the multi-dimensional math that underpins machine learning (and I use that term loosely here).  

The rough sketch is thus:

1. Point this little tool (called `atm`, for "article topic modeling", and the way it _dispenses_ fun things to think about) at a dropbox folder with API credentials
2. Downloads the entire directory, in this case, ~700 articles
3. Create a bag of words for each document
4. Strip of [stopwords](https://en.wikipedia.org/wiki/Stop_words) and punctuation (poorly at this point, I might add)
5. Then the fun part: create a [Latent Dirichlet Allocation (LDA) model with gensim](https://radimrehurek.com/gensim/models/ldamodel.html)
6. Index ~100 topics that are suggested by this model, for this given corpus
7. Finally, query the model with a document (in this case, an article from the corpus) for similarity to other documents in the corpus

The results are other documents in the corpus, and a percentile on topical vectors that match the article.  I should stop myself here: the details are still forming, and while I'm getting a good grasp on relatively low-level how this works, that's fodder for another post.  At this point, the rough sketches.

Below are the results of a query, run through a [Jupyter](https://jupyter.org/) notebook of atm:

![Screen Shot 2017-01-12 at 3.41.16 PM.png]({{site.baseurl}}/assets/images/Screen Shot 2017-01-12 at 3.41.16 PM.png)

I submitted an article called `Arnold_2003.pdf`, and it suggested a handful that match topically.  The magic, the interest, lies in how these topics are derived and how the similarites are ranked.  Much of this can be attributed to the [LDA model](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation) that gensim creates for me.  While these results are fascinating in their own right, what really sends chills down my spine, is the similarity with which this process / workflow shares with other domains like image processing, self-driving cars, speech recognition, etc.

Google's Tensorflow has a wonderful tutorial, [MNIST for ML](https://www.tensorflow.org/tutorials/mnist/beginners/) that helped with my understanding.  When the inputs you're dealing with are 28x28 pixel images, of nothing but handwritten numerals from 0-9, you can begin to wrap your head around the math that supports machine learning.  When we quantify input -- sound, visual, text -- into vectors and matrices, we can look for patterns over moving windows of input.  Well, computers can.  They can see patterns in a machine-digestable version of media we entertain with our senses.  And when, with great grit and finesse, we can bubble up these patterns to more high-level libraries, we can apply them to actual corpuses.  It's phenomonal.  

The results from atm are already encouraging, and it's almost by-the-book tuning from tutorials.  I want more corpuses, more targeted querying, adjusting of modeling parameters, the works.  But for now, I've been thrilled to get something working with the tools I love and understand.

Much more to come on this front.  An example: taking the thousands of PDFs that will soon flood our digital collections platform at [Wayne State Digital Collections](https://digital.library.wayne.edu/digitalcollections/), run topic modeling on these, and provide new and interesting ways to find related documents.









