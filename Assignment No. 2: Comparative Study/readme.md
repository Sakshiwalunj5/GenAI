# Generative AI Lab – Assignment 02
## Comparative Analysis of Image Reconstruction and Image Generation Models

**Practical Study of Autoencoder, Variational Autoencoder, DCGAN and Stable Diffusion**

---

## 1. Student Information

| Field | Details |
|---|---|
| Student Name | Sakshi Uttam Walunj |
| PRN | 202502110007 |
| Class | T.Y. B.Tech |
| Batch | A3 |
| Department | Computer Science and Engineering (AI & ML) |
| Course | Generative AI Lab |
| Institute | MIT Academy of Engineering, Alandi, Pune |
| Group Members | Shreya Dattaram Bhosale, Dhanashri Shah |
| Submission Date | 31-10-2026 |

## 2. Project Overview

This assignment explores four generative AI models and examines how they reconstruct existing images or create new ones.

The practical work is divided into two parts:

- **CO3 – Image Reconstruction:** Convolutional Autoencoder (AE) and Variational Autoencoder (VAE).
- **CO4 – Image Generation:** Deep Convolutional Generative Adversarial Network (DCGAN) and pretrained Stable Diffusion.

The experiments use Fashion-MNIST for the reconstruction models and DCGAN. Stable Diffusion is used separately to generate a fashion-product image from a text description.

## 3. Objectives

- Understand the architecture and operation of four generative AI models.
- Prepare and visualize the Fashion-MNIST dataset.
- Reconstruct clothing images using AE and VAE.
- Examine the effect of latent representations on reconstruction quality.
- Generate new clothing-like images using DCGAN.
- Perform text-to-image generation using pretrained Stable Diffusion.
- Evaluate AE and VAE using MSE and MAE.
- Compare model outputs, advantages, limitations and applications.
- Document practical observations, AI usage and group contributions.

## 4. Dataset Description

### Fashion-MNIST

Fashion-MNIST is a grayscale image dataset containing ten categories of clothing and accessories. It is used for the AE, VAE and DCGAN experiments.

| Dataset property | Description |
|---|---|
| Dataset | Fashion-MNIST |
| Training images | 60,000 |
| Testing images | 10,000 |
| Image dimensions | 28 × 28 pixels |
| Channels | 1 (grayscale) |
| Categories | 10 |
| Normalized pixel range | 0 to 1 |

The categories include T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag and Ankle boot.

### Dataset Visualization

The following figure shows sample images from the dataset.

![Fashion-MNIST dataset samples](screenshots/01_fashion_mnist_samples.png)

## 5. Part A – Image Reconstruction (CO3)

### 5.1 Convolutional Autoencoder

A convolutional Autoencoder learns a compressed representation of an input image and uses that representation to reconstruct the original image.

**Architecture:**
- Encoder: convolutional layers extract image features and reduce spatial dimensions.
- Latent representation: stores the compressed image information.
- Decoder: transposed convolutional layers reconstruct the image.

**Implementation configuration:**

| Parameter | Configuration |
|---|---|
| Input shape | 28 × 28 × 1 |
| Latent dimension | 128 |
| Optimizer | Adam |
| Learning rate | 0.001 |
| Loss function | Mean Squared Error |
| Epochs | As executed in the notebook |
| Batch size | As executed in the notebook |

**Training performance**

![Autoencoder training and validation loss](screenshots/02_autoencoder_loss.png)

**Reconstruction output**

![Autoencoder reconstruction results](screenshots/03_autoencoder_reconstruction.png)

### 5.2 Variational Autoencoder (VAE)

A Variational Autoencoder learns a probabilistic latent representation instead of mapping each input to a single fixed latent vector.

The encoder predicts the mean and log-variance of the latent distribution. The reparameterization trick is used to sample a latent vector, which is passed to the decoder to reconstruct the image.

**Implementation configuration:**

| Parameter | Configuration |
|---|---|
| Input shape | 28 × 28 × 1 |
| Latent dimension | 64 |
| Beta | 0.25 |
| Optimizer | Adam |
| Loss function | Reconstruction loss + weighted KL divergence |
| Epochs | As executed in the notebook |
| Batch size | As executed in the notebook |

**Training performance**

![VAE training curves](screenshots/04_vae_loss.png)

**Reconstruction output**

![VAE reconstruction results](screenshots/05_vae_reconstruction.png)

### 5.3 Reconstruction Evaluation

The Autoencoder and VAE are evaluated using the same test images. Mean Squared Error and Mean Absolute Error are used to measure pixel-wise reconstruction differences.

| Model | Test MSE | Test MAE |
|---|---:|---:|
| Convolutional Autoencoder | Enter your executed value | Enter your executed value |
| Variational Autoencoder | Enter your executed value | Enter your executed value |

Lower MSE and MAE indicate smaller pixel-wise reconstruction errors. The visual outputs are also examined to identify blurring, distortion and loss of image details.

## 6. Part B – Image Generation (CO4)

### 6.1 Deep Convolutional Generative Adversarial Network (DCGAN)

DCGAN uses two neural networks that compete during training.

- **Generator:** receives random noise and produces synthetic images.
- **Discriminator:** evaluates whether an image is real or generated.

The generator gradually learns to create images that resemble the Fashion-MNIST training samples.

**Implementation configuration:**

| Parameter | Configuration |
|---|---|
| Input | Random noise vector |
| Noise dimension | 100 |
| Generated image shape | 28 × 28 × 1 |
| Generator output activation | tanh |
| Optimizer | Adam |
| Learning rate | 0.0002 |
| Beta 1 | 0.5 |
| Epochs | As executed in the notebook |

**Generated image samples**

![DCGAN generated images](screenshots/06_dcgan_generated_grid.png)

**Generator and discriminator performance**

![DCGAN generator and discriminator losses](screenshots/07_dcgan_losses.png)

The generated images are examined for clothing-like shapes, visual consistency, diversity and artifacts. Since DCGAN learns through adversarial training, its output quality depends on training stability and model configuration.

### 6.2 Pretrained Stable Diffusion

Stable Diffusion is a pretrained latent diffusion model used for text-conditioned image generation.

Its major components include:

- Text encoder: converts the text prompt into an encoded representation.
- U-Net: performs iterative denoising in latent space.
- Scheduler: controls the denoising process.
- VAE: converts the final latent representation into an image.

Unlike the previous experiments, Stable Diffusion is used as a pretrained model and is not trained on Fashion-MNIST.

**Generation prompt:**

> A high-quality studio product photograph of a single classic white sneaker, side view, isolated on a clean light grey background, soft studio lighting, sharp details and realistic material texture.

**Negative prompt:**

> Blurry, distorted, low resolution, multiple shoes, cropped object, watermark, text and logo.

**Generation configuration:**

| Parameter | Configuration |
|---|---|
| Model | Stable Diffusion v1.5 |
| Inference steps | As executed in the notebook |
| Guidance scale | 7.0 |
| Output resolution | 512 × 512 |
| Attention slicing | Enabled |
| CPU offloading | Enabled where supported |

**Generated image**

![Stable Diffusion generated sneaker](screenshots/08_stable_diffusion_output.png)

The generated image is evaluated based on prompt adherence, object appearance, visual detail and image quality.

## 7. Comparative Analysis

| Comparison factor | Autoencoder | VAE | DCGAN | Stable Diffusion |
|---|---|---|---|---|
| Primary purpose | Image reconstruction | Reconstruction and latent-space generation | Image generation | Text-to-image generation |
| Input | Existing image | Existing image | Random noise | Text prompt and random noise |
| Main components | Encoder and decoder | Encoder, latent distribution, sampling and decoder | Generator and discriminator | Text encoder, U-Net, scheduler and VAE |
| Output | Reconstructed image | Reconstructed or sampled image | New synthetic image | New prompt-conditioned image |
| Main advantage | Simple reconstruction architecture | Probabilistic latent representation | Learns to generate new samples | Detailed prompt-guided image generation |
| Main limitation | May lose fine image details | Reconstructions may appear smooth | Training can be unstable | High computational requirements |
| Applications | Compression and anomaly detection | Representation learning and generation | Synthetic image generation | Creative design and text-to-image applications |

### 7.1 Results and Discussion

The AE and VAE experiments demonstrate two approaches to learning image representations and reconstructing input images. Their reconstruction quality is compared using test-set MSE, MAE and visual inspection.

DCGAN produces new grayscale clothing-like images from random noise. Its outputs are different from reconstructed test images because it does not receive a specific image to reproduce.

Stable Diffusion generates a separate high-resolution sneaker image using a natural-language prompt. It provides text-based control over image generation, unlike the unconditional DCGAN experiment.

The four models therefore address different tasks and should be evaluated using metrics appropriate to their outputs.

## 8. Technologies and Tools Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| TensorFlow / Keras | Autoencoder, VAE and DCGAN implementation |
| NumPy | Numerical operations |
| Pandas | Evaluation and comparison tables |
| Matplotlib | Image visualization and loss graphs |
| PyTorch | Stable Diffusion execution |
| Hugging Face Diffusers | Pretrained diffusion pipeline |
| Transformers | Model dependencies |
| Accelerate | Model execution and memory optimization |
| Google Colab | Cloud-based notebook execution |

## 9. Project Structure

The project is organized as follows:

```text
Assignment_02_Generative_AI/
│
├── Sakshi_Walunj_GenerativeAILabAssignment.ipynb
│
├── README.md
│
└── screenshots/
    ├── 01_fashion_mnist_samples.png
    ├── 02_autoencoder_loss.png
    ├── 03_autoencoder_reconstruction.png
    ├── 04_vae_loss.png
    ├── 05_vae_reconstruction.png
    ├── 06_dcgan_generated_grid.png
    ├── 07_dcgan_losses.png
    └── 08_stable_diffusion_output.png
```

The `screenshots` directory contains the practical evidence captured from the executed notebook. The README uses relative image paths, so the folder must remain in the same project directory when uploading to GitHub.

## 10. Execution Procedure

1. Open the notebook using Google Colab.
2. Select a GPU runtime where available.
3. Execute the import and dataset preparation cells.
4. Train and evaluate the Autoencoder.
5. Train and evaluate the Variational Autoencoder.
6. Execute the DCGAN training and image-generation cells.
7. Load the pretrained Stable Diffusion pipeline.
8. Generate the image using the specified prompt.
9. Review the reconstruction metrics and model comparison.
10. Save the executed notebook and export the practical screenshots.

Stable Diffusion requires downloading its pretrained model weights and dependencies. A GPU is recommended for faster inference.

## 11. AI Usage and Verification

**AI tool used:** ChatGPT.

AI assistance was used to understand model architectures, organize implementation steps, resolve coding issues and interpret the differences between reconstruction and generation models.

**Important prompts used:**

1. Explain the differences between Autoencoder and Variational Autoencoder architectures.
2. Help implement image reconstruction using Fashion-MNIST.
3. Explain DCGAN generator and discriminator architecture and training.
4. Demonstrate image generation using a pretrained Stable Diffusion model.
5. Compare the reconstruction and generation results of the four models.

**Verification process:**

- Checked the dataset dimensions and normalization.
- Examined the original and reconstructed images.
- Compared AE and VAE using test-set MSE and MAE.
- Inspected DCGAN generated images and loss curves.
- Checked the Stable Diffusion output against the text prompt.
- Reviewed model concepts using TensorFlow and Hugging Face documentation.

## 12. Group Contributions

The assignment was completed collaboratively by three group members.

- **Sakshi Uttam Walunj:** [Add your actual contribution in 2–3 lines.]
- **Shreya Dattaram Bhosale:** [Add her actual contribution in 2–3 lines.]
- **Dhanashri Shah:** [Add her actual contribution in 2–3 lines.]

## 13. Key Learning Outcomes

- Understood convolutional image reconstruction using Autoencoders.
- Studied probabilistic latent representations in VAEs.
- Learned the purpose of the reparameterization trick and KL divergence.
- Understood adversarial learning using DCGAN.
- Explored pretrained diffusion models for prompt-conditioned generation.
- Interpreted model loss curves and reconstruction metrics.
- Identified differences between reconstruction and image generation.

## 14. Limitations and Future Scope

**Limitations**

- Fashion-MNIST contains small grayscale images and does not represent the complexity of real-world images.
- AE and VAE reconstruction quality depends on training configuration and latent dimension.
- DCGAN may generate blurred or incomplete samples when adversarial training is unstable.
- Stable Diffusion requires substantial computational resources and pretrained model downloads.
- Reconstruction metrics cannot directly rank models that generate unrelated new images.

**Future improvements**

- Experiment with different latent dimensions and VAE beta values.
- Study the effect of additional DCGAN training and hyperparameter tuning.
- Explore conditional GANs for category-specific image generation.
- Fine-tune diffusion models on specialized image datasets.
- Evaluate generated images using additional image-quality and diversity metrics.

## 15. Conclusion

The practical study examined four generative AI models across image reconstruction and image generation tasks.

The Autoencoder and VAE were used to reconstruct Fashion-MNIST images, with their performance evaluated using MSE, MAE and visual comparison. DCGAN generated new clothing-like images from random noise, while pretrained Stable Diffusion produced a fashion-product image from a text prompt.

The experiments provided an understanding of deterministic and probabilistic latent representations, adversarial learning and diffusion-based image generation. They also demonstrated how the choice of model architecture and generation approach affects the resulting outputs.

## 16. References

1. Xiao, H., Rasul, K. and Vollgraf, R. (2017). *Fashion-MNIST: A Novel Image Dataset for Benchmarking Machine Learning Algorithms.*
2. Kingma, D. P. and Welling, M. (2013). *Auto-Encoding Variational Bayes.*
3. Radford, A., Metz, L. and Chintala, S. (2015). *Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks.*
4. [TensorFlow Documentation](https://www.tensorflow.org/)
5. [Fashion-MNIST Dataset](https://github.com/zalandoresearch/fashion-mnist)
6. [Hugging Face Diffusers Documentation](https://huggingface.co/docs/diffusers)
7. [Stable Diffusion v1.5 Model](https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-v1-5)

---

**Prepared by:** Sakshi Uttam Walunj  
**PRN:** 202502110007  
**Course:** Generative AI Lab  
**Department:** CSE (AI & ML)  
**Institute:** MIT Academy of Engineering, Alandi, Pune
