# The notebooks, step by step

This page explains each notebook in plain words, so you know what you're about to see before you open it. You don't need any maths to follow along here. The notebooks themselves have the formulas and the code, but every step starts with the idea in words first.

Run them in order. Each one builds on the one before.

## 01 · Embeddings and cosine similarity

**The question:** when a search system says two pieces of text are "similar", what is it actually computing, and when should you not trust that number?

1. **Turn a sentence into numbers.** You give an embedding model one sentence and it returns a list of 384 numbers. That list is the sentence's position in a 384-dimensional space.
2. **A bit of history.** Older models gave every word one fixed vector. Newer ones give a word a vector that depends on the sentence around it. Retrieval systems use one vector per chunk of text.
3. **Look at it in 2D.** You embed twelve short sentences about late invoices, football and recipes, squash them down to two dimensions and plot them. Sentences on the same topic land near each other.
4. **Measure similarity with cosine.** Cosine similarity compares the direction of two vectors. You compute it by hand, check it against scikit-learn, and see all twelve sentences compared in a heatmap.
5. **Three ways to measure, one answer.** Dot product, cosine and straight-line distance look different, but once every vector has length 1 they rank results in exactly the same order. You check this with real numbers.
6. **Build a tiny search engine.** A handful of chunks, one query, one line of maths, and you get the top three results.
7. **Mix in keyword search.** BM25 matches exact words, which is great for an invoice number. Embeddings match meaning, which is great for a paraphrase. You combine the two rankings with Reciprocal Rank Fusion and see each one win in a different case.
8. **First crack: the scores bunch up.** Even completely unrelated sentences get a cosine well above zero, and where that band sits depends on the model. So "anything above 0.8 is relevant" means something different for every model.
9. **Second crack: cosine goes both ways.** "A dog is an animal" is true and "an animal is a dog" is not, yet cosine gives both directions the same score. You also see a small trick that leaves everything training cares about unchanged but moves the cosine scores around.
10. **Takeaway.** Cosine in flat space is a choice with consequences, and notebook 02 tries a different space.

## 02 · Hyperbolic embeddings

*Coming next.*

## 03 · Moving frames

*Coming after that.*
