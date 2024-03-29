---
title: 'Chapter 3: Representing text as features: Tokenizers, TextFields, and TextFieldEmbedders'
description:
  "A deep dive into AllenNLP's core abstraction: how exactly we represent textual inputs, both on
the data side and the model side."
prev: /chapter02
next: /chapter04
type: chapter
id: 3
---

<textblock>

This chapter assumes you have a basic familiarity with what a `Field` is in AllenNLP, which you can
get near the beginning of [chapter 2 of this course](/chapter02).

</textblock>


<exercise id="1" title="The basic problem: text to features" type="slides">

<slides source="chapter03/01_the_basic_problem">
</slides>

</exercise>

<exercise id="2" title="The data side: TextFields" type="slides">

<slides source="chapter03/02_data_side">
</slides>

</exercise>

<exercise id="3" title="A simple TextField example">

In this exercise you can get some hands-on experience with how `TextField` data processing works.
We'll tokenize text and convert it into arrays using a `TextField`, printing out what it looks like
as we go.  First just run the code block below as is and see what comes out, then try modifying it
however you like to see what happens.

After you've gotten a feel for what's going on in the given example, as an exercise, see if you can
switch from using a single integer id for each word to representing words by the sequence
of characters that make them up.

Notice as you're looking at the output that the words in the vocabulary all start with index 2.
This is because index 0 is reserved for padding (which you can see once you finish the exercise),
and index 1 is reserved for out of vocabulary (unknown) tokens.  If you change the text to include
tokens that you don't add to your vocabulary, you'll see they get a 1 from the
`SingleIdTokenIndexer`.

<codeblock id="chapter03/data/simple">
You'll want to use a `TokenCharactersIndexer` instead of a `SingleIdTokenIndexer`, and you'll need
to update the vocabulary, also.
</codeblock>

This time, let's modify the same code, but to use characters as tokens.  Both of these cases use
characters as their base input, but if we do it this way, in the model we'll get a single vector
per _character_, instead of per _word_.

<codeblock id="chapter03/data/simple2">
You'll want to modify the `Tokenizer` and the `Vocabulary`.
</codeblock>

</exercise>

<exercise id="4" title="Combining multiple TokenIndexers">

In some cases in NLP we want to use multiple separate methods to represent text as vectors, then
combine them to get a single representation.  For instance, we might want to use pre-trained [GloVe
vectors](https://nlp.stanford.edu/projects/glove/) along with a character convolution to handle
unseen words (this was the approach taken by the [Bidirectional Attention
Flow](https://www.semanticscholar.org/paper/Bidirectional-Attention-Flow-for-Machine-Seo-Kembhavi/007ab5528b3bd310a80d553cccad4b78dc496b02)
model (BiDAF), one of the first successful models on the [Stanford Question Answering
Dataset](https://rajpurkar.github.io/SQuAD-explorer/)).  The `TextField` abstraction is built with
this in mind, allowing you specify _any number_ of `TokenIndexers` that will get their own entries
in the tensors that are produced by the `TextField`.

In the following code, we show the setup used in BiDAF.  Run it to see what the output looks like
(pre-trained GloVe vectors happen in the model, not here, and in a real setting AllenNLP
automatically constructs the vocabulary in a special way that's informed by what words you have
vectors for).  See if you can modify it to add a third kind of representation: part of speech tag
embeddings.  Remember that the "embeddings" happen in the model, not here; we just need to get
_indices_ for the part of speech tags that we see in the text.

<codeblock id="chapter03/data/combined">
The `Tokenizer` needs to be modified to include part of speech tags, you need a third entry in the
`token_indexers` dictionary, and the vocabulary needs to include part of speech tags.
</codeblock>

</exercise>

<exercise id="5" title="Contextualized representations in TextFields">

ELMo and BERT, how they get converted to tensors, and how wordpieces are handled.

</exercise>

<exercise id="6" title="The model side: TextFieldEmbedders" type="slides">

<slides source="chapter03/06_model_side">
</slides>

</exercise>

<exercise id="7" title="Embedding simple TextField inputs">

Cover SingleIdTokenIndexer and TokenCharactersIndexer, point to other simple ones.

</exercise>

<exercise id="8" title="Embedding text that has multiple TokenIndexers">

What happens when you provide more than one TokenIndexer, how to make it work.

</exercise>

<exercise id="9" title="Embedding contextualized inputs">

ELMo and BERT, how they get converted to tensors, and how wordpieces are handled.

</exercise>
