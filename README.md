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
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/9e835a4d-e931-407f-bf13-f582ce202146" width="700" alt="Pre-processed Image">
</p>
<p align="center">Figure 2: Proposed Pipeline for Image Pre-processing and Lung Segmentation.</p>

## Intensity Based Registration

