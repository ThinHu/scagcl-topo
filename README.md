# scAGCL with Topology Signature Loss

This project implements an Adversarial Graph Contrastive Learning (scAGCL) framework for clustering single-cell RNA sequencing (scRNA-seq) data. The model is uniquely enhanced by combining adversarial graph attacks with an Optimal Topology Signature Loss derived from Persistent Homology.

## Key Features

* Data Preprocessing: Uses Scanpy for robust normalization and selection of highly variable genes.
* Graph Construction: Generates a k-Nearest Neighbors (kNN) graph based on Pearson correlation distances between cells.
* Adversarial Contrastive Learning: Employs a Graph Convolutional Network (GCN) encoder, trained using a contrastive loss function to pull similar cell representations together.
* Graph Adversarial Attacks: Applies Projected Gradient Descent (PGD) to introduce perturbations via Gene Dropping (node features) and Edge Dropping (graph structure), ensuring highly robust latent embeddings.
* Topology Signature Loss: Utilizes the `aleph` library to compute Vietoris-Rips complexes. This custom loss function forces the latent representations to preserve the higher-order topological features (like H0 and H1 components) of the original data manifold.

## Dependencies

* Python 3.x
* PyTorch & PyTorch Geometric
* Scanpy
* Scikit-learn
* NetworkX
* Aleph (for Topological Data Analysis)
* NumPy, SciPy, h5py

## Workflow

1. Data Loading: Imports scRNA-seq datasets in `.h5` format.
2. Setup: Preprocesses the counts and builds the adjacency matrix for the cell graph.
3. Training:
   * Base Training: Minimizes contrastive loss between augmented views of the graph.
   * Adversarial Training: Generates adversarial examples dynamically during training to challenge the encoder.
   * Topological Alignment: Applies the `OptimalTopologySignatureLoss` to penalize deviations from the original topological structure.
4. Evaluation: Embeddings are evaluated using K-Means clustering, measuring performance via Adjusted Rand Index (ARI) and Normalized Mutual Information (NMI).

## Results

Integrating the topology signature loss yields measurable improvements in clustering performance. On the tested dataset (`QS_Diaphragm`):
* Without Topology Loss: ARI = 0.985, NMI = 0.971
* With Topology Loss: ARI = 0.989, NMI = 0.977
