# Fake Colorized Image Detection with Channel Difference Maps

This repository implements a deep learning framework for detecting fake colorized images using a novel **Channel Difference Map (CDM)** based approach, as described in the paper *"[Channel Difference Based Regeneration Architecture for Fake Colorized Image Detection](https://link.springer.com/chapter/10.1007/978-3-031-11349-9_7)"* by Shruti S. Phutke and Subrahmanyam Murala (CVIP 2021). The project leverages a two-stage architecture: an autoencoder for image regeneration and a transfer learning-based detection network to classify images as real or fake colorized.

## Overview

The advancement of image editing technologies has made it easier to create convincingly fake colorized images, often used for fraudulent purposes like selling counterfeit products. This project introduces a **Channel Difference Map (CDM)** based method to detect such manipulations. The CDM captures subtle differences in color channels (R-G, G-B, R-B), which are then used by a regeneration network (autoencoder) and a detection network to distinguish real images from fake colorized ones.

### Key Features
- **Channel Difference Maps (CDM):** Extracts distinguishing edge and color information from RGB images.
- **Regeneration Network:** An autoencoder that reconstructs images using CDM, providing learned features for downstream tasks.
- **Detection Network (DCDNet):** A transfer learning-based classifier that uses encoder weights from the regeneration network to detect fake colorized images.
- **Evaluation Metrics:** Includes Half Total Error Rate (HTER), accuracy, precision, recall, F1 score, and ROC AUC.
- **Visualization Tools:** Visualizes CDMs, training history, confusion matrices, and ROC curves.

### Architecture
1. **Regeneration Network:** Uses parallel encoder paths for each CDM channel (R-G, G-B, R-B), followed by a dense module with scale blocks and a decoder with skip connections.
2. **Detection Network:** Transfers encoder weights from the regeneration network, processes features through a residual block, and outputs a binary classification (real vs. fake).


### Prerequisites
- Python 3.8+
- TensorFlow 2.x
- OpenCV (`cv2`)
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- A GPU (recommended for training, e.g., NVIDIA with CUDA support)

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/ayushkumar912/CDM_based_Fake_Image_Detection.git
   cd CDM_based_Fake_Image_Detection 
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

   Example `requirements.txt`:
   ```
   tensorflow>=2.10.0
   opencv-python>=4.5.0
   numpy>=1.21.0
   matplotlib>=3.5.0
   seaborn>=0.11.0
   scikit-learn>=1.0.0
   tqdm>=4.62.0
   ```

3. (Optional) Verify TensorFlow GPU support:
   ```python
   import tensorflow as tf
   print(tf.config.list_physical_devices('GPU'))
   ```

## Usage

The project supports three modes: `train`, `test`, and `visualize`. Below are examples of how to use each mode.

### Directory Structure
```
fake-colorized-image-detection/
├── data/
│   ├── real/           # Real images (e.g., *.jpg)
│   └── fake/           # Fake colorized images (e.g., *.jpg)
├── src/
│   ├── cdm_utils.py    # CDM creation and visualization
│   ├── models.py       # Network architectures
│   ├── training.py     # Training functions
│   ├── evaluation.py   # Evaluation metrics
│   ├── visualization.py # Plotting utilities
│   └── data_utils.py   # Data loading and generators
├── outputs/
│   ├── models/         # Saved models
│   └── results/        # Plots and visualizations
├── main.py             # Main script
└── README.md
```

### 1. Training the Model
Train both the regeneration and detection networks:
```bash
python main.py --mode train --data_path ./data --batch_size 16 --img_size 256 --epochs_regen 50 --epochs_detect 50
```
- Outputs: Trained models saved in `outputs/models/`, plots in `outputs/results/`.
- Metrics: Printed to console (e.g., HTER, accuracy).

### 2. Testing an Image
Classify a single image as real or fake using a pre-trained model:
```bash
python main.py --mode test --test_image path/to/image.jpg --model_path outputs/models/detection_model.h5 --img_size 256
```
- Output: Prediction (e.g., "Fake Colorized with 0.95 confidence").

### 3. Visualizing CDM
Visualize the Channel Difference Maps for an image:
```bash
python main.py --mode visualize --test_image path/to/image.jpg
```
- Output: Plot saved to `outputs/results/cdm_visualization.png`.

## Datasets
The original paper uses datasets from ImageNet and Oxford Building Dataset, with fake images generated by methods from [9, 13, 16, 37]. For your own data:
- Place real images in `data/real/`.
- Place fake colorized images in `data/fake/`.
- Ensure images are in JPG format.

## Results
The proposed method outperforms state-of-the-art approaches in terms of HTER on benchmark datasets (D2-D7). See the paper for detailed comparisons (Table 1).

### Example Metrics (from paper)
| Dataset | HTER (%) - Proposed Method | HTER (%) - Baseline [9] |
|---------|----------------------------|-------------------------|
| D2      | 1.57                       | 8.41                    |
| D5      | 0.10                       | 1.25                    |

## Contributing
Contributions are welcome! Please:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

### Ideas for Improvement
- Add data augmentation to reduce overfitting.
- Experiment with different optimizers (e.g., Adam).
- Support additional image formats (e.g., PNG).

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Citation
If you use this code in your research, please cite the original paper:
```
@InProceedings{Phutke_2022_CVIP,
    author="Phutke, Shruti S. and Murala, Subrahmanyam",
    title="Channel Difference Based Regeneration Architecture for Fake Colorized Image Detection",
    booktitle="Computer Vision and Image Processing",
    year="2022",
    publisher="Springer",
    pages="73--84",
    doi="10.1007/978-3-031-11349-9_7"
}
```

## Acknowledgments
- Authors of the original paper for their innovative approach.
- xAI for providing Grok, which assisted in generating this README.

---

