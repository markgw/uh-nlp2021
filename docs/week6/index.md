# Week 5: Vectors, Lexical Semantics and Word Embeddings

Carry out all the exercises below
and submit your answers on
[Moodle](https://moodle.helsinki.fi/course/view.php?id=33565#section-5).
Also submit a single Python file containing your full implementation.  



## Exercise 1: Document-term matrix

In this exercise, we will build a simple document-term matrix for the documents provided.

````python
documents = ['Wage conflict in retail business grows',
			 'Higher wages for cafeteria employees',
			 'Retailing Wage Dispute Expands',
			 'Train Crash Near Petershausen',
			 'Five Deaths in Crash of Police Helicopter']
````

### Exercise 1.1: Step-by-step construction of the doc-term matrix
Pre-process each document by converting it to lowercase, tokenize, remove stopwords and then lemmatize each token.

Construct the vocabulary of your pre-processed corpus and then construct a document-term matrix by going through each document and checking if a vocabulary word is present or not.

The shape of the matrix will be the number of documents by the vocabulary size `(n_docs x vocab_size)`.

> * What is the shape of your matrix?
> * **Submit the matrix shape**


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

> How does your doc-term matrix from 1.1 compare to your doc-term matrix
> from 1.2? Are they exactly the same or are their differences?
>
> If they are different, what could account for such difference?
>
> *Submit your answers*

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

> * Does it produce the same results?
> * **Submit your answers. Include the dot products of the query with the unnormalized and normalized doc-term matrices.**

### Exercise 2.2: Using TF-IDF to weight words

In the previous exercise, our doc-term matrix is composed of count vectors where each element in the vector is the number of times a word appeared in the document.

In this exercise, we will convert our doc-term matrix which is composed of count vectors to TF-IDF vectors.
Construct a TF-IDF doc-term matrix using the [TfidfVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer) from Scikit-learn. This implements the TF-IDF calculations seen in lectures.

Perform the same relevance ranking that we did in Exercise 2.1 by getting the dot product of the same query with your new TF-IDF doc-term matrix. Don't forget to convert the query string to a vector using the `transform()` method of the `TfidfVectorizer` this time.

> * Does the ranking change?
> * If so, what do you think could account for this?
> * **Submit your answers**


## Exercise 3: Finding similar documents

### Exercise 3.1

Using the doc-term matrix from Exercise 2.2, use cosine similarity for each document pair to find which two documents are most similar to each other.
You can use the `cosine_similarity()` method from Scikit-learn for this.


> * Which document pair are most similar to each other?
> * Does it follow your intuition?
> * *Submit your answers*

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

> * Which document is most similar to Doc 3a?
> * How about Doc 3b?
> * **Submit your answers to these questions and those above**


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
> * How many documents are in the corpus?
> * *Submit your answer*

### Exercise 4.2: Train a topic model with Gensim

Once we have loaded our dataset, we need to do some standard pre-processing on each document as in the previous exercises.

Next, train an LDA topic model for 10 topics on the pre-processed data. Read the [documentation](https://radimrehurek.com/gensim/models/ldamodel.html) on how to train an LDA topic model using Gensim. It is generally a good idea to save the trained model so you can load it afterwards to inspect the learned parameters.   

> * What are the top 5 words for each topic? Tip: check out the `show_topic()` method or similar methods.
> * *Submit your answer*

### Exercise 5: Word embeddings
In this exercise, we will train some word embeddings and do some simple queries on the trained model. Gensim also has modules for loading and training word embeddings. Take a look at the [documentation](https://radimrehurek.com/gensim/models/word2vec.html#module-gensim.models.word2vec).

Normally we would use very large corpora with millions of tokens to train word embeddings but since this is just an exercise, we will use the small `common_texts` corpora provided by Gensim.
Use the following code snippet to train Word2Vec embeddings:
````python
from gensim.test.utils import common_texts
from gensim.models import Word2Vec

model = Word2Vec(common_texts, size=100, window=5, min_count=1, workers=4)
# optional but there's no harm in saving the trained model
model.save("word2vec.model")
````

> * What is the vocabulary size of your model?
> * *Submit your answer*

### Exercise 5.1: Finding similar words using word embeddings

After training your model, use the `similar_by_word()` method to find the word most similar to the following words (excluding itself):
- system
- human
- trees

> * Do the similar words look reasonable to you? Discuss why or why not.
> * *Submit your answer*

## Suggested extensions:
1. Doc2vec is an extension of Word2vec that learns document embeddings as well as word vectors. Another way to build document embeddings is to sum up the embeddings of each word in a document weighted by word frequency or TF-IDF. Another strategy is to apply clustering on the document embeddings. Use these methods to find similar documents and evaluate their performance. Whichever method(s) you want to try, you would need a dataset with documents grouped according to categories or other criteria. This dataset from [Kaggle](https://www.kaggle.com/uciml/news-aggregator-dataset#uci-news-aggregator.csv) is a good start.

2. Cross-lingual word embeddings are embeddings that have been aligned for two or more languages. This means that words from different languages that have similar meanings will be close to each other in the embedding space. Use cross-lingual embeddings to match similar documents across languages. There are many pretrained cross-lingual embeddings available online, one example is from [FastText](https://fasttext.cc/docs/en/pretrained-vectors.html). To build cross-lingual document embeddings, you can sum up the embedding of each word in the document weighted by frequency or TF-IDF. You would need a multilingual dataset with some gold standard matching such as a parallel corpus. There are many available online, [Opus](http://opus.nlpl.eu/) is a good start.

#### Suggested papers on document embeddings (cross-lingual and monolingual):
* Balikas, Georgios, et al. "Cross-lingual document retrieval using regularized wasserstein distance." European Conference on Information Retrieval. Springer, Cham, 2018.
* Kusner, Matt, et al. "From word embeddings to document distances." International conference on machine learning. 2015.
* Litschko, Robert, et al. "Unsupervised cross-lingual information retrieval using monolingual data only." The 41st International ACM SIGIR Conference on Research & Development in Information Retrieval. 2018.
* Litschko, Robert, et al. "Evaluating resource-lean cross-lingual embedding models in unsupervised retrieval." Proceedings of the 42nd International ACM SIGIR Conference on Research and Development in Information Retrieval. 2019.
