# Week 2: NLU Pipeline and Evaluation

**NB: This is an old assignment. It hasn't yet been updated for this year's course!**

Carry out all the exercises below and submit your answers
[on Moodle](https://moodle.helsinki.fi/mod/assign/view.php?id=1593564).
Also submit a single Python file containing your full
implementation.

## Exercise 1: NLP tools

### Exercise 1.1: Using NLTK
In the previous session, you have installed NLTK and used it to load a corpus. In this exercise, you will use NLTK to process sentences. More specifically, you will tokenize sentences and words, apply POS tagging on words, lemmatize them and remove any stop words in them.

The following code imports the required NLTK modules for the task and calls the functions for tokenization, pos tagging and lemmatization. Run the code and answer the below questions.

You might need to download the following NLTK packages:
````python
import nltk
nltk.download('wordnet')
nltk.download('stopwords')
````


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

* What is sentence tokenization?
* What is word tokenization?
* What is POS tagging and why is it useful?
* What is lemmatization and why is it useful?
* What kind of words were in `stopWords`? What is the purpose of defining a set of such words?
* Build your own NLP pipeline (a function named `process_text(text)`) that takes a paragraph as input, and splits the paragraph into sentences, applies word tokenization, POS tagging and lemmatization on all words. The function should return a list containing the processed sentences. The format of the returned processed text could be something like this
````python
[ # sentences
	[ # sentence 0, contains words
		(word, lemma, POS, …), # word 0, in sentence 0
    ...
	],
  ...
]
````
* Implement a function (`filter_text(text)`) that uses `process_text(text)` to process a paragraph and then removes stop words and words that are not verbs, adjectives or nouns (for descriptions of POS tags, [read this](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)).
* **Submit your answers**

###  Exercise 1.2: Using spaCy
In this exercise, you will install and use an a Natural Language Processing (NLP) tool other than NLTK, namely [*spaCy*](https://spacy.io/).

To install *spaCy*, open the terminal, load the virtual environment configured during the previous assignment and execute the following commands:

````sh
pip install -U spacy # install the package
python -m spacy download en # download the English model
````

Once installed, you can load and use the model as follows:

````python
import spacy # import the spaCy module
nlp = spacy.load("en") # load the English model

doc = nlp(text) # process the text (which is defined in the previous code)

for sent in doc.sents:
  for token in sent: # iterate over every token
    print(token.text, token.lemma_, token.pos_, token.tag_, token.dep_, token.shape_, token.is_alpha, token.is_stop)
  print()
````

When analyzing sentences, spaCy automatically runs the text throw an NLP pipeline (tokenizer, tagger, parser and so on). Read and try out the code snippets in [this article](https://spacy.io/usage/spacy-101) (until "Word vectors and similarity") and [this](https://spacy.io/usage/linguistic-features) (until "Tokenizer data") to familiarize yourself with spaCy (mainly: tokenization, POS tagging, lemmatization, dependency parsing, noun phrase chunking and named entity recognition).

Sample code to iterate over detected named entities:
````python
for ent in doc.ents: # for iterating over detected entities
  print(ent.text, ent.start_char, ent.end_char, ent.label_)
````

Sample code to iterate over noun chunks:
````python
for chunk in doc.noun_chunks: # for iterating over noun chunks
  print(chunk.text, chunk.root.text, chunk.root.dep_,
          chunk.root.head.text)
````

* What is dependency parsing and how is it different than POS tagging?
* What is the difference between `token.pos_` and `token.tag_`?
* What is noun phrase chunking?
* What is named entity recognition? Describe any two types of entities that spaCy can recognize.
* **Submit your answers**


For additional reading regarding spaCy (optional):
* [An interactive course for using spaCy](https://course.spacy.io/)
* [A brief comparison between spaCy and other NLP tools.](https://spacy.io/usage/facts-figures)


### Exercise 1.3: Comparing different NLP tools

The goal of this exercise is to experiment with different NLP tools, know what they offer and compare them. The tools that you will use in this exercise are:

* spaCy (Using the code above. Alternatively, you can use [https://explosion.ai/demos/displacy](https://explosion.ai/demos/displacy) to visualize the parsed dependencies. *Uncheck the merge options to see the full relations.*)
* Stanford CoreNLP (Using [https://corenlp.run/](https://corenlp.run/))
* (*optional*) NLTK, using the code you implemented.


Try parsing a simple sentence (e.g. "I have a dog.") using the tools. Now, parse the text given in the first exercise. Do the same for "Finger Lickin' Good.", "Finger licking good.", "Think Different." and "Think different.". Compare the results by the tools.

* From your observations, any differences between the results (e.g. parsed trees, POS tags, ... etc) of spaCy and CoreNLP? Briefly discuss the difference and any missing/correct/wrong results by the tools.
* **Submit your answers**

#### Other NLP tools (Optional)
In case you'd like to try out other NLP tools, here are some more:

* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/) *(Implemented in Java, follow this tutorial to use [Stanford CoreNLP from Python](https://www.khalidalnajjar.com/setup-use-stanford-corenlp-server-python/))*
* [AllenNLP](https://allennlp.org/tutorials)
* [Pattern](https://www.clips.uantwerpen.be/pages/pattern-en) (use `pip install pattern3` for *Python 3*)
* [MaltParser](http://www.maltparser.org/)  (*Implemented in Java*)
* [UralicNLP](https://github.com/mikahama/uralicNLP) (for processing Finnish and other Uralic languages)
* Others?


## Exercise 2: Pun generation
The goal of this exercise is to develop a simple application for generating food related puns. To do so, you will implement a method that accepts a simple expression as input, replaces a word in it with a new (punny) word and returns the new expression. You are expected to use either spaCy or your `process_text` for this exercise, in addition to NLTK's CMU library.


To get food-related words, we will query [Thesaurus Rex](http://ngrams.ucd.ie/therex3). Thesaurus Rex mines categorical relations and adjectival modifiers of nouns from the web. To query and use Thesaurus Rex's API, install the following packages:


````sh
pip install requests xmltodict
````

The below function sends a request to the API to get words falling under the category "food", parsers the result and returns it. The function returns a dictionary where its keys are the words and the values are the weights (representing how related the word is to the category "food").

````python
import requests, xmltodict, pickle, os

def food_words(file_path='./food_words.pkl'):
  if os.path.isfile(file_path): # load stored results
    with open(file_path, 'rb') as f:
      return pickle.load(f)

  url = 'http://ngrams.ucd.ie/therex3/common-nouns/head.action?head=food&ref=apple&xml=true'
  response = requests.get(url)
  result = xmltodict.parse(response.content)
  _root_content = result['HeadData']
  result_dict = dict(map(lambda r: tuple([r['#text'].replace('_', ' ').strip(), int(r['@weight'])]), _root_content['Members']['Member']))

  with open(file_path, 'wb') as f: # store the results locally (as a cache)
    pickle.dump(result_dict, f, pickle.HIGHEST_PROTOCOL)
  return result_dict
````

Now that you have access to food-related words, implement a function `make_punny(text)` that processes the input `text`, selects a token that is either a verb or noun at random, replaces it with a similar sounding food-related word from `food_words()`. You can implement the function to return more than one punny variation (5 at most). To measure the pronunciation similarity between words, we will employ the *CMU Pronouncing Dictionary* provided in NLTK and Levenshtein edit distance.

The below code loads the CMU dictionary and returns the pronunciation of a word. In case the module did not exist, run `nltk.download('cmudict')`.

````python
from nltk.corpus import cmudict
arpabet = cmudict.dict()
def pronounce(word):
  return arpabet[word.lower()][0] if word.lower() in arpabet else None # make sure the word is lowercased and exists in the dictionary
````

You can use the Python existing package `editdistance` (install it using `pip install editdistance`) to measure the Levenshtein edit distance. Here is an example of how to use `editdistance` and `pronounce`:

````python
import editdistance
distance = editdistance.eval(pronounce('pi'), pronounce('pie')) # 0 == identical pronunciation
````

Using the given code snippets, implement `make_punny(text)`. Feel free to add any custom improvements/measures to enhance the quality of puns (e.g. considering multiple punny words and presenting them to user, using the weights provided by Thesaurus Rex, ... etc).


* What are the punny expressions for "Jurassic Park" and "Life of Pi" produced by your method. Choose two custom movie titles and report the output of your method.
* **Submit your answers**




## Exercises from day 5: Evaluation

The following exercises were a separate day's exercises last year.


## Exercise 1: Basics


Consider an information retrieval system that returns a retrieval set of 15 documents (`retrieved`).
Each document in `retrieved` is labelled as *relevant* (`'R'`) or *non-relevant* (`'N'`):   

````python
total_docs = 100
total_relevant = 10

retrieved = ['R', 'N', 'N', 'R', 'R', 'N', 'N', 'N',
             'R', 'N', 'R', 'N', 'N', 'R', 'R']

````

### Exercise 1.1

Continuing the snippet given above, compute the numbers of true positives, false positives, true negatives, and
false negatives. Then, compute the values of the following metrics (round the values to two decimal places):

* Precision
* Recall
* F-score with &beta; = 1 (also known as *F1-score*)
* Accuracy
* **Submit the values you computed for each metric**

### Exercise 1.2

Consider the following scenario: a database consists of 10,000 documents in total, of which 10 are relevant.    

* Is accuracy an appropriate metric for evaluating the performance of a retrieval system in this scenario?
Why/why not? Discuss shortly.
* **Submit your answer**


## Exercise 2: Evaluation of a POS tagger

In exercises 2.1-3, we evaluate a POS tagger based on a hidden Markov model (HMM), which you
implemented on Day 3.

Today, we will again use the Penn Treebank corpus that you used yesterday.
You will already have downloaded yesterday using:
````python
import nltk
nltk.download('treebank')
````

We use 80% of sentences for training, and the remaining 20% for testing.  
The following code splits the corpus of sentences into training and test sentences,
and assigns test tokens and the correct tags into separate lists.

Train the HMM with `training_sents`, as in exercise 2 of Day 3.
Download [ass5utils.py](ass5utils.py) into the same directory as
your source code.

````python
from nltk.corpus import treebank
from nltk.tag.hmm import HiddenMarkovModelTagger
from ass5utils import split_corpus

training_sents, test_sents = split_corpus(treebank, 0.8)

test_tokens = [t[0] for s in test_sents for t in s]
correct_tags = [t[1] for s in test_sents for t in s]

hmm_tagger = HiddenMarkovModelTagger.train(training_sents)
````

### Exercise 2.1: Confusion matrix

Use the HMM to predict the tags for `test_tokens`.
(If you've forgotten how to do this, refer back to your code from day 3.)

Then, compute the confusion matrix between the predicted tags and `correct_tags`.  
You can use the
[`nltk.metrics.ConfusionMatrix`](https://www.nltk.org/api/nltk.metrics.html#nltk.metrics.confusionmatrix.ConfusionMatrix)
class for this exercise.

(In the confusion matrix, rows are the correct tags and columns are the predicted tags.
That is, an entry `cm[correct_tag, predicted_tag]` is the number of times a token with true tag `correct_tag` was
tagged with `predicted_tag`.)

* Which `(correct_tag, predicted_tag)` pair was the most common error? How many times did it occur?
* What is the overall accuracy of the HMM tagger? (Round the value to two decimal places.)
* Compute the precision, recall, and F1-score (&beta; = 1) for the tag `'NN'`. (Round the value to two decimal places.)
* **Submit the answers**


### Exercise 2.2: Comparison with baselines

We would like to know whether the HMM tagger is any good compared to naive baselines.

Now, implement the following functions:
 * `random_tagger(tagset, tokens)`: given a list of tokens, assigns a POS tag randomly to each token.
 (The tagset is defined in [ass5utils.py](ass5utils.py).)

 * `majority_tagger(training_sents, tokens)`: find the tag that is most common in the training sentences,
 and tag each token with this tag.

Compute the overall accuracy of both baselines, and compare the values with the HMM.

* Which baseline performs better?
* What is the difference in accuracy (expressed in [percentage points](https://en.wikipedia.org/wiki/Percentage_point)) between this baseline and the HMM? (Round the value to one decimal place.)   
* **Submit the name of baseline and accuracy difference**


### Exercise 2.3: Evaluation of HMM language model

Recall exercise 5 on Day 3, where you used the HMM as a language model.

Again, use the `log_probability()` method of the HMM to compute the total log-probability of test tokens.
(The input should be given as `(token, None)` pairs.)

* Compute the perplexity given the log-probability (round the value to two decimal places).
* What does the perplexity of a language model describe? Explain shortly.
* How could we find out whether the HMM language model is 'good'? Explain shortly.
* **Submit the perplexity value and explanations**

## Exercise 3: Text annotation

Consider the following sentences from Penn Treebank corpus:
````python
s1 = ['So', 'far', 'Mr.', 'Hahn', 'is', 'trying', 'to', 'entice', 'Nekoosa', 'into', 'negotiating', 'a', 'friendly',
'surrender', 'while', 'talking', 'tough']
s2 = ['Despite', 'the', 'economic', 'slowdown', 'there', 'are', 'few', 'clear', 'signs', 'that', 'growth', 'is',
'coming', 'to', 'a', 'halt']
s3 =  ['The', 'real', 'battle', 'is', 'over', 'who', 'will', 'control', 'that', 'market', 'and', 'reap',
'its', 'huge', 'rewards']
````


### Exercise 3.1

Annotate the sentences with appropriate POS tags.
The tags are described [here](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html).  

(It is not the aim of the exercise to annotate exactly according to guidelines,
so simply make your best guess of the correct tag.)       

* Give an example of a word/phrase you found difficult to annotate.  
* Why is this example difficult? Explain shortly.
* **Submit your answer as text**


### Exercise 3.2

The corresponding gold-standard tags of the sentences are below:

````python
tags1 = ['IN', 'RB', 'NNP', 'NNP', 'VBZ', 'VBG', 'TO', 'VB', 'NNP', 'IN', 'VBG', 'DT', 'JJ', 'NN', 'IN', 'VBG', 'JJ']
tags2 = ['IN', 'DT', 'JJ', 'NN', 'EX', 'VBP', 'JJ', 'JJ', 'NNS', 'IN', 'NN', 'VBZ', 'VBG', 'TO', 'DT', 'NN']
tags3 = ['DT', 'JJ', 'NN', 'VBZ', 'IN', 'WP', 'MD', 'VB', 'DT', 'NN', 'CC', 'VB', 'PRP$', 'JJ', 'NNS']
````

* Compute the *raw agreement rate* between your own annotations and the tags above.
**Submit the rate**  
