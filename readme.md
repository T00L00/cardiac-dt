# Steps in constructing personalized digital twins

Gitlab [repo](https://gitlab.com/natalia-trayanova/assessing-the-arrhythmogenic-propensity-of-fibrotic-substrate-using-digital-twins) for study 

Repo should contain:  
- parameter files for openCARP simulations
- source data used to produce Figure 3
- code and data to produce Fig 2, 5, 6

3D meshes available upon request to Prof. Natalia Trayanova (ntrayanova@jhu.edu)

## TLDR

1. Obtain cardiac MRI scan of patients

2. Segment and adjust MRI scan images to accurately show atrial anatomy

3. Use PIIR method to distinguish between fibrotic and non-fibrotic tissue

4. Generate 3D mesh from segmented, labeled images

5. Simulate electrophysiology on DTs
    - non-fibrotic tissue: chronic AF atrial action potential model
    - fibrotic tissue: chronic AF atrial action potential model modified by reducing K+, Ca2+, and Na+ channel conductances
    - Simulate ablation by replacing fibrotic tissue with non-fibrotic tissue
    - Apply sequential burst pacing from 40 sites to induce rotors

6. Try various ablation strategies to determine optimal ablations needed to treat atrial fibrillation

7. Compare simulation results with actual ablation surgery results. 

## Details

1. Patient Enrollment and Imaging:

    - Patient Selection: Patients with PsAF undergoing catheter ablation were enrolled in the study.
    
    - Cardiac MRI (CMR): Pre-procedure CMR was performed using a 1.5 Tesla MRI scanner to acquire detailed images of the atria.
    
    - Contrast-Enhanced MRI: Time-resolved contrast-enhanced magnetic resonance angiography was used to assess left atrium (LA) and pulmonary vein (PV) anatomy.
    
    - Late Gadolinium Enhancement MRI (LGE-MRI): A 3D LGE-MRI scan was performed 20-25 minutes after administering a gadolinium-based contrast agent to visualize fibrosis in the atrial tissue.

2. Image Processing and Fibrosis Mapping:

    - Segmentation and Quality Control: A convolutional deep neural network (CLARAnet) and manual quality control with [ITK-SNAP](http://www.itksnap.org) were used to segment and refine the atrial anatomy, ensuring accurate representation.
    
    - Personalized Intensity Ratio (PIIR): A new method, PIIR, was used to differentiate fibrotic from non-fibrotic tissue. This method normalizes atrial intensity ratios by the mean intensity of the aorta wall, minimizing variability due to imaging conditions.
    
    - Fibrosis Thresholding: Using the PIIR, voxels with intensities above a specific threshold were classified as fibrotic.

3. Mesh Generation and Fiber Mapping:

    - 3D Tetrahedral Mesh: The segmented LGE-MRI images were transformed into a 3D tetrahedral mesh with a target edge length of 300 μm using [Materialise Mimics](https://www.materialise.com).
    
    - Fiber Directions: Fiber directions were computed from diffusion tensor imaging data and mapped onto the patient-specific meshes using custom software and the Universal Atrial Coordinates approach.

4. Electrophysiological Modeling:

    - Membrane Kinetics: A human chronic AF atrial action potential model was used to represent membrane kinetics in non-fibrotic regions. In fibrotic regions, modifications were made to represent the effects of elevated transforming growth factor β1, reducing IK1, ICaL, and INa channel conductances. Details can be found in references 27, 49, 50, 58.

    - Conduction Properties: The non-fibrotic longitudinal conduction velocity was set at 43.39 cm/s, while fibrotic regions had a reduced velocity of 20 cm/s. The anisotropy ratios were 5:1 for non-fibrotic and 8:1 for fibrotic tissue.

5. Simulation Execution:

    - Software and Hardware: Simulations were executed on a parallel computing system using [openCARP](https://opencarp.org)
    
    - Virtual Ablation: Ablation lesions were represented by non-conductive tissue replacements in the DTs. Linear lesions for PVI had a width of 7.5 mm, while rotor-targeting lesions had a diameter of 12 mm on the surface, extending transmurally.

6. Rotor Induction and Analysis:

    - Pacing and Inducibility Tests: The DTs underwent sequential burst pacing from 40 sites to induce rotors. These sites represented potential triggers in the fibrotic substrate outside the PVI lines.
    
    - Rotor Detection: Rotors were identified by examining phase singularity trajectories using [Meshalyzer](https://github.com/cardiosolv/meshalyzer). Rotor locations were classified based on their behavior post-ablation into locations of independent rotors (LIRs) and locations of contingent rotors (LCRs).

7. Assessment of Arrhythmogenicity:

    - Fibrosis Distribution Analysis: Fibrosis distribution at rotor locations was evaluated using fibrosis density (FD) and fibrosis entropy (FE). FD was calculated as the ratio of fibrotic volume to total volume within a rotor location.
    
    - Sequential Ablation Strategy: Various ablation strategies were simulated to determine the minimum set of lesions required to achieve rotor non-inducibility while preserving atrial function and reducing the risk of iatrogenic atrial tachycardia (iAT).

8. Clinical and Electrogram Analysis:

    - Electroanatomical Mapping (EAM): During the ablation procedure, intra-atrial electrograms were recorded using a multi-electrode mapping catheter and analyzed to distinguish LIRs and LCRs.
    
    - Bipolar Voltage and FAAM Analysis: Bipolar voltage and fractionated signal area in atrial muscle (FAAM) potentials were analyzed to identify the characteristics of rotor locations and their propensity to induce iAT post-ablation.




