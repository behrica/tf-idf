TF-IDF
======
This is a small and reasonably performant implementation of TF-IDF written in Clojure.

Usage
-----
There is only a single namespace, `dk.cst.tf-idf`. This namespace contains the core TF-IDF functions as well as a few extra utility functions, e.g. functions for picking terms from the TF-IDF results.

The core TF-IDF functions take a sequence of `documents` (usually strings) and return regular Clojure collections. In order to avoid recalculating too much, results of any intermediate calculations can usually also be fed into the next step of the algorithm.

### Alternative tokenizers
The `*tokenizer-xf*` dynamic var contains a reference to the default transducer used to tokenize input documents. This dynamic var can be rebound to allow for alternative tokenizer implementations. The simplest way to create a new tokenizer transducer is to use the included `->tokenizer-xf` function.

Explanation of terms
--------------------
This is a very brief explanation of the different terms used in TF-IDF.

### Vocab
* The list of all words considered in the corpus.

### Term frequency
```
tf(d,t) = count(t in d) / count(x in d)
```

* How many times does the word/lemma appear in the document?
* Each frequency score is normalised by dividing with the total number of words in the text (`count(x in d)`).
* Only the frequency of _vocab_ is considered!

### Document frequency (absolute)
```
df(d,t) = count(d containing t)
```
* How many documents does word/lemma appear in?
* Not normalised _by default_ in this implementation, although you can always run `(normalize-frequencies df-result)` to achieve this.

### Inverse document frequency
```
idf(d,t) = log(count(d) / (count(d containing t) + 1))
```
* The `document frequency`, except this number has been normalised by dividing with `count(d containing t)`.
* This has the opposite effect of `df(d,t)` as rarer words will have a _higher_ inverse document frequency than common words.
* To avoid dividing by zero, 1 is added to `count(d containing t)`.
* To keep very rare terms from having gigantic scores, the final value returned is actually the _logarithm_ applied to this expression.

### TF-IDF
```
tfidf(d,t) = tf(d,t) * idf(d,t)
```

* The product of the `term frequency` and the `inverse document frequency`.

Links
-----
* https://towardsdatascience.com/tf-idf-for-document-ranking-from-scratch-in-python-on-real-world-dataset-796d339a4089
* https://www.freecodecamp.org/news/how-to-process-textual-data-using-tf-idf-in-python-cd2bbc0a94a3/
* https://stackoverflow.com/questions/42269313/interpreting-the-sum-of-tf-idf-scores-of-words-across-documents