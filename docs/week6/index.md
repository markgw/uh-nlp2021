# Week 6: NLG and Dialogue

**NB: These assignments can still change, do not start working on them yet.**

Carry out all the exercises below and submit your answers on Moodle. Also submit a single Python file containing your full implementation.

## Exercise 1: The End-to-End NLG Challenge

Read the sections "Motivation" and "The Task" from the website of the [End-to-End NLG Challenge](http://www.macs.hw.ac.uk/InteractionLab/E2E/). Observe especially the MR format and the example natural language reference associated with it. Download the dataset from the website and inspect the file `devset.csv`. For the purposes of this week's assignments, the other files in the archive do not exist and you are not supposed to do anything with them.

> **Submit to Moodle** your answer to the following questions:
> 1) How difficult does the task appear to you?
> 2) Observe the scores reported in the section "Baseline System". Are they meaningful in isolation?
> 3) What are your thoughts on the variety of language in the references of the devset?

## Exercise 2: Trivial NLG

Download [ass6utils.py](/ass6utils.py) and store it in the same directory as `devset.csv`. In the same directory set up a python file with the following contents:

```python
from ass6utils import read_file, score, MeaningRepresentation
from typing import Callable, List, Optional
import random

meaning_representations, references = read_file('devset.csv')

def generate_trivial(mr: MeaningRepresentation) -> str:
    """
    Trivial NLG
    """
    return "{} is a {} {}.".format(mr.name, mr.food, mr.eat_type)


def evaluate(
    generator: Callable[[MeaningRepresentation], str],
    meaning_representations: List[MeaningRepresentation],
    references: List[List[str]],
) -> None:
    for _ in range(10):
        print(generator(random.choice(meaning_representations)))
    print("\n")
    score(generator, meaning_representations, references)
    print("\n----\n")


evaluate(generate_trivial, meaning_representations, references)
```

Familiarize yourself with the `MeaningRepresentation` class in `ass6utils.py`, especially in terms of what fields it contains. The code contains type hints, but you are free to ignore them. You don't have to add them to your code.

Run the code a few times (5 or so) and observe the results. Note that the `score` method called inside `evaluate` applies your NLG-method to the whole `devset` corpus, not just the ten random samples shown to you.

> **Submit to Moodle** your answers to the following questions (one or two sentences per questions is enough):
> 1) What kinds of scores is this extremely simple system achieving?
> 2) How do they compare to the baseline results on the challenge's website?
> 3) Do you observe any problems with the output (other than it being so short)?

## Exercise 3: Less simple NLG

Write a **new** generation function that realizes the three features in `generate_trivial` but **leave the original function untouched**. Your new system should inspect which of the three fields are not `None` and based on that decide what to output.

You can use the following as a starting point:
```python
def generate_2(mr: MeaningRepresentation) -> str:
    if mr.name and mr.food and mr.eat_type:
        return "{} is a {} {}.".format(mr.name, mr.food, mr.eat_type)
    elif mr.name and mr.food:
        raise NotImplementedError("Something needs to go here")
    elif mr.name and mr.eat_type:
        raise NotImplementedError("Something needs to go here")
    else:
        raise NotImplementedError("Something needs to go here")
```

Evaluate this improved version by calling `evaluate(generate_2, meaning_representations, references)`. 
> **Submit to Moodle** your answers to the following questions (one or two sentences per answer is sufficient):
> 1) Did your changes improve the evaluation scores?
> 2) Let us assume that the name is always present, but that all other features are optional. This means that if the MR consisted of only a name, there would be 2^0 = 1 variations of features being present or absent. In the above case, with name and two optional features, we had 2^2 = 4 variations of features being present or absent. How many variatations are there (i.e. how many if-statements would we need) for the full meaning representation in the `ass6utils.py` file?
> 3) How many variations would there be if we introduced another feature into the meaning representation?

## Exercise 4: Finding the popular choice

Write code that delexicalizes each reference available in the devset, e.g. turning `Aromi is a coffee shop, which offers Chinese food, and has a customer rating of 5 out of 5. It is located in a riverside area` into `X-NAME is a X-EAT-TYPE, which offers X-FOOD food, and has a customer rating of X-CUSTOMER-RATING. It is located in a X-AREA area`.

To do this, you'll need to replace words from each reference based on what values the relevant MR has. Note: this should not require tokenization. Ignore the `family_friendly` field when delexicalizing.

Hint: `for mr, refs in zip(meaning_representations, references)` might be useful, assuming `meaning_representations` is of type `List[MeaningRepresentation]` and `references` is of type `List[List[str]]`, like those obtained from calling `read_file()`.

After obtaining the delexicalized references, use `Counter` (recall first week's exercises) to determine the 10 most common reference formats. 

Take the most common reference type as a starting point, and write a function that realizes an arbitrary `MeaningRepresentation` into that sentence. Do **not** overwrite your previous code. Do **not** special case `None`: having them in the output is fine. Ignore the `family_friendly` field for now.

> **Submit to Moodle** your answers to the following questions:
> 1) What is the most common delexicalized reference? How many instances of it are in the devset?
> 2) Do you see any obvious patterns in the delexicalized references?
> 3) Evaluate your new generation function as above, how does it perform compared to the two previous functions?

## Exercise 5: Helpers for articles

Create the following helper function with a working implementation:
```python
def get_indefinite_article(word: str) -> str:
    """
    Returns either "a" or "an" depending on whether the input word's 
    *pronunciation* starts with a vowel sound (A, E, I, O, U).

    Pronunciations are retrieved from `nltk.corpus.cmudict`. Unknown 
    words return based on the first character in the word.
    """
    raise NotImplementedError()
```

Use the following helper to retrieve the pronunciation of the word (you need to run `nltk.download('cmudict')` at least once beforehand):
```python
from nltk.corpus import cmudict
pronunciations = cmudict.dict()
def pronounce(word: str) -> Optional[List[str]]:
    """
    Returns a pronunciation of the word supplied as a parameter.

    If the word is unknown, returns None. 
    
    For known words, output is a list of strings wherein each string
    corresponds to a phoneme. If the word has multiple known
    pronunciations, returns an arbitrary one of those.

    Example:
    >>> pronounce("Hello")
    ['HH', 'AH0', 'L', 'OW1']
    """
    word = word.lower()
    if word not in pronunciations:
        return None
    return pronunciations[word][0]
```

You can test your code with the following `assert` statements:
```python
assert get_indefinite_article("dog") == "a"
assert get_indefinite_article("fish") == "a"
assert get_indefinite_article("university") == "a"
assert get_indefinite_article("utopia") == "a"
assert get_indefinite_article("idiot") == "an"
assert get_indefinite_article("element") == "an"
assert get_indefinite_article("honor") == "an"
assert get_indefinite_article("heirloom") == "an"
```

Create also the following function with a working implementation:
```python
def realize_articles(text: str) -> str:
    """
    Replaces instances of "INDEF_ART" in text with the suitable form of the
    indefinite article ("a" or "an") as necessitated by the following word.

    Internally calls get_indefinite_article(). Input is tokenized using
    nltk.tokenize.treebank.TreebankWordTokenizer.tokenize() and detokenized 
    using nltk.tokenize.treebank.TreebankWordDetokenize.detokenize(). 
    
    Capitalization is ignored, meaning a sentence-first INDEF_ART can be
    realized fully lowercase. 
    """
    raise NotImplementedError()
```

You can test your code with the followin `assert` statements:
```python
assert realize_articles("This is INDEF_ART example.") == "This is an example."
assert realize_articles("This is INDEF_ART test.") == "This is a test."
assert realize_articles("This was, truly, INDEF_ART honor.") == "This was, truly, an honor."
```

You should only create the (de)tokenizer once, storing it outside the function, rather than creating a new instance every time the function is called. The same holds for the `cmudict.dict()`.

## Exercise 6: A helper for lists

Create the following helper function with a working implementation:
```python
def combine(components: List[Optional[str]], conjunction: str = " and ") -> Optional[str]:
    """
    Produces a string representation containing the non-None values in 
    `components`.

    The string representation consists of the non-None values in the list 
    separated by the string ", ". The exception are the last and 
    second-to-last components which are instead separated by `conjunction`, 
    by default the string " and ". None values in `components` are ignored. 
    In case `components` is empty or contains only None values, returns None.
    """
    raise NotImplementedError()
```

You can test your code with the following `assert` statements:
```python
assert combine(["a"]) == "a"
assert combine(["a", "b"]) == "a and b"
assert combine(["a", "b", "c"]) == "a, b and c"
assert combine(["a", "b", "c", "d"]) == "a, b, c and d"
assert combine(["a", "b"], conjunction=" or ") == "a or b"
assert combine([]) is None
assert combine([None]) is None
assert combine(["a", None, "b"]) == "a and b"
```

## Exercise 7: The big one

Implement a generator function that gracefully realizes all values in the meaning representations. Ensure that there are no `None`s in your output. Note that certain fields can have values of multiple forms:

- `customer_rating` can be either a word (e.g. "average") or a score (e.g. "1 out of 5")
- `family_friendly` can be either "yes", "no" or `None`. In case it's `None`, say nothing about family friendliness. 
- `price_range` can be either a range (e.g. "£20-25") or a word (e.g. "cheap").

Make sure the produced text makes sense in both cases. You will likely need to check which version of the value the MR has and select on two slightly different phrasings based on that.

Whatever you do, do **not** simply extend the code from Exercise 3 into a 100+ line long `if-elif-elif-elif...` statement.

A good starting place is to come up with an example output, e.g. `"The Eagle is a family-friendly coffee shop serving English food. It is located in city centre, near Burger King. It has prices in the range of £20-25 and has a high customer rating."`.

Here, having family friendliness undefined (`family_friendly` is `None`) is easy to achieve by simply omitting `"family-friendly"` from the output, but it's not so easy to negate the statement in the above format. For that, we can instead output `"It is not family friendly."` at the end. That is, depending on the `family_friendly` value, the output could be

- If `family_friendly` is "yes": `"The Eagle is a family-friendly coffee shop serving English food. It is located in city centre, near Burger King. It has prices in the range of £20-25 and has a high customer rating."`
- If `family_friendly` is `None`: `"The Eagle is coffee shop serving English food. It is located in city centre, near Burger King. It has prices in the range of £20-25 and has a high customer rating."`
- If `family_friendly` is "no": `"The Eagle is coffee shop serving English food. It is located in city centre, near Burger King. It has prices in the range of £20-25 and has a high customer rating. It is not family friendly."`

It might be a good idea to generate the text in chunks, e.g. as follows:

```
[
    [
        [The Eagle] 
        is 
        a 
        [family-friendly] 
        [coffee shop] 
        [
            serving 
            [English]
            food
        ]
        .
    ]
    [
        It 
        is 
        located
        [
            in 
            [city centre]
        ]
        ,
        [
            near 
            [Burger King]
        ]
        .
    ]
    [
        It 
        [
            has 
            prices 
            in 
            the 
            range 
            of 
            [£20-25]
        ]
        and
        [ 
            has 
            a 
            [high] 
            customer 
            rating
        ]
        .
    ]
]
```

If possible, take advantage of the helper functions created in the previous assignments. For example, the chunk containing the `area` and `near` values could be generated like this:

```python
    def location(mr: MeaningRepresentation) -> Optional[str]:
        area = "in {}".format(mr.area) if mr.area else None
        near = "near {}".format(mr.near) if mr.near else None
        if area is None and near is None:
            return None
        return "It is located {}.".format(
            combine([area, near], conjunction=", ")
        )
```
The call to `combine` handles the possibly `None` values of `area` and `near` automatically: only the case of both being `None` at the same time needs to be handled separately. The same approach can be used to generate the other chunks. Finally, in the end, combine all the chunks into a single string.

Take care to handle all instances of "a" and "an" using the helper functions if they are, or could be, followed by text from the meaning representation. For example, the "a" preceding "family-friendly" in the above example could also be "an" in a case where `family_friendly` was `"no"` or `None` and `eat_type` was `"inn"` (even if that value doesn't exist in the dataset we are working with).

**Evaluating this implementation might take many minutes, mainly because the pronounciation lookups are slow**. When developing the solution, consider temporarily commenting out the call to `score()` and just looking at the example outputs. When working on later exercises, consider commenting out the call to `evaluate()`.

> **Submit to Moodle** the output of calling `evaluate()` on your generator, both the examples and the numerical results. You can either build your generator along the above descriptio, or do something different.


## Exercise 8: Reflect on the complexity

>**Submit to Moodle** your answers to the following questions.
> 1) How difficult would it be to modify the system to produce a wider variety of sentences, for example by randomly ordering elements in the output?
> 2) Think of another language you speak. How much work would it be to translate the system to that language compared to this initial implementation? Try to consider cases like the "a" vs. "an" in English. Give examples of difficult things you come up with, if any.
> 3) Using the Gatt & Krahmer classification (Refer to slides), how would you characterize the system you built? Why?
> 4) Think back on your answers to Exercise #1. Did the task turn out easier or more difficult than you anticipated?
> 5) Think about the pros and cons of the neural systems as discussed in the lecture. Do you think this task is good for them (consider the data, the complexity etc.)? Do you expect them to fare better than "classical" systems?
> 6) How do the Baseline scores on the E2E website compare to your scores? How did you compare to the other system reported in Table 3 of the [Findings of the E2E NLG Challenge -paper](https://arxiv.org/pdf/1810.01170.pdf)?
> 7) Look at the same table. Check from the caption how the colors match the system architectures. How are the rule-based and template-based systems faring against the seq2seq and other data-driven systems? Does this match your expectation from before?

**NB:** Regarding the evaluation, note that we are running our evaluation on a different dataset than that which produced the results in the table on the E2E Challenge website. You are free to also evaluate on the larger dataset but that might take a long time and is completely optional.


## Exercise 9: Explore BLEU

Import the `bleu_single` method from `ass6utils.py`. Pick some NL realisation, either from those you generated or from the `devset.csv`. Call it the *reference*.

Try out different modifications to the reference and calculate the BLEU scores between the original and the modified reference. Try to come up with a pair of modifications where candidate #1 has the same logical content (i.e. same information) as the reference and candidate #2 contains some falsehood, but the BLEU scores rank candidate #2 higher than candidate #1.

>**Submit to Moodle** the reference and the candidates you found together with the BLEU scores. What does this tell you about the BLEU scores as a metric? What is the problem with the way we are using the BLEU score? Recall the assumptions behind these kinds of metrics from the slides.

## Exercise 10: Human Evaluation

>**Submit to Moodle** your answers to the following questions. A few sentences each is sufficient.
> 1) What kinds of questions would you ask if you were to conduct an intrinstic human evaluation on this task?
> 2) Can you come up with an extrinsic human evaluation for this task?
> 3) Read Section 4.2 from the [Findings of the E2E NLG Challenge](https://arxiv.org/pdf/1810.01170.pdf) paper. How did seq2seq systems compare to other interms of naturalness and quality?
> 4) Do you think naturalness or quality (~correctness) is more important for a system describing (perhaps recommending) restaurants? Come up with an example of both a system where correctness is much more important than fluency, and one where the reverse holds true.

## Possible extensions for the final project

Here are a few ways you might extend this week's assignment for the final project. Some are more laborous than others.

### 1. More variation
Make the system output more varied by adding in multiple variants in terms of words used or the ordering of the information. Then randomly pick from among them. You might want to consider evaluating on the full `testset.csv`, but it's fine to keep using the `devset.csv` if that takes too long.

### 2. More natural output
Change the output of the system so that it's more natural in your eyes. You might want to consider evaluating on the full `testset.csv`, but it's fine to keep using the `devset.csv` if that takes too long.

### 3. Contextual generation
Make the system output a text describing two restaurants. In the case of the latter restaurant, you'd only want to say how it's different from the first one. For example, `"Aurum is an English coffee shop in city center. It has low prices. Mark's Inn, on the other hand, is in the price range £20-25."` Try and reuse a lot of the generation logic between the first and the second restaurant.

### 4. Neural NLG

**This is a very ambitious project and requires previous background with neural sequence-2-sequence models**.
Train a seq-2-seq model to predict the references from the MRs. A possible workflow:
1. Delexicalize the training data (at least the references, maybe even the MRs)
2. Train a seq2seq encoder-decoder that encodes the MR (as a sequence of characters or words) into an internal MR representation and then decodes the reference from the internal MR representation.
3. Once trained, embed the model in some code that feeds an MR to the trained model (delexicelized if you chose to do so) and then relexicalizes the model's output from the MR to produce the actual system output.
4. Evaluate the system from step 3 as a whole.
