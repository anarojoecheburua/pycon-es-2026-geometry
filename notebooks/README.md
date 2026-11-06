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

**The question:** if flat space can't hold a hierarchy, is there a space that can?

1. **Why flat space struggles with trees.** A tree doubles its nodes at every level, and flat space only grows like a polynomial, so the deep levels run out of room. You see it on one plot.
2. **Three curvatures.** You measure how long a circle is on a sphere, on a flat plane and in hyperbolic space. In hyperbolic space circles grow faster and faster, and that extra room is what trees need.
3. **The Poincaré disk from scratch.** Hyperbolic space squeezed into a disc, where the edge is infinitely far away. You write the distance formula in a few lines of numpy, see that a small step near the edge is a huge distance, and draw a whole tree in the disk.
4. **Train a hierarchy in 2D.** You give gensim a list of (child, parent) pairs like ("dog", "mammal") and it places them in the disk. General words land near the centre, specific ones near the edge, and each branch gets its own direction.
5. **Flat vs hyperbolic, same data.** You train a 121-node tree in both spaces with exactly the same settings and score how well each one keeps the hierarchy. Hyperbolic wins by a lot, and the published WordNet results say the same thing at scale.
6. **geoopt.** A quick tour of the PyTorch library that handles the curved-space maths, checked against our hand-written formula.
7. **Results from retrieval and LLMs.** What happened when researchers put hyperbolic geometry into RAG, image–text models and LLMs.
8. **The costs.** Running out of decimal places near the edge, trickier training, and no support in today's vector databases.

## 03 · Moving frames

**The question:** what does it actually mean to choose a geometry, and how do you find the number worth comparing?

1. **Klein's idea.** In 1872 Felix Klein said a geometry is whatever stays the same when you apply a group of transformations. You see what that means for Euclidean distance, cosine and hyperbolic distance, and why your similarity metric is really a statement about what you decided doesn't matter.
2. **What a moving frame is.** Put every object into a standard position, then compare what's left. You get the formal definition and a four-step recipe: pick the group, choose normalisations, solve for the frame, read off the invariants.
3. **The curvature of a curve, in SymPy.** You run the recipe on a curve in the plane, with rotations and translations as the symmetries. Move the point to the origin, rotate the tangent flat, and the curvature formula falls out without anyone typing it in. Then you check it on a circle.
4. **Noether in one cell.** Every symmetry gives a conservation law. SymPy checks that rotating symmetry gives conserved angular momentum, and you see how moving frames write these laws in a readable form (including the discrete version from my own research).
5. **A moving frame for the Poincaré disk.** The frame behind the talk's demo. Slide the query point to the centre, rotate the other point onto an axis, and the only number left is the hyperbolic distance. You see it in a three-panel picture and check it under 1,000 random symmetries.
6. **The demo.** A pointer to notebook 05, where the full search demo lives.
7. **Why this matters for embeddings.** Arbitrary cosine scores as a symmetry problem, equivariant ML that already uses frames, and a conservation law hiding inside ordinary neural network training, which you watch happen live.
8. **Closing thought.** Decide which symmetries matter, build the geometry around them, then measure only what survives.
