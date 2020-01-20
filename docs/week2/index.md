# Week 2: NLU Pipeline and Evaluation

Carry out all the exercises below and submit your answers
[on Moodle](https://moodle.helsinki.fi/course/view.php?id=36809).
Also submit a single Python file containing your full
implementation.

## Exercise 1: NLP tools

### Exercise 1.1: Using NLTK
In the previous session, you have installed NLTK and used it to load a corpus. In this exercise, you will use NLTK to process sentences. More specifically, you will tokenize sentences and words, apply POS tagging on words, lemmatize them and remove any stop words in them.

You might need to download the following NLTK packages:
````python
import nltk
nltk.download('wordnet')
nltk.download('stopwords')
````


The following code imports the required NLTK modules for the task and calls the functions for tokenization, POS tagging and lemmatization. Run the code and answer the below questions.

````python
from nltk.tokenize import sent_tokenize, word_tokenize
from nltk.tag import pos_tag
from nltk.corpus import stopwords
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer() # used to lemmatize words.

text = "One morning I shot an elephant in my pajamas. How he got into my pajamas I'll never know." # by Groucho Marx

sentences = sent_tokenize(text)
print(sentences)

words = word_tokenize(sentences[0])
print(words)

pos = pos_tag(words)
print(pos)

print([lemmatizer.lemmatize(w) for w in ['elephants', 'go', 'goes', 'going', 'went', 'gone']])

stopWords = set(stopwords.words('english'))
print(stopWords)
````

> What is sentence tokenization?
>
> What is word tokenization?
>
> What is POS tagging and why is it useful?
>
> What is lemmatization and why is it useful?
>
> What kind of words were in `stopWords`? What is the purpose of defining a set of such words?
>
> Build your own NLP pipeline (a function named `process_text(text)`) that takes a paragraph as input, and splits the paragraph into sentences, applies word tokenization, POS tagging and lemmatization on all words. The function should return a list containing the processed sentences. The format of the returned processed text could be something like this
````python
[ # sentences
	[ # sentence 0, contains words
		(word, lemma, POS, …), # word 0, in sentence 0
    ...
	],
  ...
]
````
> Implement a function (`filter_text(text)`) that uses `process_text(text)` to process a paragraph and then removes stop words and words that are not verbs, adjectives or nouns (for descriptions of POS tags, [read this](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)).
>
> What words are left when you run this filter on the example input text above? (Groucho Marx)
>
> **Submit your written answers to the questions**

###  Exercise 1.2: Using spaCy
In this exercise, you will use an NLP tool other than NLTK, namely [*spaCy*](https://spacy.io/).
Last week, you installed *spaCy* and used it to recognize named entities in text. Let's see what else it has to offer.

Process the sentence in the previous example using the code below:

````python
import spacy # import the spaCy module
nlp = spacy.load("en") # load the English model

doc = nlp(text) # process the text (which is defined in the previous code)

for sent in doc.sents:
  for token in sent: # iterate over every token
    print(token.text, token.lemma_, token.pos_, token.tag_, token.dep_, token.shape_, token.is_alpha, token.is_stop)
  print()
````

When analyzing sentences, spaCy automatically runs the text through an NLP pipeline (tokenizer, tagger, parser and so on). Read and try out the code snippets in [this article](https://spacy.io/usage/spacy-101) (until "Word vectors and similarity") and [this](https://spacy.io/usage/linguistic-features) (until "Tokenizer data") to familiarize yourself with spaCy (mainly: tokenization, POS tagging, lemmatization, dependency parsing, noun phrase chunking and named entity recognition). Once done, answer the questions below:

> What is dependency parsing and how is it different than POS tagging?
> (A short answer is enough here. This comes up in later lectures, so we'll
< go into more detail then.)
>
> What is the difference between `token.pos_` and `token.tag_`?
>
> What is noun phrase chunking?
>
> What is named entity recognition? Describe any two types of entities that spaCy can recognize.
>
> **Submit your answers**


For additional reading regarding spaCy (optional):
* [An interactive course for using spaCy](https://course.spacy.io/)
* [A brief comparison between spaCy and other NLP tools.](https://spacy.io/usage/facts-figures)


### Exercise 1.3: Comparing different NLP tools

The goal of this exercise is to experiment with different NLP tools, know what they offer and compare them. The tools that you will use in this exercise are:

* **spaCy**: Using the code above. Alternatively, you can use [https://explosion.ai/demos/displacy](https://explosion.ai/demos/displacy) to visualize the parsed dependencies. (Uncheck *merge* options to see the full relations.)
* **Stanford CoreNLP**: Using [https://corenlp.run/](https://corenlp.run/)
* (*optional*) **NLTK**: Using the code you implemented above.


Try parsing a simple sentence (e.g. "I have a dog.") using the tools. Now parse the text given in the first exercise. Do the same for:

 * *Finger Lickin' Good.*
 * *Finger licking good.*

> From your observations, note differences between the results (e.g. parsed trees, POS tags, ... etc) of spaCy and CoreNLP? Briefly discuss the difference and any missing/correct/wrong results by the tools.
>
> **Submit your answers**


#### Other NLP tools (Optional)
In case you'd like to try out other NLP tools, here are some more:

* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/) *(Implemented in Java, follow this tutorial to use [Stanford CoreNLP from Python](https://www.khalidalnajjar.com/setup-use-stanford-corenlp-server-python/))*
* [flair](https://github.com/flairNLP/flair)
* [UralicNLP](https://github.com/mikahama/uralicNLP) (for processing Finnish and other Uralic languages)
* [AllenNLP](https://allennlp.org/tutorials)
* [Pattern](https://www.clips.uantwerpen.be/pages/pattern-en) (use `pip install pattern3` for *Python 3*)
* [TextBlob](https://textblob.readthedocs.io/): simple interface to English NLP tools
* [MaltParser](http://www.maltparser.org/): dependency parser (*implemented in Java*)


## Exercise 2: Evaluation

In these exercises, we will be running multiple evaluation metrics that can be used in assessing the quality and performance of some NLP methods.

### Exercise 2.1

Consider an information retrieval system that returns a retrieval set of 15 documents (`retrieved`).
Each document in `retrieved` is labelled as *relevant* (`'R'`) or *non-relevant* (`'N'`):   

````python
total_docs = 100
total_relevant = 10

retrieved = ['R', 'N', 'N', 'R', 'R', 'N', 'N', 'N',
             'R', 'N', 'R', 'N', 'N', 'R', 'R']

````

Continuing the snippet given above, compute the numbers of true positives, false positives, true negatives, and
false negatives. Then, compute the values of the following metrics (round the values to two decimal places):

> Precision
>
> Recall
>
> F-score with &beta; = 1 (also known as *F1-score*)
>
> Accuracy
>
> **Submit the values you computed for each metric**

### Exercise 2.2
Consider the following scenario: a database consists of 10,000 documents in total, of which 10 are relevant.    

> Is accuracy an appropriate metric for evaluating the performance of a retrieval system in this scenario?
Why/why not? Discuss briefly.
>
> **Submit your answer**


## Exercise 3: Evaluation of a POS tagger
In this exercise, we will use the Penn Treebank corpus provided by NLTK to obtain POS annotations, as gold standard:

````python
import nltk
nltk.download('treebank')
````

We will use 80% of sentences for training, and the remaining 20% for testing.  
The following code splits the corpus of sentences into training and test sentences,
and assigns test tokens and the correct tags into separate lists.

Download [ass5utils.py](ass5utils.py) into the same directory as
your source code, then run the below code to train the HMM with `training_sents`:

````python
from nltk.corpus import treebank
from nltk.tag.hmm import HiddenMarkovModelTagger
from ass5utils import split_corpus

training_sents, test_sents = split_corpus(treebank, 0.8)

test_tokens = [t[0] for s in test_sents for t in s]
correct_tags = [t[1] for s in test_sents for t in s]

hmm_tagger = HiddenMarkovModelTagger.train(training_sents)
````

### Exercise 3.1: Confusion matrix

Use the HMM to predict the tags for `test_tokens`, refer to [`nltk.tag.hmm`](https://www.nltk.org/api/nltk.tag.html?#nltk.tag.hmm.HiddenMarkovModelTagger.tag) for additional details.

Then, compute the confusion matrix between the predicted tags and `correct_tags`.  
You can use the
[`nltk.metrics.ConfusionMatrix`](https://www.nltk.org/api/nltk.metrics.html#nltk.metrics.confusionmatrix.ConfusionMatrix)
class for this exercise.

(In the confusion matrix, rows are the correct tags and columns are the predicted tags.
That is, an entry `cm[correct_tag, predicted_tag]` is the number of times a token with true tag `correct_tag` was
tagged with `predicted_tag`.)

> Which `(correct_tag, predicted_tag)` pair was the most common error? How many times did it occur?
>
> What is the overall accuracy of the HMM tagger? (Round the value to two decimal places.)
>
> Compute the precision, recall, and F1-score (&beta; = 1) for the tag `'NN'`. (Round the value to two decimal places.)
>
> **Submit the answers**


### Exercise 3.2: Comparison with baselines

We would like to know whether the HMM tagger is any good compared to a naive baseline and to the `spaCy`.

Implement the following function:
 * `random_tagger(tagset, tokens)`: given a list of tokens, assigns a POS tag randomly to each token.
 (The tagset is defined in [ass5utils.py](ass5utils.py).)

Compute the overall accuracy and F1-score of the random_tagger, and compare the values with the HMM and spaCy model.

> Which model has the best performance?
>
> What is the difference in accuracy (expressed in [percentage points](https://en.wikipedia.org/wiki/Percentage_point)) between the random baseline, HMM, and spaCy? (Round the value to one decimal place.)
>
> **Submit the name of the best model and accuracy difference**
