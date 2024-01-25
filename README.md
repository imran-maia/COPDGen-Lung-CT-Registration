# Lung Computed Tomography Image Registration: A Comparative Study between Intensity Based Method and Deep Learning

Author(s): **Mohammad Imran Hossain**, Muhammad Zain Amin
<br>University of Girona (Spain), Erasmus Mundus Joint Master in Medical Imaging and Applications

**Abstract:** The process of image registration not only aids in diagnosing directly from the aligned images but also significantly impacts the performance of algorithms supporting medical imaging diagnosis. This project used inspiratory and expiratory Breath-Hold CT image pairs from the COPDgene study repository. Our experimentation focused on various aspects of the registration framework, including the similarity metric, geometric transformation, interpolation, and resolutions. By preprocessing intensities, we reduced variations across images, enhancing comparability and facilitating easier image registration. Utilizing Elastix and Transformix computer software, we performed intensity-based registrations (rigid and non-rigid) on unsegmented and segmented lung structures. By defining the region of interest and eliminating non-essential areas, our approach yielded an average mean TRE (Target Registration Error) of **2.08 ± 2.26 mm**, showcasing the effectiveness of our approach and fine-tuned parameters. Furthermore, using VoxelMorph, we obtained an average mean TRE of **2.18 ± 2.74 mm** for the same cases.

# Dataset
The dataset, COPDgene, used for the lung CT image registration challenge is a publicly available dataset by the National Heart Lung Blood Institute in the United States. It consists of ten distinct cases: labeled COPD1-COPD10, however; we used COPD1-COPD4 images for the registration challenge. For each case, there are two raw CT images for exhale and inhale conditions with their corresponding landmarks.


<p align="center">
  <img src="https://github.com/imran-maia/COPDGen-Lung-CT-Registration/assets/122020364/d2192951-a00c-4fa2-9914-ee9d68289698" width="500" alt="Pre-processed Image">
</p>
<p align="center">Figure 1: The Exhale eBH-CT (Left) Slice and The Inhale iBH-CT (Right) Slice of COPD1 Image with Corresponding Landmark Points.</p>
