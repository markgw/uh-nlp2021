# Week 2: NLU Pipelines

Carry out all the exercises below and submit your answers
[on Moodle](https://moodle.helsinki.fi/course/view.php?id=44338).
Also submit a single Python file containing your full
implementation.

## Exercise 1: Questions from the lectures

### Question 1.1

You saw the following factual text and question, given respectively
as data source (among others) and user input to a question answering system.

* *A robotic co-pilot developed under DARPA’s ALIAS programme has already flown a light aircraft.*
* *What agency has created a computer that can pilot a plane?*

> **Submit a list at least 4 of steps (using formal names or just your own descriptions)
  that must be carried out by the system to extract information from the first sentence in a suitable
  internal/logical form for answering the question.**

### Question 1.2

Consider the following sentences:

1. *John loves Mary.*
2. *The number of moped-related crimes rose from 827 in 2012 to more than 23,000 last year.*

> **List three challenges encountered in extracting information from sentence 2
  that make it more difficult than sentence 1.**

### Question 1.3

> **Submit short answers to each of the following questions**

1. What is sentence tokenization?
2. What is word tokenization?
3. What is lemmatization and why is it useful?

## Exercise 2: Using NLTK
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

> What is POS tagging and why is it useful?
>
> What kind of words were in `stopWords`? What is the purpose of defining a set of such words?
>
> **Submit your answers**

Build your own NLP pipeline (a function named `process_text(text)`) that takes a paragraph as input, and splits the paragraph into sentences, applies word tokenization, POS tagging and lemmatization on all words. The function should return a list containing the processed sentences. The format of the returned processed text could be something like this
````python
[ # sentences
	[ # sentence 0, contains words
		(word, lemma, POS, …), # word 0, in sentence 0
    ...
	],
  ...
]
````

Implement a function (`filter_text(text)`) that uses `process_text(text)` to process a paragraph and then removes stop words and words that are not verbs, adjectives or nouns (for descriptions of POS tags, [read this](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)).

> What words are left when you run this filter on the example input text above? (Groucho Marx)
>
> **Submit your answer**


##  Exercise 3: Using spaCy
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

> What is dependency parsing and how is it different to POS tagging?
> (A short answer is enough here. We'll go into more detail in later lectures.)
>
> What is the difference between `token.pos_` and `token.tag_`?
>
> What is noun phrase chunking?
>
> Describe any two types of entities that spaCy can recognize.
>
> **Submit your answers**


For additional reading regarding spaCy (optional):
* [An interactive course for using spaCy](https://course.spacy.io/)
* [A brief comparison between spaCy and other NLP tools.](https://spacy.io/usage/facts-figures)


## Exercise 4: Comparing different NLP tools

The goal of this exercise is to experiment with different NLP tools, know what they offer and compare them. The tools that you will use in this exercise are:

* **spaCy**: Using the code above. Alternatively, you can use [https://explosion.ai/demos/displacy](https://explosion.ai/demos/displacy) to visualize the parsed dependencies. (Uncheck *merge* options to see the full relations.)
* **Stanford CoreNLP**: Using [https://corenlp.run/](https://corenlp.run/)
* (*optional*) **NLTK**: Using the code you implemented above.


Try parsing a simple sentence (e.g. "I have a dog.") using the tools. Now parse the text given in the first exercise. Do the same for:

 * *Finger Lickin' Good.*
 * *Finger licking good.*

> From your observations, note some differences between the results (e.g. parsed trees, POS tags)? Briefly discuss the differences and any missing/correct/wrong results by the tools.
>
> **Submit your answers**


## Other NLP tools (Optional)
In case you'd like to try out other NLP tools, here are some more:

* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/) *(Implemented in Java, follow this tutorial to use [Stanford CoreNLP from Python](https://www.khalidalnajjar.com/setup-use-stanford-corenlp-server-python/))*
* [flair](https://github.com/flairNLP/flair)
* [Turku Neural Parser](https://turkunlp.org/Turku-neural-parser-pipeline/): parsing pipeline implemented by the Turku NLP group, with models for Finnish and about 50 other languages.
* [UralicNLP](https://github.com/mikahama/uralicNLP) (for processing Finnish and other Uralic languages)
* [AllenNLP](https://allennlp.org/tutorials)
* [Pattern](https://www.clips.uantwerpen.be/pages/pattern-en) (use `pip install pattern3` for *Python 3*)
* [TextBlob](https://textblob.readthedocs.io/): simple interface to English NLP tools
* [MaltParser](http://www.maltparser.org/): dependency parser (*implemented in Java*)




## Final project suggestion: Pun generation
The idea here would be to develop a simple application that utilizes the NLP pipeline for generating food related puns. To do so, you will implement a method that accepts a simple expression as input, replaces a word in it with a new (punny) word and returns the new expression. You are expected to use either spaCy or your `process_text` for this exercise, in addition to NLTK's CMU library.

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

> What are the punny expressions for "Jurassic Park" and "Life of Pi" produced by your method. Choose two custom movie titles and report the output of your method.

Below are some suggestions that you may consider for your final project:

* Build a generic pun generator (e.g. using words in `wordnet` or a dictionary of lemmatized words)
* Consider specific prosody features (e.g. rhyme and alliteration)
* Consider additional aesthetics when picking the replacement word (e.g. sentiment, semantic, ... etc).
* Different pronunciation measurement (have a look at [abydos](https://github.com/chrislit/abydos))
* Use [`eSpeak NG`](https://github.com/espeak-ng/espeak-ng) to obtain International Phonetic Alphabet (IPA) transcription of words to measure sonic similarities better.
