# Day 7: Vectors and Lexical Semantics

Carry out all the exercises below
and submit your answers on
[Moodle](https://moodle.helsinki.fi/course/view.php?id=33565#section-5).
Also submit a single Python file containing your full implementation.  

## Exercise 0: Load the documents

````python
documents = ['Wage conflict in retail business grows',
			 'Higher wages for cafeteria employees',
			 'Retailing Wage Dispute Expands',
			 'Train Crash Near Petershausen',
			 'Five Deaths in Crash of Police Helicopter']
````

## Exercise 1: Document-term matrix

In this exercise, we will build a simple document-term matrix for the documents in Exercise 0.

### Exercise 1.1: Step-by-step construction of the doc-term matrix
Pre-process each document by converting it to lowercase, tokenize, remove stopwords and then lemmatize each token. 

Construct the vocabulary of your pre-processed corpus. 

Construct a document-term matrix by going through each document and checking if a vocabulary word is present or not.

The shape of the matrix will be the no. of documents by the vocabulary size `(n_docs x vocab_size)`.

 * What is the shape of your matrix?
 * **Submit the matrix shape**


### Exercise 1.2: Using scikit-learn to build the document-term matrix

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
At the bottom of the page is a code snippet to build count vectors for each document. You can easily convert these to a binary doc-term matrix.

````python
from sklearn.feature_extraction.text import CountVectorizer

vectorizer = CountVectorizer()
X = vectorizer.fit_transform(corpus)
counts = X.toarray()  # Get the doc-term count matrix
dt = counts > 0       # Convert to a binary matrix
doc_term_mat = dt * 1 # If you prefer, represent as 1s and 0s
````

How does your doc-term matrix from 1.1 compare to your doc-term matrix from 1.2? Are they exactly the same or are their differences?
If they are different, what could account for such difference?

For the next exercises, we will make use the doc-term matrix with count vectors produced by the `CountVectorizer`.


## Exercise 2: Ranking documents by query

### Exercise 2.1: Using the dot product to rank documents

Suppose you have the query *'retail wages'*. Rank the documents by relevance to this query by getting the dot product of the query by the doc-term matrix.
To convert the query string into a vector, use the `transform()` method of the vectorizer you created in the previous exercise. Remember that the vectorizer expects a list of strings.

Use `numpy`'s [`dot()`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html)
to compute dot products.

 * Which document is the most relevant?
 * Does it align with your intuition?

> If necessary, [remind yourself](https://mathinsight.org/matrix_vector_multiplication)
> of what happens when you
> take a dot product of a matrix and vector. Looking
> at the diagrams on the lecture slides might also help.

Normalize the count vectors of the doc-term matrix by the document length and perform the same relevance ranking.

 * Does it produce the same results?
 * **Submit your answers. Include the dot products of the query with the unnormalized and normalized doc-term matrices.**

### Exercise 2.2: Using TF-IDF to weight words

In the previous exercise, our doc-term matrix is composed of count vectors where each element in the vector is the number of times a word appeared in the document.

In this exercise, we will convert our doc-term matrix which is composed of count vectors to TF-IDF vectors.
Construct a TF-IDF doc-term matrix using the [TfidfVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer) from Scikit-learn. This implements the TF-IDF calculations seen in lectures.

Perform the same relevance ranking that we did in Exercise 2.1 by getting the dot product of the same query with your new TF-IDF doc-term matrix. Don't forget to convert the query string to a vector using the `transform()` method of the `TfidfVectorizer` this time.

 * Does the ranking change?
 * If so, what do you think could account for this?
 * **Submit your answers**


## Exercise 3: Finding similar documents

### Exercise 3.1

Using the doc-term matrix from Exercise 2.2, use cosine similarity for each document pair to find which two documents are most similar to each other.
You can use the `cosine_similarity()` method from Scikit-learn for this.

Tip: The ````itertools```` package can produce the document pairs so you don't have to construct them yourselves.
````python
import itertools
doc_count = len(documents)
doc_list = [i for i in range(doc_count)]
doc_pairs = list(itertools.combinations(doc_list, 2))
# doc_pairs contain tuples where each tuple is a pair of document index numbers
````

 * Which document pair are most similar to each other?
 * Does it follow your intuition?

### Exercise 3.2

Suppose you are given two new documents that you have not seen so far:

````python
new_docs = [
    'Plane crash in Baden-Wuerttemberg',          # Doc 3a
	'The weather'                             # Doc 3b
]
`````

Construct the TF-IDF matrix for these unseen documents (use `transform()` again, not `fit_transform()`) and find the documents from our original corpus that are most similar to each
using cosine similarity.

 * Which document is most similar to the first new document (Doc 3a)?
 * How about the second one (Doc 3b)?
 * **Submit your answers to these questions and those above**


## Exercise 4: Topic modelling

We will use the [Gensim package](https://radimrehurek.com/gensim/models/ldamodel.html) to train topic models. 

Check whether Gensim is installed and importable:
````python
import gensim
````

If not, install it in your virtual environment:
````sh
pip install gensim
````

### Exercise 4.1: Load 20 Newsgroups dataset

Topic modelling is more suitable for larger corpora therefore we will use the 20 newsgroups dataset from Scikit-learn.
````python
from sklearn.datasets import fetch_20newsgroups
data = fetch_20newsgroups(subset='train', shuffle=True, random_state=42).data
````
How many documents are in the corpus?

### Exercise 4.2: Train a topic model with Gensim

Once we have loaded our dataset, we need to do some standard pre-processing on each document as in the previous exercises. 

Next, train an LDA topic model for 10 topics on the pre-processed data. Read the documentation on how to train an LDA topic model using Gensim. It is generally a good idea to save the trained model so you can load it afterwards to inspect the learned parameters.   

What are the top 10 words for each topic?

### Exercise 5: Word embeddings
In this exercise, we will train some word embeddings and do some simple queries on the trained model. Gensim also has modules for loading and training word embeddings, documentation [here] (https://radimrehurek.com/gensim/models/word2vec.html#module-gensim.models.word2vec). 
Use this code snippet to train Word2Vec embeddings from the common_texts corpus in Gensim:
````python
from gensim.test.utils import common_texts
from gensim.models import Word2Vec

model = Word2Vec(common_texts, size=100, window=5, min_count=1, workers=4)
model.save("your_model_filename")
````
### Exercise 5.1: Finding similar words using word embeddings

````python
# Load your saved word embeddings
model = Word2Vec.load("your_model_filename")
````
What is the vocabulary size of your model?
After loading your model, use the `similar_by_word()' method to find the word most similar to the following words (excluding itself):
- system
- human
- trees


## Suggested extensions:
1. Doc2vec is an extension of Word2vec that learns vectors vectors for each document in a corpus as well as word vectors. Another way to build document vectors is to get the weighted sum of embedding of each word in a documents. Use both methods to find similar documents and evaluate their performance.
2. Cross-lingual word embeddings are embeddings that have been aligned for two or more languages. This means that words from different languages that have similar meanings will be close to each other in the embedding space. Use cross-lingual embeddings to match similar documents across languages. There are many pretrained cross-lingual embeddings available online, one example is from [FastText](https://fasttext.cc/docs/en/pretrained-vectors.html). To build cross-lingual document embeddings, you can sum up the embedding of each word in the document weighted by frequency or TF-IDF. You would need a multilingual dataset with some gold standard matching such as a parallel corpus. There are many available online, [Opus](http://opus.nlpl.eu/) is a good start.






