# Advanced Stable Diffusion: Inference, DreamBooth, and LoRA Fine-Tuning

This repository contains a collection of Jupyter notebooks that serve as a comprehensive guide to using and customizing the Stable Diffusion model. We progress from basic inference techniques to advanced personalization methods like DreamBooth for subject training and LoRA for style fine-tuning.

## Table of Contents

1.  [Repository Overview]
2.  [Notebooks]
      - [Part 1: Mastering Stable Diffusion Inference]
      - [Part 2: Personalizing Models with DreamBooth]
      - [Part 3: Efficient Style Training with LoRA]
3.  [Prerequisites]
4.  [How to Use]
5.  [Acknowledgements]

-----

## Repository Overview

The goal of this repository is to provide a practical, hands-on journey into the world of generative AI with Stable Diffusion. The notebooks are structured to build upon each other, demonstrating a clear path from generating your first image to creating a highly specialized custom model.

  - **Start with Inference**: Learn to control the image generation process using text prompts, image prompts, and key parameters.
  - **Train a Subject**: Use DreamBooth to teach the model a new concept, such as your own face, allowing you to generate images of that subject in any style or setting.
  - **Train a Style**: Use the lightweight and efficient LoRA technique to train the model on a specific artistic style, like Qajar painting, without retraining the entire model.

-----

## Notebooks

This repository is divided into three main parts, each contained in its own notebook.

### Part 1: Mastering Stable Diffusion Inference

**File:** `01_Stable_Diffusion_Inference.ipynb`

This notebook covers the fundamental techniques for generating images with a pre-trained Stable Diffusion model (`runwayml/stable-diffusion-v1-5`). It's the perfect starting point for understanding how to interact with the model.

**Key Concepts Covered:**

  - **Text-to-Image (`text2img`)**: Generating images from simple text descriptions.
  - **Style Transfer via Prompts**: Modifying prompts to generate images in specific artistic styles (e.g., Van Gogh, Pixar, Cyberpunk).
  - **Image-to-Image (`img2img`)**: Modifying an existing image based on a text prompt.
  - **Controlling Generation**: Understanding the impact of parameters like `strength` and `guidance_scale` on the final output.
  - **Negative Prompts**: Using negative prompts to guide the model away from undesirable features (e.g., `blurry`, `bad anatomy`), significantly improving image quality.
  - **Refinement Workflow**: Using a `text2img` followed by an `img2img` pass to enhance the details of a generated image.

### Part 2: Personalizing Models with DreamBooth

**File:** `02_DreamBooth_Finetuning.ipynb`

This notebook demonstrates how to personalize Stable Diffusion using **DreamBooth**. We fine-tune the model on a small set of custom images to teach it a new subject (in this case, a person's face).

**Workflow:**

1.  **Setup**: Installs necessary libraries (`diffusers`, `bitsandbytes`, `accelerate`) and downloads the official Hugging Face training script.
2.  **Configuration**: The user provides a few images of a subject, defines a unique identifier (e.g., `"a photo of a sks person"`), and sets up paths for training.
3.  **Training**: Launches the fine-tuning process using `accelerate`. The script trains both the UNet and the text encoder to deeply embed the new concept.
4.  **Inference & Validation**: After training, the custom model is loaded, and we generate images of the learned subject in various creative scenarios.
5.  **Comparison**: The notebook provides a clear visual comparison between the results from the original base model and the DreamBooth fine-tuned model, showcasing the success of the training.

### Part 3: Efficient Style Training with LoRA

**File:** `03_LoRA_Finetuning_with_Kohya.ipynb`

This notebook explores **Low-Rank Adaptation (LoRA)**, a highly efficient fine-tuning technique for teaching the model new styles. We use the popular `kohya_ss` GUI to train a LoRA on the "Qajar" painting style. LoRA modifies the model by training small, additional weight matrices, which are then added to the original weights during inference.

The core idea of LoRA is to update a pre-trained weight matrix $W\_0$ with a low-rank decomposition:

$$W_{final} = W_0 + \alpha \cdot (W_{up} \cdot W_{down})$$

where $\\alpha$ is a scaling factor. This makes training much faster and results in very small output files (usually \< 100 MB).

**This notebook covers two methods for using the trained LoRA:**

1.  **Dynamic Loading**: The base model is loaded first, and the LoRA weights (`.safetensors` file) are applied on-the-fly using `pipe.load_lora_weights()`. This is flexible and allows for mixing and matching different LoRAs.
2.  **Model Merging**: The LoRA weights are permanently merged into the base model's UNet weights. This creates a new, standalone model checkpoint that has the style baked in, simplifying distribution and inference.

The notebook concludes with a comparison of images generated by the base model, the dynamically loaded LoRA, and the fully merged model, confirming that the style has been successfully learned and applied.

-----

## Prerequisites

  - A Python environment (e.g., Google Colab, local Jupyter).
  - A GPU is **highly recommended**, especially for the training notebooks. A T4 GPU on Google Colab is sufficient.
  - Key Python libraries: `torch`, `diffusers`, `transformers`, `accelerate`, `bitsandbytes`. The notebooks include installation commands.
  - For the training notebooks (`02` and `03`), you will need to provide your own set of training images.

## How to Use

1.  **Clone the repository**
2.  **Open in Google Colab or Jupyter:** Upload the `.ipynb` files to your environment.
3.  **Provide Your Data:** For Notebooks 2 and 3, you must upload your own training images and update the file paths in the configuration cells.
4.  **Run the Cells:** Execute the cells sequentially. The notebooks are commented to guide you through each step.

## Acknowledgements

  - **Hugging Face ??**: For the incredible `diffusers` library and the original DreamBooth training script.
  - **runwayml**: For providing the open-source Stable Diffusion v1.5 model.
  - **Kohya-ss**: For the powerful and user-friendly GUI for advanced fine-tuning methods like LoRA.