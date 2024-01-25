# Lung Computed Tomography Image Registration: A Comparative Study between Intensity Based Method and Deep Learning

Author(s): **Mohammad Imran Hossain**, Muhammad Zain Amin
<br>University of Girona (Spain), Erasmus Mundus Joint Master in Medical Imaging and Applications

**Abstract:** Image registration, essential for both direct diagnosis from aligned images and optimizing algorithmic performance in medical imaging, played a central role in this project. In this project, we utilized inspiratory and expiratory Breath-Hold CT image pairs from the COPDgene study repository, with experimentation focusing on key aspects of the registration framework, including the choice of similarity metric, geometric transformation, interpolation, and resolutions. To reduce variations across images and enhance comparability for easier image registration, we preprocessed intensities. Utilizing Elastix and Transformix computer software, we conducted intensity-based registrations, both rigid and non-rigid, on unsegmented and segmented lung structures. Our approach involved defining a region of interest and excluding non-essential areas. As a result, our method demonstrated an average mean Target Registration Error (TRE) of **2.08 ± 2.26 mm**, highlighting the effectiveness of our approach and the fine-tuned parameters. Additionally, using VoxelMorph, we achieved an average mean TRE of **2.18 ± 2.74 mm** for the same cases. These findings underscore the success of our methodology in achieving accurate image registration and its potential impact on supporting medical imaging diagnosis.

# Dataset
The dataset, COPDgene, used for the lung CT image registration challenge is a publicly available dataset by the National Heart Lung Blood Institute in the United States. It consists of ten distinct cases: labeled COPD1-COPD10, however; we used COPD1-COPD4 images for the registration challenge. For each case, there are two raw CT images for exhale and inhale conditions with their corresponding landmarks.


<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/d2192951-a00c-4fa2-9914-ee9d68289698" width="500" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: The Exhale eBH-CT (Left) Slice and The Inhale iBH-CT (Right) Slice of COPD1 Image with Corresponding Landmark Points.</p>

# Methodology

The methodology for Lung Computed Tomography (CT) image registration involves a meticulous process to accurately align inspiratory and expiratory CT image pairs. Employing both intensity-based and deep learning approaches, our study places particular emphasis on robust pre-processing and lung segmentation techniques.

## Pre-processing and Lung Segmentation

### 1. Metadata Extraction and NIfTI Format Conversion
   - Detailed metadata, including image dimensions, voxel spacing, mean, and standard deviation of displacement, is extracted during pre-processing.
   - The SimpleITK library is utilized for effective conversion to the standardized NIfTI format, ensuring compatibility with diverse medical image processing tools.

### 2. Lung Segmentation Pipeline
   The segmentation of lung images is critical, especially in the context of CT lung imaging with inherent variability due to breathing motion. To address this, segmentation masks are employed to define Regions of Interest (ROIs) with irregular shapes, focusing on dynamic lung aspects while considering static anatomical features.

   - **Thresholding:**
     - Analysis of voxel intensity histograms reveals intensities exceeding 800 are irrelevant for lung segmentation.
     - Thresholding within the range of 0-800 is applied, creating a binary mask isolating potential lung regions.

   - **Morphological Operations:**
     - Closing and opening operations are applied to the binary mask to eliminate noisy voxels and refine the lung region.

   - **Region of Interest (ROI):**
     - Midpoints are identified to extract the lung region accurately, creating an essential lung mask.

   - **Connected Components:**
     - Labeling is performed to distinguish connected components within the binary lung mask, identifying primary lung areas.

   - **Lung Masks and Segmentation:**
     - Largest connected components are used to generate lung masks, producing a segmented image with isolated lung regions.

### 3. Further Processing
   - Steps involve normalizing intensities, enhancing contrast through Contrast-Limited Adaptive Histogram Equalization (CLAHE) [9], and rescaling segmented lung regions to their original intensity range.
   - This method enhances the visibility and isolation of lung tissues, aiding medical professionals in analyzing and diagnosing various pulmonary conditions or diseases from CT scan data.

The comprehensive segmentation workflow is visually illustrated in Figure 2, providing a clear guide to the intricate steps involved.

<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/9e835a4d-e931-407f-bf13-f582ce202146" width="600" alt="Pre-processed Image">
</p>
<p align="center">Figure 2: Proposed Pipeline for Image Pre-processing and Lung Segmentation.</p>

## Intensity Based Registration
Registration methodologies that use voxel or pixel intensity values are commonly known as intensity-based. The main idea of intensity-based image registration is to iteratively search for the geometric transformation that optimizes a similarity measure, between the fixed image and the transformed moving image. In this project, we have used ***elastix-transformix*** tools developed by SimpleITK for performing intensity-based image registration. Our proposed pipeline of lung CT registration based on elastix-transformix consists of several steps shown in Figure 3.

### 1. Elastix 
Elastix is a robust platform for intensity-based image registration, employing various transformations. It iteratively enhances the similarity metric between fixed (inhale) and moving (exhale) images to achieve optimal transformation. Notably, Elastix supports multi-resolution registration using image pyramids and allows customization of key components, including interpolators, optimizers, metrics, iteration counts, and spatial samples.

- Initiation with pre-processing and segmentation of fixed and moving lung CT images.
- Segmented images are input into Elastix with predefined registration parameters.
- Elastix outputs the registered image and transform parameters, pivotal for generating precise landmarks using Transformix.

### 2. Registration Parameter
In the context of elastix-based image registration, the parameter file is a critical element for successful outcomes. It encodes the specific transformation instructions required to register the moving (exhale) image with the fixed (inhale) image accurately. For chest CT images, particularly for lung registration, the Elastix Model Zoo offers a variety of parameter files. In this project, three-parameter files—**Par007, Par0011, and Par0056**—were identified as potentially suitable. These were selected based on their initial compatibility with the task requirements

### 3. Transformix
In deformable image registration using Elastix, Transformix plays a crucial role. After registering inhale and exhale images with different parameter files, Transformix transforms inhale-phase landmarks, aligning them with exhale-phase landmarks. The process utilizes Elastix-determined parameters, adjusting inhale landmarks to align precisely with the target, either an exhale-phase image or another point in a dynamic sequence. The transformation ensures accurate alignment by registering the moving exhale lung image against the fixed inhale lung image, projecting coordinates internally for precision.
<br>

<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/2b96e46b-6438-4b82-8f18-1783a7c59f81" width="900" alt="Pre-processed Image">
</p>
<p align="center">Figure 3: Proposed Pipeline for Intensity-Based Registration Using elastix-transformix.</p>

## Deep Learning-Based Registration

Contrary to intensity-based registration methods, recent attention from researchers has been drawn towards learning-based approaches utilizing neural networks. One such approach is VoxelMorph, which operates as a deep learning model without requiring supervised information for registration.

### 1. Dataset and Preprocessing
- Used COPD1-COPD4 datasets, split 75:25 for training and testing.
- Standardized inputs by resizing all sample volumes to (256 × 256 × 128) before utilizing them in the VoxelMorph network.

### 2. VoxelMorph Architecture
- VoxelMorph employs a convolutional neural network (CNN) with a U-Net-like architecture.
- The model represents a function gθ (F, M ) = φ, where φ is a registration field, and θ denotes the learnable parameters of g.
- For each voxel p ∈ ω, φ(p) denotes a location, managing significant displacements within the registration field.

### 3. Experiment Setup
- Utilized Keras with TensorFlow on Google Colab with a V100 GPU and 16GB RAM.
- Custom data generator processed fixed (Inhale) and moving (Exhale) image volumes, producing a concatenated tensor.
- The output of the network was the registration field φ.

### 4. Training Process
- Involved a two-part loss function: maximizing a similarity metric between fixed and moving images and smoothing the registration field.
- Experimented with similarity metrics such as Mean Squared Error (MSE) and Normalized Cross-Correlation (NCC).

### 5. Experimental Rounds
Two rounds of experiments were conducted with VoxelMorph:
1. Applied VoxelMorph directly on unprocessed image volumes.
2. Aligned moving images to fixed ones using Elastix before implementing VoxelMorph’s local non-linear registration.


<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/e7e05eea-72cd-4a9b-828d-c60bac5a4daa" width="800" alt="Pre-processed Image">
</p>
<p align="center">Figure 4: Proposed Pipeline for Deep Learning Based Registration Using VoxelMorph.</p>


# Results
We conducted a comprehensive experiment to perform image registration, employing both an intensity-based approach and deep learning. The intensity-based registration was carried out in two phases: initially, using the original inhale and exhale images without any pre-processing, and subsequently, repeating the registration process using segmented lung images. In the realm of deep learning, two experiments were conducted as well. In the first, the VoxelMorph model was trained using preprocessed segmented lung images and then utilized for image registration. In the second experiment, a different approach was employed. Instead of using preprocessed images directly, the VoxelMorph model was trained with images that had already undergone registration using the elastix tool with different parameters. To assess the registration performance in all experiments, we computed the evaluation metric, Mean and Standard Deviation of Target Registration Error (TRE) in millimeters, as summarized in Tables 1, 2, and 3.

<p align="center">Table 1: Analysis of Results without Pre-processing and Measurement of Target Registration Error (TRE) in (mm).</p>
<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/6890e57e-e80c-4a70-a436-059cc2b1b0a7" width="900" alt="Pre-processed Image">
</p>

<p align="center">Table 2: Analysis of Results with Pre-processing and Measurement of Target Registration Error (TRE) in (mm).</p>
<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/5fc3bba6-7345-4697-b55f-983cb7889bae" width="900" alt="Pre-processed Image">
</p>


<p align="center">Table 3: Analysis of Results with VoxelMorph and Measurement of Target Registration Error (TRE) in (mm).</p>
<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/5d813123-318a-459c-990e-be407c63531e" width="900" alt="Pre-processed Image">
</p>

<br>
The results demonstrated that the intensity-based method, specifically the elastix-based registration utilizing affine and B-spline transformation parameters with pre-processed segmented lung images, exhibited superior performance. While the deep learning model, voxelMorph, exhibited potential, its performance was hindered by the limited volume of training data. Figure 4 (a) illustrates the statistical robustness of this approach through a box plot, reflecting the consistency and limited variability in TRE across multiple datasets of COPDgen. Furthermore, the visual comparison provided in Figure 4 (b) offers a clear depiction of the registration process, showcasing the fixed (inhale) image, moving (exhale) image, and the resulting aligned image post-registration. The images in both axial and coronal views reinforce the efficacy of the selected registration pipeline<br>.

<br>
<br>
<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/c0ad4dd8-34f0-4c79-a3ea-01d9a87b9b7d" width="400" alt="Pre-processed Image">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/2072781c-709b-4865-9017-6b710b70b743" width="400" alt="Pre-processed Image">
</p>
<p align="center">Figure 4: (a) Evaluation metric for the best model (b) Registration output (axial-coronal view) for the best model..</p>
