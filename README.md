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

## How It Works

This project replaces the original DeepDream inception classifier with CLIP, allowing for arbitrary concept hallucination. It creates a "Smoothness Map" of your image to identify where to allow hallucinations and where to enforce smoothness, ensuring the result looks like a coherent remix of the original rather than a noisy mess.

## License

MIT License. See [LICENSE](LICENSE) for details.
