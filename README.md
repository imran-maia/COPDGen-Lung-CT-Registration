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
