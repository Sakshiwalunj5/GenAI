# Autoencoder and Variational Autoencoder for Image Reconstruction

**Practical No. 03 – Autoencoder and Variational Autoencoder for Image Reconstruction**

Implementation and comparison of a Convolutional Autoencoder and a Variational Autoencoder (VAE) for reconstructing MNIST handwritten digit images and generating new samples using latent-space sampling.

**Course:** Generative AI Lab | **Student:** Sakshi Uttam Walunj | **Class:** T.Y. B.Tech | **Department:** CSE (AI & ML)

---

## 1. Objective

Build an Autoencoder and a Variational Autoencoder for image reconstruction and generation, then compare their performance using reconstruction quality, training behaviour, and Mean Squared Error (MSE).

**Pipeline:** Dataset loading → preprocessing → Autoencoder construction → VAE construction → model training → image reconstruction → VAE image generation → performance comparison.

## 2. Dataset Description

The experiment uses the **MNIST Handwritten Digit Dataset**, consisting of grayscale images of handwritten digits from 0 to 9.

- Training images: 60,000
- Test images: 10,000
- Image dimensions: 28 × 28 pixels
- Number of classes: 10
- Input normalization: Pixel values scaled to the range [0, 1]

The dataset is loaded using `tensorflow.keras.datasets.mnist`.

## 3. Technologies Used

- **Language:** Python
- **Deep Learning:** TensorFlow, Keras
- **Numerical Computing:** NumPy
- **Visualization:** Matplotlib
- **Environment:** Jupyter Notebook / Google Colab

## 4. Convolutional Autoencoder

The Autoencoder compresses input images into a compact latent representation using an encoder and reconstructs them using a decoder.

- Latent dimension: 32
- Optimizer: Adam
- Loss function: Mean Squared Error (MSE)
- Training epochs: 10

The model is evaluated using reconstruction loss and comparisons between original and reconstructed images.

## 5. Variational Autoencoder (VAE)

The VAE learns a probabilistic latent representation using the latent mean and log variance. The reparameterization trick enables latent-vector sampling, and the decoder reconstructs images from these vectors.

- Latent dimension: 16
- Optimizer: Adam
- Reconstruction loss: Binary cross-entropy
- Regularization: KL-divergence
- Training epochs: 10

The trained decoder is also used to generate new digit-like images by sampling from the latent space.

## 6. Model Training and Evaluation

Both models are trained and evaluated using the MNIST dataset.

The evaluation includes:

- Training and validation loss curves.
- Original versus reconstructed image comparisons.
- Test-set reconstruction MSE.
- A bar chart comparing reconstruction errors.
- VAE-generated image samples.
- Analysis of the differences between deterministic and probabilistic latent representations.

## 7. Results

The recorded test-set reconstruction results are:

| Metric | Autoencoder | VAE |
|---|---:|---:|
| Mean Squared Error (MSE) | 0.003710 | 0.009876 |

The Autoencoder achieved lower pixel-wise reconstruction error in this evaluation. The VAE additionally supports probabilistic latent-space learning and image generation.

The VAE's training objective includes reconstruction loss and KL-divergence. Its total loss is therefore not directly comparable to the Autoencoder's MSE.

## 8. Visual Results

The `screenshots/` directory contains the outputs produced during the experiment, including:

- MNIST dataset samples.
- Autoencoder and VAE architecture summaries.
- Training and validation loss graphs.
- Original and reconstructed image grids.
- VAE-generated digit samples.
- Reconstruction MSE output and comparison chart.

## 9. How to Run

### Install Dependencies

```bash
pip install tensorflow numpy matplotlib jupyter
```

### Execute the Notebook

1. Clone or download this repository.
2. Open `Practical_No_03_Autoencoder_VAE_Image_Reconstruction(1).ipynb` in Jupyter Notebook or Google Colab.
3. Run the notebook cells in order.
4. Allow the MNIST dataset to download when requested.
5. Review the training curves, reconstructed images, generated samples, and evaluation metrics.

## 10. Project Structure

```text
Practical-3-Autoencoder-and-VAE/
├── Practical_No_03_Autoencoder_VAE_Image_Reconstruction(1).ipynb
├── readme.md
└── screenshots/
    ├── 01_mnist_dataset.png
    ├── 02_autoencoder_architecture.PNG
    ├── 03_autoencoder_training.PNG
    ├── 04_autoencoder_reconstruction.PNG
    ├── 05_vae_architecture.PNG
    ├── 06_vae_training.PNG
    ├── 07_vae_reconstruction.PNG
    ├── 08_vae_generated_images.PNG
    ├── 09a_reconstruction_comparison.PNG
    ├── 09b_mse_results.PNG
    └── 09c_mse_comparison_chart.PNG
```

## 11. Key Learning Outcomes

- Image preprocessing and normalization.
- Convolutional encoder-decoder architectures.
- Latent-space representation learning.
- Variational inference and the reparameterization trick.
- Reconstruction loss and KL-divergence.
- Model evaluation using MSE.
- Visual analysis of reconstructed images.
- Image generation using a VAE latent space.

## 12. Conclusion

This practical demonstrates the implementation and evaluation of an Autoencoder and a Variational Autoencoder for image reconstruction using MNIST. The Autoencoder achieved lower reconstruction MSE in the recorded experiment, while the VAE provided additional generative capabilities through probabilistic latent-space sampling.

The experiment highlights the difference between deterministic reconstruction and probabilistic representation learning for image generation.

---

## 13. Author

**Sakshi Uttam Walunj**  
T.Y. B.Tech – CSE (AI & ML)  
Generative AI Lab

**GitHub:** [Sakshiwalunj5](https://github.com/Sakshiwalunj5/GenAI/tree/main/Practical-3-Autoencoder-and-VAE)
