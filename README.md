# Questioning the mathematical foundations of AI

Companion notebooks for my talk at **PyCon España 2026** (Barcelona, 8 November 2026).

Almost every production retrieval system today does the same thing. It turns text into vectors with an embedding model, then compares those vectors with cosine similarity in flat space. It works well enough that most of us never stop to ask what it assumes. These notebooks do stop and ask. You'll see what that flat, cosine-based geometry takes for granted, where it starts to break (hierarchies, one-way relationships, scores that mean different things in different models), and two alternatives that come from mathematics: hyperbolic space and moving frames.

Everything runs on a laptop CPU, and every notebook is saved with its outputs, so you can read it here on GitHub without running anything.

## How to run

You'll need Python 3.11 or newer (the saved outputs were made with Python 3.13).

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Then open the `notebooks/` folder and start with `01`. The first run of notebook 01 downloads two small models from Hugging Face, `all-MiniLM-L6-v2` and the same network before contrastive training (`nreimers/MiniLM-L6-H384-uncased`, used in Step 8), about 90 MB each, so give it a minute.

The outputs saved in the notebooks come from gensim 4.4.0, sentence-transformers 6.2.0, torch 2.15.0, geoopt 0.5.1, hypll 0.1.1 and geomstats 2.8.0. The demo's exact neighbours depend on the gensim version, so if yours come out different, check that first.

## The notebooks

If you'd like a friendlier walk through what happens in each notebook before you open it, have a look at [`notebooks/README.md`](notebooks/README.md).

| Notebook | What you'll see |
|---|---|
| [01 · Embeddings and cosine similarity](notebooks/01_embeddings_and_cosine.ipynb) | How text becomes a vector, how retrieval ranks chunks with cosine, BM25 and hybrid search, and the two places cosine quietly misleads you (anisotropy and symmetry). |
| [02 · Hyperbolic embeddings](notebooks/02_hyperbolic_embeddings.ipynb) | Why trees don't fit in flat space, the Poincaré disk built from scratch, a small hierarchy trained with gensim, and a race between hyperbolic and flat space on a 121-node tree. |
| [03 · Moving frames](notebooks/03_moving_frames.ipynb) | Klein's idea of geometry as "what survives a group of transformations", a moving frame computed in SymPy, and the frame on the Poincaré disk that powers the demo. |
| [04 · Curved spaces in Python](notebooks/04_curved_spaces_in_python.ipynb) | Four Python packages for curved spaces, and how to start with each. |
| [05 · The demo](notebooks/05_demo_moving_frame_search.ipynb) | The talk's demo: a hierarchy in the Poincaré disk, searched with a moving frame. |

## About the author

**Dr Ana Rojo-Echeburúa** is Head of FIN and Fractional CTO at AinTools. She's a mathematician with a PhD from the University of Kent, where she worked on moving frames and Noether's conservation laws, supervised by Elizabeth Mansfield. She's spent about ten years building AI systems, leading engineering teams and founding startups.

## References

- Mikolov et al. (2013). Efficient Estimation of Word Representations in Vector Space. arXiv:1301.3781.
- Pennington, Socher & Manning (2014). GloVe. EMNLP.
- Devlin et al. (2019). BERT. NAACL.
- Peters et al. (2018). Deep Contextualized Word Representations (ELMo). NAACL.
- Reimers & Gurevych (2019). Sentence-BERT. EMNLP.
- Karpukhin et al. (2020). Dense Passage Retrieval for Open-Domain Question Answering. EMNLP.
- Wang et al. (2022). Text Embeddings by Weakly-Supervised Contrastive Pre-training (E5). arXiv:2212.03533.
- Robertson & Zaragoza (2009). The Probabilistic Relevance Framework: BM25 and Beyond. Foundations and Trends in Information Retrieval.
- Cormack, Clarke & Büttcher (2009). Reciprocal Rank Fusion Outperforms Condorcet and Individual Rank Learning Methods. SIGIR.
- Ethayarajh (2019). How Contextual are Contextualized Word Representations? EMNLP.
- Gao, Yao & Chen (2021). SimCSE: Simple Contrastive Learning of Sentence Embeddings. EMNLP.
- Steck, Ekanadham & Kallus (2024). Is Cosine-Similarity of Embeddings Really About Similarity? WWW '24 Companion.
- Weller, Boratko, Naim & Lee (2026). On the Theoretical Limitations of Embedding-Based Retrieval. ICLR.
- Bourgain (1986). The metrical interpretation of superreflexivity in Banach spaces. Israel J. Math.
- Sarkar (2011). Low Distortion Delaunay Embedding of Trees in Hyperbolic Plane. Graph Drawing.
- van Spengler & Mettes (2025). Low-distortion and GPU-compatible Tree Embeddings in Hyperbolic Space. ICML.
- Nickel & Kiela (2017). Poincaré Embeddings for Learning Hierarchical Representations. NeurIPS.
- Nickel & Kiela (2018). Learning Continuous Hierarchies in the Lorentz Model. ICML.
- Sala, De Sa, Gu & Ré (2018). Representation Tradeoffs for Hyperbolic Embeddings. ICML.
- Gu, Sala, Gunel & Ré (2019). Learning Mixed-Curvature Representations in Product Spaces. ICLR.
- Desai et al. (2023). Hyperbolic Image-Text Representations (MERU). ICML.
- He et al. (2025). HELM: Hyperbolic Large Language Models via Mixture-of-Curvature Experts. NeurIPS.
- Madhu et al. (2026). HypRAG: Hyperbolic Dense Retrieval for Retrieval Augmented Generation. ICML.
- Kochurov, Karimov & Kozlukov (2020). Geoopt: Riemannian Optimization in PyTorch. arXiv:2005.02819.
- Řehůřek & Sojka (2010). Software Framework for Topic Modelling with Large Corpora (gensim). LREC Workshop on New Challenges for NLP Frameworks.
- van Spengler, Wirth & Mettes (2023). HypLL: The Hyperbolic Learning Library. ACM Multimedia. arXiv:2306.06154.
- Miolane et al. (2020). Geomstats: A Python Package for Riemannian Geometry in Machine Learning. JMLR 21.
- Klein (1872). Vergleichende Betrachtungen über neuere geometrische Forschungen.
- Noether (1918). Invariante Variationsprobleme.
- Fels & Olver (1998, 1999). Moving Coframes I and II. Acta Appl. Math.
- Mansfield (2010). A Practical Guide to the Invariant Calculus. Cambridge University Press.
- Gonçalves & Mansfield (2012). On Moving Frames and Noether's Conservation Laws. Stud. Appl. Math. 128.
- Mansfield, Rojo-Echeburúa, Hydon & Peng (2019). Moving Frames and Noether's Finite Difference Conservation Laws I. Trans. Math. Appl.
- Mansfield & Rojo-Echeburúa (2019). Moving Frames and Noether's Finite Difference Conservation Laws II. Trans. Math. Appl.
- Kunin et al. (2021). Neural Mechanics: Symmetry and Broken Conservation Laws in Deep Learning Dynamics. ICLR.
- Puny et al. (2022). Frame Averaging for Invariant and Equivariant Network Design. ICLR.
- Kaba et al. (2023). Equivariance with Learned Canonicalization Functions. ICML.
- Sangalli et al. (2022). Moving Frame Net: SE(3)-Equivariant Network for Volumes. NeurReps.

## Licence

MIT, see [LICENSE](LICENSE).
