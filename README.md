# Model Selection for Clustering — Histology Tissue Image Features

Unsupervised clustering pipeline applied to dimensionality-reduced histology tissue image features.
Four clustering algorithms are evaluated across four deep learning feature extractors using both
PCA and UMAP reduced representations, with results assessed via Silhouette Score and V-measure.

## Problem Statement

Given pre-extracted and dimensionality-reduced image features from histology tissue samples,
identify the best combination of clustering algorithm and feature representation for grouping
tissue types without labels.

## Dataset

Pre-computed feature files are provided as HDF5 (.h5) files. Each file contains:

| Key | Description |
|---|---|
| pca_feature | PCA-reduced feature vectors |
| umap_feature | UMAP-reduced feature vectors |
| file_name | File paths containing tissue type labels |

| File | Feature Extractor |
|---|---|
| pge_dim_reduced_feature.h5 | PGE (custom) |
| resnet50_dim_reduced_feature.h5 | ResNet50 |
| inceptionv3_dim_reduced_feature.h5 | InceptionV3 |
| vgg16_dim_reduced_feature.h5 | VGG16 |

Tissue labels are extracted from the file path structure:
path/tissue_type/filename — the third path component gives the tissue class.


## Pipeline Overview

1. Load PCA and UMAP features from each HDF5 file
2. Extract true tissue labels from file path metadata
3. Apply four clustering algorithms to both PCA and UMAP features
4. Evaluate each combination using Silhouette Score and V-measure Score
5. Visualise clusters in 2D and 3D
6. Plot label distribution per cluster as stacked bar charts

## Clustering Algorithms

| Algorithm | Notes |
|---|---|
| K-Means | Partition-based; number of clusters tuned via inertia elbow method |
| Gaussian Mixture Model (GMM) | Probabilistic; number of components tuned via silhouette score |
| Louvain | Graph-based community detection; adjacency matrix built from pairwise distances |
| Agglomerative Clustering | Hierarchical; number of clusters tuned via silhouette score |


## Feature Extractors vs Dimensionality Reduction

Each clustering algorithm is run on:
- PCA features from PGE, ResNet50, InceptionV3, VGG16
- UMAP features from PGE, ResNet50, InceptionV3, VGG16

This gives a comprehensive comparison across 4 algorithms x 4 extractors x 2 reductions = 32 configurations.

## Visualisations

Each clustering configuration produces:
- 2D scatter plot of the first two components coloured by cluster assignment
- 3D scatter plot of the first three components
- Stacked bar chart showing the distribution of true tissue labels within each predicted cluster

## Tech Stack

| Library | Purpose |
|---|---|
| h5py | Loading HDF5 feature files |
| scikit-learn | K-Means, GMM, Agglomerative Clustering, evaluation metrics |
| sknetwork | Louvain community detection |
| scipy | Sparse matrix construction for Louvain |
| numpy / pandas | Data handling and label processing |
| matplotlib | 2D/3D cluster plots and label distribution charts |
