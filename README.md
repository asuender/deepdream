# DeepDream Reformulated: Direct Latent Optimization

![DeepDream Preview](images/preview.png)

> "We haven’t built a painter; we’ve built a pair of digital glasses. We can now look at any ordinary photo and force the computer to squint at it until it dreams a new meaning into the pixels."

**DeepDream Reformulated** is a modern implementation of DeepDream using **CLIP (Contrastive Language-Image Pre-training)**. 

Unlike traditional style transfer or diffusion models which generate images from scratch, this tool uses **Direct Latent Optimization** to steer the pixels of an *existing* image towards a text prompt while preserving the original structure and topology.

## Features

-   **Zero-Shot Steering**: Use any text prompt to transform an image.
-   **Topology Preservation**: Uses Weighted Total Variation loss to keep smooth areas smooth (like skies) while hallucinating details in textured areas.
-   **Multi-View Cutout Optimization**: Implements a Multi-View Cutout strategy with augmentation to prevent adversarial noise.
-   **High-Fidelity**: Uses the powerful **ViT-H-14** CLIP model for deep semantic understanding.
-   **Octave Schedule**: Optimizes across multiple scales (Low -> Mid -> High frequency) for organic details.

## Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/yourusername/deepdream-reformulated.git
    cd deepdream-reformulated
    ```

2.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

## Usage

1.  Open the notebook:
    ```bash
    jupyter notebook DeepDream_Reformulated.ipynb
    ```

2.  Configure your image and prompt in the **Execution** cell:
    ```python
    IMAGE_URL = "images/forest_trail_1024.jpg"
    PROMPT = "Image representing Love - 8k"
    ```

3.  Run all cells. The notebook will visualize the optimization process and save the final result.

## Running on Lower VRAM

The default configuration (ViT-H-14) generally requires a high-end GPU (20GB+ VRAM). For consumer GPUs (8GB - 16GB), make the following adjustments in the notebook:

1.  **Switch Model**: Change `model_name` from `'ViT-H-14'` to `'ViT-L-14'`.
2.  **Reduce Batch Size**: Lower `cutout_batch_size` (e.g., from 40 to 8 or 16). This reduces peak memory usage while keeping the total number of cutouts the same (gradient accumulation).
3.  **Reduce Resolution**: Lower `base_size` (e.g., from 1024 to 512).

## How It Works

This project replaces the original DeepDream inception classifier with CLIP, allowing for arbitrary concept hallucination. It creates a "Smoothness Map" of your image to identify where to allow hallucinations and where to enforce smoothness, ensuring the result looks like a coherent remix of the original rather than a noisy mess.

## Configuration Guide

You can tweak the following hyperparameters in the `dream_masked` function to control the result:

| Parameter | Default | Description |
| :--- | :--- | :--- |
| `text_prompt` | - | The concept you want to hallucinate (e.g., "Love", "Space", "Fire"). |
| `negative_prompt` | - | Concepts to avoid (e.g., "blur", "noise"). |
| `negative_weight` | `0.5` | Strength of the negative prompt optimization. |
| `tv_weight` | `0.005` | **Total Variation Weight**. Controls smoothness. Higher = smoother/blurrier, Lower = more noise/texture. |
| `global_view_weight` | `0.03` | Strength of the full-image consistency check. Higher = stays closer to original structure. |
| `octave_scales` | `[1.0]` | List of scales to optimize. `[0.5, 1.0, 1.5]` optimizes at half, full, and 1.5x res. |
| `steps_per_octave` | `50` | Number of optimization steps per scale. More steps = stronger effect. |
| `learning_rate` | `0.05` | How fast the image changes. Too high = artifacts, Too low = no change. |
| `num_cutouts` | `64` | Number of random crops per batch. Higher (128+) = better quality but slower. |
| `mask_threshold` | `20` | Controls the "Smoothness Map". Lower = protects more detail, Higher = smooths more area. |
| `gradient_blur_kernel_size` | `3` | Smoothing applied to gradients before optimization. |

## License

MIT License. See [LICENSE](LICENSE) for details.
