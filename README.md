# Recolorization Project 🌸🎨

This project addresses the **image recolorization problem**: adding colors to grayscale images when only limited information (mean intensity of R, G, and B channels) is available.  
The solution is based on a **U-Net architecture with residual blocks**, enhanced by **AdaIN conditioning** inspired by StyleGAN.

---

## 📝 Project Overview
The main idea is to leverage the separation of **luminance and chrominance** in the Lab color space:
- The **L channel** (lightness) preserves geometric and intensity information from the grayscale image.
- The **ab channels** (chrominance) are predicted by the network, responsible for reconstructing realistic colors.

This design reduces the complexity of the learning task, letting the model focus only on colorization.

---

## 📂 Dataset
We use the [Oxford Flowers 102 dataset](https://www.robots.ox.ac.uk/~vgg/data/flowers/102/), which contains 102 flower categories with varying shapes and appearances.  
To simplify training:
- Images are resized to a lower resolution (128×128).  
- The dataset is split into grayscale (L) and color components (ab).  
- Data augmentation (geometric transformations and noise) increases robustness and reduces overfitting, since the dataset is relatively small.

---

## ⚙️ Methodology

### Data Processing
- **Generator** returns a tuple:  
  - Input: *(grayscale image, condition vector)*  
  - Target: *ab color channels*  
- Training is done in Lab space.  
- For inference: concatenate `L + predicted ab` → convert back to RGB.  

### Baseline
As a reference, a naive baseline uniformly assigns colors from the provided palette. This model serves as a benchmark to evaluate the network’s performance.

### Network Architecture
- **U-Net with residual blocks** ensures effective feature extraction.  
- **AdaIN conditioning** integrates style information, guiding color prediction.  
- **Loss objective** focuses only on chrominance channels.  

---

## 🚀 How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/FiloSamo/Colorization_OxfordFlowers102.git
   cd Colorization_OxfordFlowers102
   pip install -r requirements.txt
   jupyter notebook recolorization_project.ipynb
---

## 🧑‍💻 Author

Filippo Samorì
Project developed as part of academic work in deep learning.

---

## 📊 Results
- The network produces colorized images that are visually closer to the ground truth compared to the baseline.  
- Results are evaluated qualitatively by comparing reconstructed images with real colored ones.  
- Flowers with distinctive shapes and strong chromatic cues are recolored more faithfully.  

![Results](results.png)
