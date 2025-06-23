# Stable-Diffusion-Transformer
Reimplementation of Stable Diffusion's DDIM with a Diffusion Transformer as per (Peebles & Xie, 2022) [[arXiv](https://arxiv.org/abs/2212.09748)]

## **Autoencoder**
**Input**: 
    256×256 RGB images  
- **Latents**:
    Encoded to a 32×32×4 tensor — a 48,000 % compression
- **Dataset**:
    Trained on the MSCOCO 2017 dataset  
- **Architecture**:
    Encoder, Decoder architecture featuring resnet-style residual blocks, followed by either 2x2 bilinear upsampling (in the decoder) or a 2-strided 2D convolution (in the encoder). The encoder and decoder also         feature a bottleneck layer incorporating 2D spatial self-attention blocks to extract long-distance relationships between latent features.
- **Training**:
    1) VGG Perceptual Loss as per Zhang et al., 2018 [[arXiv](https://arxiv.org/abs/1801.03924)]
    2) KL Divergence Loss as per Kingma et al., 2018 [[arXiv](https://arxiv.org/abs/1312.6114)]
       ⚠️ Note: My encoder does not output a mean and log-var (μ, logσ²) pair as in a standard VAE. Instead, it deterministically outputs a singular latent vector.  
          To encourage a Gaussian prior, I apply a quasi-KL divergence loss between the encoder output and a standard normal distribution. This acts as a regularizer, loosely encouraging the latent space to remain            centered and isotropic.
    3) PatchGAN Discriminator Loss as per Isola et al., 2018 [[arXiv](https://arxiv.org/pdf/1611.07004)]
