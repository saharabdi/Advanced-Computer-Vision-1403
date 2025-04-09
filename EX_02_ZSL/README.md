# Generative-Based Models (VAE) for Zero-Shot Learning

This project implements a Variational Autoencoder (VAE) based approach for Zero-Shot Learning (ZSL), focusing on recognizing classes that were never seen during training by leveraging semantic information.

## Overview

Zero-Shot Learning (ZSL) addresses the challenge of recognizing classes that were not present in the training data. This implementation uses a generative approach with Variational Autoencoders to bridge the gap between seen and unseen classes by leveraging semantic information (attributes and text descriptions).

## Key Features

- Implementation of Conditional VAE for ZSL
- Integration with BERT for semantic embeddings
- Training and inference phases for both seen and unseen classes
- Visualization of generated features
- Evaluation metrics for ZSL performance

## Technical Implementation

### Architecture
- **Encoder**: Maps visual features and semantic embeddings to latent space
- **Decoder**: Reconstructs visual features conditioned on semantic input
- **BERT Integration**: For generating semantic embeddings
- **Classifier**: For final class prediction

### Training Process
1. Data preparation with seen and unseen class split
2. Feature extraction using pre-trained CNN
3. Semantic embedding generation using BERT
4. VAE training on seen classes
5. Feature generation for unseen classes
6. Classifier training and evaluation

## Dataset

The implementation uses CIFAR-10 dataset with:
- **Seen Classes**: airplane, automobile, bird, cat, deer, dog, frog, horse
- **Unseen Classes**: ship, truck

## Dependencies

- PyTorch
- Transformers (BERT)
- NumPy
- Matplotlib
- Torchvision

## 📚 Resources

- [Presentation Slides](https://gamma.app/docs/Generative-Methods-for-Zero-Shot-Learning-The-Power-of-VAEs-5j1snunpsed7xja)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!