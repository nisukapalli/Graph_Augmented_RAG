# Graph-Augmented RAG Pipeline

## Overview

This project implements a graph-augmented RAG (retrieval-augmented generation) pipeline for question-answering over scientific papers. Traditional RAG systems rely solely on semantic similarity for retrieval, which can overlook influential but less lexically similar papers. This system augments retrieval with a citation graph structure to surface papers that are both semantically relevant and influential.

A citation graph was constructed from a subset of Microsoft's Open Academic Graph, resulting in ~2.5M fully linked and pruned paper entries. PageRank was computed over the graph to capture paper influence, and dense embeddings were generated using the all-MiniLM-L6-v2 sentence transformer model to measure semantic similarity. Candidate papers are ranked using a weighted combination of PageRank and cosine similarity.

At inference time, the top-K ranked papers are passed as context to GPT-4o along with the user's query. In evaluation across 100 test queries spanning 10 disciplines, human reviewers preferred responses from this graph-augmented system over baseline GPT-4o in 84% of cases.

## Architecture

The system consists of three main components:

1. Graph Construction: Parse Open Academic Graph data and build a citation graph (~2.5M nodes).
2. Ranking Layer: Compute PageRank scores and generate sentence embeddings for each paper.
3. Retrieval + Generation: Rank candidates using a weighted combination of influence and similarity, then provide top-K context to GPT-4o.

## Tech Stack

- Python
- NetworkX (citation graph, PageRank)
- Sentence Transformers (all-MiniLM-L6-v2)
- GPT-4o (generation)
