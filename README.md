# DDOS Article identification
The goal here is to teach a machine how to recognize that a news article is
about Denial of Service incidents. I might possibly expand that to try to apply
appropriate labels to other kinds of articles.
The repo consists of several ipython notebooks that I've been working with.

# Problems
There are two problems that I need to overcome. First is getting the highest quality
text extraction from the source documents. Right now I'm working on that in a notebook
called __extraction__. The other problem is training a classifier and making
good predictions. My best work so far is in a notebook called __pipeline__.

# Files

* **extraction.ipynb**: One way of getting the article text from the VCDB
articles
* **extraction2.ipynb**: A different way of doing the extraction that might
result in higher quality text for the classifier.
* **softpedia DOS.ipynb**: A custom text extractor just for softpedia.
* **Absolute Garbage**: My first attempt at a classifier made from text that
I copied almost verbatim from a book.
* **CountVectorizer**: Tries to get better results by counting how many times
a word appears rather than just a boolean value of whether it appears.
* **Guassian Naive Bayes**: I don't know what I was doing here or how well it
worked.
* **Top VCDB Sources**: a notebook I used to figure out which websites were
used as sources in the VCDB most frequently.
* **NLTK Example**: an example of using Natural Language toolkit to classify.
* **Pipeline**: an example of how to combine a vectorizer, transformer, and
classifier into a single python object that does everything.



## Extraction
I've experimented with a few different ways that I can
grab the text from a bunch of web pages and only process the meat of the
article instead of all the ads, javascript, and other bs.

 * ```if len(re.findall(r'\b\S+\b',tempstring)) > 7:```
 checks to make sure that a line from an article is at least 8 words long or it
 wont be included in the file that gets downloaded.
 * urllib2.urlopen and addheaders to get around pages that don't want to be
 scraped.
 * ```cleaned = nltk.clean_html(rawHTML)``` does a good job of getting rid of
 the html tags and leaves behind just the text.
 * need to experiment with a python module called ```newspaper```
 http://newspaper.readthedocs.org/en/latest/

early attempts to use just ```nltk.clean_html``` resulted in a lot of html
codes sneaking in &gt;, &copy; and stuff like that. So I added some regular
expressions to strip those out manually.

_We call vectorization the general process of turning a collection of text
documents into numerical feature vectors. This specific strategy (tokenization,
counting and normalization) is called the Bag of Words or “Bag of n-grams”
representation. Documents are described by word occurrences while completely
ignoring the relative position information of the words in the document_
http://scikit-learn.org/stable/modules/feature_extraction.html

## Classification
As pointed out in one of the articles that I've looked at, the process of
taking a text document, vectorizing it, transforming it, and then classifying
is common enough that scikit-learn has a pipeline class that behaves like a
compound classifier.

Now I need to play with grid search and a few other ideas for improving the
classifier

 * what if I go through the text before it gets classified and change a few
 key terms to be consistent. For example, _denial of service_, _distributed
 denial of service_, _ddos_, _denial-of-service_ all mean the same thing.
 What if I use a regular expression to change all of these to one common
 term like _ddos_?
 __OK I did that and I squeezed an extra 2% accuracy for identifying denial of
 service attacks__

# Evaluating
Right now I'm doing manual validation where I take my data set and split it
into a training set and a testing set. I need to experiment with kfolds
