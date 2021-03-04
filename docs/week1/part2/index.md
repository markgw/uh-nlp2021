# Week 1: Part II: Installing stuff, NLTK and SpaCy

[Part I](../part1/) provided an intro/refresher on Python programming.
Now we get ourselves ready to do some NLP in Python.

While Python has a good standard library, we are going to need specialized tools like the
**Natural Language Tool Kit (NLTK)**. Let's first install NLTK.

## Virtual environments

Installing a huge number of dependencies globally is a bad idea for a multitude of reasons. We'll want to set up a *virtual environment*, which is like a sandbox where we can play: Python modules you install in the virtual environment are not visible to other virtual environments. At the same time, the venv is not a virtual machine or a real sandbox: malicious code can do whatever it pleases.

In your shell, run the following command to set up a new virtual environment, replacing `<venv_path>` with some directory:
```shell
$ python3 -m venv <venv_path>
```
This part only needs to be ran once.

Next, we want to activate the virtual environment.
```shell
$ source <venv_path>/bin/activate
```
This command needs to be run again every time you start a new shell.

You can now start a Python interpreter by tying in
```shell
(venv) $ python
```

Note how you no longer need to type in `python3`, as in the first example of this page.

> Verify that the Python interpreter that started is running Python 3 by checking the version information on the first line printed. It should start with "Python 3.x.y" for some values of "x" and "y".

## Installing NLTK

Exit the Python interpreter. We'll next install some useful dependencies using PIP, the Python package manager.
```shell
(venv) $ pip install --upgrade pip
(venv) $ pip install nltk
```
The first line asks pip to upgrade itself. This does not need to be run every time. The second like installs the `nltk` package. Normally, if ran outside of the venv, this command would try to install `nltk` for every user on the computer. You would likely get an error since you are not (hopefully) running as a super user. If you want to install packages **outside of the venv** so that they are available globally for you only, use the flag `--user` to install into your home directory. **This flag is not needed when working in a venv**.

Open up the Python interpreter again, and run the command `import nltk`:
 ```shell
 (venv) $ python
Python 3.6.7 (default, Oct 22 2018, 11:32:17)
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import nltk
>>>
```
This loads the nltk module. If you got an error message, something went wrong in a previous step.


## Basic use of NLTK

Next up, we'll download a corpus that comes with the NTLK. In your Python interpreter, write `nltk.download()`. A window should pop up. Select the tab "Corpora", select the corpus "*inaugural*" and press "download". The status should switch from "not installed" to "installed". We've now downloaded a corpus of the inaugural speeches of US presidents. Close the window.

You should find yourself back in the Python interpreter. We'll also want to load the NLTK module `punkt`, which is allows us to easily tokenize text. Do so by running the command `nltk.download('punkt')`. You can also find it in the NLTK downloader's "models" tab.

We can now access the corpus via `nltk.corpus.inaugural`. For example, running `nltk.corpus.inaugural.fileids()` produces a list of all the files that make up the corpus.

```python
>>> nltk.corpus.inaugural.fileids()
['1789-Washington.txt', '1793-Washington.txt', <many more>]
```

Let's check how the length of the inaugural speech has developed over time:
```python
>>> for corp in nltk.corpus.inaugural.fileids():
...   print(corp, len(nltk.corpus.inaugural.words(corp)))
...
1789-Washington.txt 1538
1793-Washington.txt 147
1797-Adams.txt 2585
<etc>
```
> Eye over the list. Are the last few speeches long, short, or about average?


Next up, let's take a close look at Kennedy's speech in 1961 by loading it up as a list of sentences:
```python
>>> sents = nltk.corpus.inaugural.sents('1961-Kennedy.txt')
```

Observe the data loaded into the variable `sents`. It should contain a list of lists. The inner lists are sentences, where each element of the list is a token like a word or punctuation.

We can use the built-in `max` function to find the longest sentence:
```python
>>> max(sents, key=len)
['Let', 'the', 'word', 'go', 'forth', 'from', 'this', 'time', 'and', 'place', ...]
```
The `key` argument expects a method it calls for each element in the input list to determine it's numeric value. The same effect could be achieved more verbosely via `max(sents, key = lambda item: len(item))`

It's getting quite tedious to keep writing the `nltk.corpus.inaugural` part over and over again. Thankfully, this can be helped by writing `from nltk.corpus import unaugural`:

```python
>>> from nltk.corpus import inaugural
>>> inaugural.fileids()
['1789-Washington.txt', '1793-Washington.txt', ...]
```
This demonstrates how we can import individual submodules. We could even import individual methods or classes if we were so inclined.


Since the word "inaugural" is relatively hard to type, we can help our life further by importing the module by a different name:

```python
>>> from nltk.corpus import inaugural as corpus
>>> corpus.fileids()
['1789-Washington.txt', '1793-Washington.txt', ...]
>>>
```

As the first graded assignment, let's find the very longest sentence in the whole inaugural corpus and the president behind it.

First, we'll use a list comprehension to iterate over the individual speeches. The comprehension will, for each speach, add to the list a tuple containing the file of the speech and the longest sentence:

```python
>>> longest_sents = [(speech, max(corpus.sents(speech), key=len)) for speech in corpus.fileids()]
>>> longest_sents[0]
('1789-Washington.txt', ['I', 'dwell', 'on', 'this', 'prospect', 'with', 'every', 'satisfaction', ...])
```

> Ensure you understand what is happening in the list comprehension.

We can then use the `max` function and it's `key` argument to find the tuple with the longest sentence (counted in tokens):

```python
>>> longest = max(longest_sents, key = lambda item: len(item[1]))
```
> Why is the lambda expression needed here? What happens if you just use `key=len`?

Call `' '.join(longest[1])` (note the space inside quotes) to get a slightly nicer looking string representation of the longest sentence of any US presidential inaugural speech. Also check `longest[0]` for which speech this sentence is from.

> **MARKED ASSIGNMENT**
>
> Submit to Moodle who's speech contained the longest sentence, when the speech was given and what the longest sentence was.

## Simple NLP stuff

Next up, let's use NLTK to determine for each word which part of speech it is.

> Do this by passing the longest sentence (as list of words) to `nltk.pos_tag()` and storing the result. The result should look something like `[..., ('without', 'IN'), ('effect', 'NN'), ('.', '.')] `.

Let's figure out the distribution of the various POS tags. To do this easily, we can use the `Counter` class provided in the built-in package `collections`.

> Import the `Counter` class from the package `collections`

`Counter`, as the name implies, is used to count things. We want to count the POS tags, so we need to obtain a copy of the list of the tagged tokens with **only** the POS tags. That is, instead of `[..., ('without', 'IN'), ('effect', 'NN'), ('.', '.')]` we want `[..., 'IN', 'NN', '.']`.

> Construct the tag-only list using, e.g., a list comprehension.

We can now use `Counter` to determine the distribution of the POS tags. Note that `Counter` is an object and not a function, so we need to construct an instance.

> Call the constructor of `Counter` to obtain a new instance, passing the tags-only list as an argument to the constructor. Store the returned object in a variable.

Use the `most_common(n)` method of the object you just constructed to determine the 3 most common POS tags and their counts.

> **MARKED ASSIGNMENT**
>Return to Moodle the 3 most common POS tags, their counts, their meanings and the code you used to obtain the result.
>
> To find out what the tags mean, install the NLTK module "tagsets" and read the output of `nltk.help.upenn_tagset()`. Don't worry if you don't understand them completely, but try to get some idea by looking at the examples.


## SpaCy and entity recognition

Let us also set up another NLP library, SpaCy. Start by installing **inside your virtual environment** the `pip` package `spacy`. Recall how you installed NLTK above.

Next up, we need to install some pretrained models: training one ourselves would take large amounts of time, computation power and data, none of which we have. Also, the SpaCy default models are relatively good for our purposes here.

To install the English language default models, write `python -m spacy download en` in your terminal. **Note:** if you opened a new terminal, remember to source your virtual environment first!

The above should have downloaded a model called `en_core_web_sm` and constructed a shortcut called `en`. SpaCy also allows you to define in more details which models, exactly, you want to download and use. Optionally see the SpaCy's [documentation](https://spacy.io/usage/models#download) for more details.

>**MARKED ASSIGNMENT**
>
> Go to https://spacy.io/models/en and observe the three different English language model sets available. How much larger is `en_core_web_lg` compared to `en_core_web_sm` in terms of megabytes taken up by the model? Observe the NER F scores reported for the various English language models. A score of 100 would mean that the model was correct in all test cases, whereas a score of 0 would mean that it was wrong in all test cases. How much performance did the larger model gain for how many fold increase in model size when compared to the smallest model?

In Python, import `spacy` and then load up a module containing English language models by calling `nlp = spacy.load('en')`.

Next, try out SpaCy's default **entity recognition model** by writing up a sentence with a few entities (people, places, organizations) and storing it in a variable `sent`. Then call `nlp(sent).ents` and observe the detected entities. Try out a few variations of inputs.

>**MARKED ASSIGNMENT**
>
> What entities does SpaCy's `en` module detect from the sentence `"President Obama said to reporters from the Washington Post that the Federal Reserve had overstepped in its decision to decrease the margins on inter-bank loans last Wednesday"`? What about if you remove the first word?
