# A walk through the notebooks

This page tells you what happens in each notebook before you open it, so you know where the story is going. You don't need any maths to follow it. The notebooks have the formulas and the code, and every step there still starts with the idea first.

They're meant to be read in order, because each one picks up where the last one stopped.

## 01 · Embeddings and cosine similarity 🧭

Every retrieval system you've used lately does the same trick. It turns text into a long list of numbers and then looks for the lists that point in the most similar direction to your question. This notebook takes that trick apart.

You start by giving an embedding model one sentence and getting 384 numbers back, then you squash twelve sentences about late invoices, football and recipes down to two dimensions and watch them fall into three groups on their own. From there you compute cosine similarity by hand, check it against scikit-learn, and see that dot product, cosine and straight-line distance all give exactly the same ranking once your vectors have length 1. With that in place you build a tiny search engine in a few lines, add keyword search with BM25, and fuse the two with Reciprocal Rank Fusion. My favourite moment is the invoice question, where the embedding search puts the wrong invoice at the top and keyword search saves the day.

Then come the cracks. Unrelated sentences score 0.06 on average with a modern embedding model and 0.85 with the same network before contrastive training, so a fixed threshold like 0.8 means something completely different from one model to the next. And cosine can't tell "a dog is an animal" from "an animal is a dog" (they score 0.99), while a small rescaling trick leaves training untouched and still moves the cosine scores around. That's the cliffhanger for notebook 02.

## 02 · Hyperbolic embeddings 🌳

If flat space can't hold a hierarchy, maybe a different space can. This notebook goes looking for one.

It opens with one plot that tells the whole story. A tree doubles its nodes at every level, flat space only grows like a polynomial, and hyperbolic space grows exponentially, just like the tree. You compare circles on a sphere, a plane and a hyperbolic plane, then build the Poincaré disk from scratch in numpy and draw a whole tree inside it, where the edge is infinitely far away and there's always room for more branches.

Next you train the little animal and document hierarchy from the talk with gensim, and general words like "animal" land near the centre while specific ones like "dog" end up near the edge. Then the race: the same 121-node tree, the same loss, the same optimiser and the same seed, once in flat space and once in hyperbolic space. Hyperbolic wins with a MAP of 0.954 against 0.599, and the published WordNet results tell the same story at scale. The notebook finishes with what the research says about hyperbolic retrieval and LLMs, and with an honest look at the costs, including the moment float32 simply runs out of digits near the edge.

## 03 · Moving frames 🔄

This is the notebook closest to my own research, and it asks the question underneath the first two: what does it actually mean to choose a geometry?

It starts with Felix Klein in 1872, who said a geometry is defined by the transformations you've decided don't matter and the things they leave unchanged. That gives the line I most want you to take home, that your similarity metric is a statement about what you decided doesn't matter. Moving frames turn Klein's idea into a recipe. Move the object into a standard position, then read off whatever is left.

You run that recipe in SymPy on a curve in the plane, and the curvature formula appears without anyone typing it in. Then Emmy Noether shows up, with symmetries giving conservation laws, and SymPy checks that rotational symmetry really does conserve angular momentum. After that you build the moving frame on the Poincaré disk that powers the talk's demo, and the notebook closes by bringing it all back to machine learning, including a conservation law hiding inside ordinary neural network training that you can watch happen live.

## 04 · Curved spaces in Python 🐍

The good news after all that theory is that the maths is already packaged, and this notebook is a hands-on tour of the four libraries on the slide.

With geoopt you reproduce the two numbers from the talk (the same ruler step of 0.09 is 0.18 near the centre and 2.35 near the edge), move between the Poincaré ball and the Lorentz model, and see what happens when you train with an optimiser that doesn't follow the curve. Spoiler, the points fly straight out of the disk. You glue hyperbolic, spherical and flat pieces into one product manifold, go from a list of pairs to a hyperbolic embedding in five lines of gensim, push data through a hyperbolic layer with HypLL, and measure circles on three kinds of space with geomstats.

Along the way you'll meet a few gotchas I found while checking each library, like HypLL's curvature quietly coming out as 1.313 and geoopt hiding precision errors near the edge. The last section asks what to do when your vector database has no hyperbolic index, and ends with a change of coordinates that turns exact hyperbolic search into an ordinary dot-product search.

## 05 · The demo ✨

This is the demo from the talk, start to finish, and it puts everything together.

Twelve words go into the Poincaré disk. For any query word, a moving frame slides it to the centre and rotates the other word onto a straight axis, and the one number left over turns out to be exactly the hyperbolic distance, matching the textbook formula and gensim to about 14 decimal places. Sort by that number and you get a search that follows the hierarchy, so for "dog" you get whale and cat first and then mammal.

Then the fun part. You move the whole embedding with 100 random symmetries of the disk, the picture changes completely every time, and the search results never change once. Try the same thing with cosine similarity and its answers change every single time. Choose the symmetry, put everything in standard position, then measure only what survives.
