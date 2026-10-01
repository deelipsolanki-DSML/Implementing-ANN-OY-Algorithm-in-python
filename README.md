# Approximate Nearest Neighbor Search from Scratch

## 📌 Introduction

Finding similar data points becomes increasingly expensive as the size of a dataset grows. In a traditional **Exact Nearest Neighbor (NN)** approach, a query point is compared against every point in the dataset, resulting in a linear search over the entire dataset.

This project explores how **Approximate Nearest Neighbor (ANN)** search can significantly reduce the amount of computation required while still retrieving highly similar results.

In this notebook, I implement a simple version of **Annoy-style tree-based Approximate Nearest Neighbor algorithm from scratch** and compare it with a traditional exact nearest-neighbor search using the **MNIST dataset**.

---

## 🔍 Project Overview

The project uses the **MNIST dataset containing 70,000 handwritten digit images**, where each image is represented as a 784-dimensional vector (`28 × 28` pixels).

The objective is simple:

> Given a query image, find the most similar images based on **Euclidean distance**.

### 1. Exact Nearest Neighbor Search

The query vector is compared against **60,000 data points**.

The process involves:

* Calculating the Euclidean distance between the query and every data point.
* Sorting all distances.
* Selecting the top `K` nearest points.

This requires scanning the entire dataset, making the search approximately **O(n)**.

### 2. Approximate Nearest Neighbor Search

An **Annoy-style binary tree** is constructed to divide the dataset into smaller groups.

The core idea behind approximate nearest-neighbor (ANN) search: don't compare the query against everything; use a structure to narrow down the candidates first.

tree construction:

* Randomly selects two points as splitting points.
* Assigns every data point to the closer splitting point.
* Recursively divides the resulting groups.
* Stops splitting when a leaf contains at most `1,000` samples.

During a query:

1. The tree is traversed instead of scanning the entire dataset.
2. Only the points in the selected leaf are considered.
3. Exact Euclidean distances are calculated for those candidates.
4. The nearest `K` points are returned.

This dramatically reduces the number of distance calculations required during search.

---

## 📊 Key Achievement

The notebook demonstrates how a tree-based ANN approach can reduce the search space from the **entire dataset of 60,000 points** to only a small subset of candidate points.

For the experiment:

* Dataset size used for indexing: **60,000 images**
* Leaf size: **1,000 samples**
* Query: A single MNIST image

```text
log₂(60,000 / 1,000) ≈ 5.9
```

The experimentally observed median tree depth is very close to this theoretical value, demonstrating that the tree construction behaves approximately as expected.

The notebook also visualizes the retrieved nearest neighbors so that the results from **Exact NN** and **Approximate NN** can be compared visually.

### ⚡ Search Complexity

For the exact approach, the search involves scanning the complete dataset:

```text
O(n)
```

For the tree-based approach, the search is approximately:

```text
O(log₂(n / leaf_size) + leaf_size + leaf_size log(leaf_size))
```

This consists of:

* Tree traversal
* Distance calculation for the candidate points
* Sorting the candidate distances

The important idea is that **the expensive distance calculations are performed on a much smaller candidate set instead of the entire dataset**.

---

## 🛠️ Tech Stack

### Programming Language

* 🐍 Python

### Libraries

* **NumPy** — Numerical computations and array manipulation
* **Pandas** — Data handling and distance-result processing
* **Scikit-learn** — MNIST dataset loading and Euclidean distance calculations
* **Matplotlib** — Visualization of MNIST images and tree-depth distribution
* **tqdm** — Progress tracking while generating multiple trees

### Concepts

* Exact Nearest Neighbor Search
* Approximate Nearest Neighbor Search
* Euclidean Distance
* Binary Trees
* Search Space Reduction
* Time Complexity
* MNIST Dataset
* Algorithmic Optimization

---

## 📁 Project Structure

```text
├── notebook.ipynb
└── README.md
```

The complete implementation, experiments, visualizations, and analysis are available in the Jupyter Notebook.

---

## 🎯 What This Project Demonstrates

This project provides an intuitive understanding of **why Approximate Nearest Neighbor algorithms are useful for large-scale similarity search**.

Rather than treating ANN as a black-box library, the core tree-building and searching logic is implemented from scratch to understand what happens internally during:

```text
Dataset
   ↓
Tree Construction
   ↓
Query
   ↓
Tree Traversal
   ↓
Candidate Selection
   ↓
Distance Calculation
   ↓
Top-K Similar Results
```

This approach provides a practical foundation for understanding more advanced similarity-search systems like HNSW, used in areas such as:

* 🔎 Semantic Search
* 🤖 Recommendation Systems
* 🧠 Vector Databases
* 🖼️ Image Similarity Search
* 🔤 Embedding Search
* 📚 Information Retrieval

---

## Thank you

Thank you for taking the time to explore this project!

If you found this implementation useful or interesting, feel free to ⭐ **star the repository** and check out the notebook for the complete implementation and experiments.
