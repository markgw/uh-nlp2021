# Week 6: Vectors, Lexical Semantics and Word Embeddings

Carry out all the exercises below and submit your answers
[on Moodle](https://moodle.helsinki.fi/course/view.php?id=44338).
Also submit a single Python file containing your full
implementation.


## 1: Questions from the lectures

### 1.1: WSD

The task of word-sense disambiguation (WSD) involves choosing one of
a pre-defined set of *senses* for a particular usage of a word in text.

Say you are applying a standard statistical classifier, like an
SVM or a feed-forward neural network, to this task.

> List three types of features that you could use as input to your
classifier for each token.

<div class="submit">Submit your answer</div>


### 1.2: Distributional semantics

Count-based distributional representations of word meaning (word embeddings)
are often derived from a matrix of feature counts for each term, where
the features are simply words that have been observed within a fixed
window of tokens around usages of the term in a training corpus.

> List two other types of features you could use for each term, from which
to derive a vector capturing its meaning.

<div class="submit">Submit your answer</div>


## 2: Document-term matrix

In this exercise, we will build a simple document-term matrix for the documents provided.

````python
documents = ['Wage conflict in retail business grows',
			 'Higher wages for cafeteria employees',
			 'Retailing Wage Dispute Expands',
			 'Train Crash Near Petershausen',
			 'Five Deaths in Crash of Police Helicopter']
````

### 2.1: Step-by-step construction of the doc-term matrix
Pre-process each document by converting it to lowercase, tokenize, remove stopwords and then lemmatize each token.

Construct the vocabulary of your pre-processed corpus and then construct a document-term matrix by going through each document and checking if a vocabulary word is present or not.

The shape of the matrix will be the number of documents by the vocabulary size `(n_docs x vocab_size)`.

> * What is the shape of your matrix?

<div class="submit">Submit the matrix shape</div>


### 2.2: Using scikit-learn to build the document-term matrix

Try importing Scitkit-learn:
````python
import sklearn
````
If you do not have it installed, install it in your virtual environment:
````sh
pip install scikit-learn
````


Scikit-learn has a class called the
[CountVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html)
to build document-term matrices easily and includes a number of options
such as removing stopwords, tokenizing, indicating encoding (important for documents in other languages), and others.

For more information, see  [the documentation](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html).
This documentation includes a code snippet to build count vectors for each document. You can easily convert these to a binary doc-term matrix, as shown here:

````python
from sklearn.feature_extraction.text import CountVectorizer

vectorizer = CountVectorizer()
X = vectorizer.fit_transform(corpus)
counts = X.toarray()  # Get the doc-term count matrix
dt = counts > 0       # Convert to a binary matrix
doc_term_mat = dt * 1 # If you prefer, represent as 1s and 0s
````

> How does your doc-term matrix from 1.1 compare to your doc-term matrix
> from 1.2? Are they exactly the same or are their differences?
>
> If they are different, what could account for such difference?

<div class="submit">Submit your answers</div>

For the next exercises, we will make use the doc-term matrix with count
vectors (i.e. not binarized) produced by the `CountVectorizer`.


## 3: Ranking documents by query

### 3.1: Using the dot product to rank documents

Suppose you have the query *'retail wages'*. Rank the documents by relevance to this query by getting the dot product of the query by the doc-term matrix.
To convert the query string into a vector, use the `transform()` method of the vectorizer you created in the previous exercise. Remember that the vectorizer expects a list of strings.

Use `numpy`'s [`dot()`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html)
to compute dot products.

> * Which document is the most relevant?
> * Does it align with your intuition?

If necessary, [remind yourself](https://mathinsight.org/matrix_vector_multiplication)
of what happens when you
take a dot product of a matrix and vector. Looking
at the diagrams on the lecture slides might also help.

Normalize the count vectors of the doc-term matrix by the document length and perform the same relevance ranking.

> * Does it produce the same results?

<div class="submit">Submit your answers to these three questions. Include the dot products of the query with the unnormalized and normalized doc-term matrices.</div>



### 3.2: Using TF-IDF to weight words

In the previous exercise, our doc-term matrix is composed of count vectors where each element in the vector is the number of times a word appeared in the document.

In this exercise, we will convert our matrix of count vectors to **TF-IDF** vectors,
hopefully giving a higher weight to more important words, not just more frequent ones.

Construct a TF-IDF doc-term matrix using the [TfidfVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer) from Scikit-learn. This implements the TF-IDF calculations seen in lectures.

Perform the same relevance ranking that we did in Exercise 3.1 by getting the dot product of the same query with your new TF-IDF doc-term matrix. Don't forget to convert the query string to a vector using the `transform()` method of the `TfidfVectorizer` this time.

> * Does the ranking change?
> * If so, what do you think could account for this?

<div class="submit">Submit your answers.</div>


## 4: Finding similar documents

### 4.1: Document similarity

Using the doc-term matrix from Exercise 3.2, use cosine similarity for each document pair to find which two documents are most similar to each other.
You can use the `cosine_similarity()` method from Scikit-learn for this.


> * Which document pair are most similar to each other?
> * Does it follow your intuition?

<div class="submit">Submit your answers.</div>


### 4.2: Querying with new documents

Suppose you are given two new documents that you have not seen so far:

````python
new_docs = [
    'Plane crash in Baden-Wuerttemberg',      # Doc 4a
	'The weather'                             # Doc 4b
]
`````

Construct the TF-IDF matrix for these unseen documents (use `transform()` again, not `fit_transform()`) and find the documents from our original corpus that are most similar to each
using cosine similarity.

> * Which document is most similar to Doc 4a?
> * How about Doc 4b?

<div class="submit">Submit your answers.</div>




## Suggested extensions:
1. Doc2vec is an extension of Word2vec that learns document embeddings as well as word vectors. Another way to build document embeddings is to sum up the embeddings of each word in a document weighted by word frequency or TF-IDF. Another strategy is to apply clustering on the document embeddings. Use these methods to find similar documents and evaluate their performance. Whichever method(s) you want to try, you would need a dataset with documents grouped according to categories or other criteria. This dataset from [Kaggle](https://www.kaggle.com/uciml/news-aggregator-dataset#uci-news-aggregator.csv) is a good start.

2. Cross-lingual word embeddings are embeddings that have been aligned for two or more languages. This means that words from different languages that have similar meanings will be close to each other in the embedding space. Use cross-lingual embeddings to match similar documents across languages. There are many pretrained cross-lingual embeddings available online, one example is from [FastText](https://fasttext.cc/docs/en/pretrained-vectors.html). To build cross-lingual document embeddings, you can sum up the embedding of each word in the document weighted by frequency or TF-IDF. You would need a multilingual dataset with some gold standard matching such as a parallel corpus. There are many available online, [Opus](http://opus.nlpl.eu/) is a good start.

3. You can easily train an LDA topic model using the [Gensim package](https://radimrehurek.com/gensim/models/ldamodel.html).
Once you have trained a model, you can analyse a document to get a **vector representation**
of it, where each dimension of the vector gives the strength of association between
the document and a particular topic. You can use these in a similar way to the
above exercises, to compare documents in a corpus. Compare this on a larger corpus,
with some carefully chosen queries or query documents, to the methods explored in
the assignment. You could further extend the model by including features in the
model training other than just document words (by presenting them to the training
routine as if they were special words). You might, for example, include metadata
about each document.

> Read the [documentation](https://radimrehurek.com/gensim/models/ldamodel.html) on training LDA with Gensim. It is generally a good idea to save the trained model so you can load it afterwards to inspect the learned parameters.   

#### Suggested papers on document embeddings (cross-lingual and monolingual):
* Balikas, Georgios, et al. "Cross-lingual document retrieval using regularized wasserstein distance." European Conference on Information Retrieval. Springer, Cham, 2018.
* Kusner, Matt, et al. "From word embeddings to document distances." International conference on machine learning. 2015.
* Litschko, Robert, et al. "Unsupervised cross-lingual information retrieval using monolingual data only." The 41st International ACM SIGIR Conference on Research & Development in Information Retrieval. 2018.
* Litschko, Robert, et al. "Evaluating resource-lean cross-lingual embedding models in unsupervised retrieval." Proceedings of the 42nd International ACM SIGIR Conference on Research and Development in Information Retrieval. 2019.
