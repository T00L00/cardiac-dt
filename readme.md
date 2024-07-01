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

### 3rd Party tools used

1. CLARAnet for MRI image segmentation
2. [ITK-SNAP](http://www.itksnap.org) for manual refinement of MRI images
3. [Materialise Mimics](https://www.materialise.com) for 3D mesh generation
4. [openCARP](https://opencarp.org) for electrophysiology simulation
5. [Meshalyzer](https://github.com/cardiosolv/meshalyzer) for rotor detection on 3D models

## Details

1. Patient Imaging
    
    - Cardiac MRI (CMR): Pre-procedure CMR was performed using a 1.5 Tesla MRI scanner to acquire detailed images of the atria.
    
    - Contrast-Enhanced MRI: Time-resolved contrast-enhanced magnetic resonance angiography was used to assess left atrium (LA) and pulmonary vein (PV) anatomy.
    
    - Late Gadolinium Enhancement MRI (LGE-MRI): A 3D LGE-MRI scan was performed 20-25 minutes after administering a gadolinium-based contrast agent to visualize fibrosis in the atrial tissue.

2. Image Processing and Fibrosis Mapping

    - Segmentation and Quality Control: A convolutional deep neural network (CLARAnet) and manual quality control with [ITK-SNAP](http://www.itksnap.org) were used to segment and refine the atrial anatomy.
    
    - Personalized Intensity Ratio (PIIR): Custom method to differentiate fibrotic from non-fibrotic tissue. This method normalizes atrial intensity ratios by the mean intensity of the aorta wall, minimizing variability due to imaging conditions.
    
    - Fibrosis Thresholding: Using the PIIR, voxels with intensities above a specific threshold were classified as fibrotic.

3. Mesh Generation and Fiber Mapping

    - 3D Tetrahedral Mesh: The segmented LGE-MRI images were transformed into a 3D tetrahedral mesh with a target edge length of 300 μm using [Materialise Mimics](https://www.materialise.com).
    
    - Fiber Directions: Fiber directions were computed from diffusion tensor imaging data and mapped onto the patient-specific meshes using custom software and the Universal Atrial Coordinates approach.

4. Electrophysiological Modeling

    - Membrane Kinetics: A human chronic AF atrial action potential model was used to represent membrane kinetics in non-fibrotic regions. In fibrotic regions, modifications were made to represent the reduced IK1, ICaL, and INa channel conductances. Details can be found in references 27, 49, 50, 58.

    - Conduction Properties: The non-fibrotic longitudinal conduction velocity was set at 43.39 cm/s, while fibrotic regions had a reduced velocity of 20 cm/s. The anisotropy ratios were 5:1 for non-fibrotic and 8:1 for fibrotic tissue.

5. Simulation Execution

    - Software and Hardware: Simulations were executed on a parallel computing system using [openCARP](https://opencarp.org)
    
    - Virtual Ablation: Ablation lesions were represented by non-conductive tissue replacements in the DTs. Linear lesions for PVI had a width of 7.5 mm, while rotor-targeting lesions had a diameter of 12 mm on the surface, extending transmurally.

6. Rotor Induction and Analysis

    - Pacing and Inducibility Tests: The DTs underwent sequential burst pacing from 40 sites to induce rotors. These sites represented potential triggers in the fibrotic substrate outside the PVI lines.
    
    - Rotor Detection: Rotors were identified by examining phase singularity trajectories using [Meshalyzer](https://github.com/cardiosolv/meshalyzer). Rotor locations were classified based on their behavior post-ablation into locations of independent rotors (LIRs) and locations of contingent rotors (LCRs).

7. Assessment of Arrhythmogenicity

    - Fibrosis Distribution Analysis: Fibrosis distribution at rotor locations was evaluated using fibrosis density (FD) and fibrosis entropy (FE). FD was calculated as the ratio of fibrotic volume to total volume within a rotor location.
    
    - Sequential Ablation Strategy: Various ablation strategies were simulated to determine the minimum set of lesions required to achieve rotor non-inducibility while preserving atrial function and reducing the risk of iatrogenic atrial tachycardia (iAT).

8. Clinical and Electrogram Analysis

    - Electroanatomical Mapping (EAM): During the ablation procedure, intra-atrial electrograms were recorded using a multi-electrode mapping catheter and analyzed to distinguish LIRs and LCRs.
    
    - Bipolar Voltage and FAAM Analysis: Bipolar voltage and fractionated signal area in atrial muscle (FAAM) potentials were analyzed to identify the characteristics of rotor locations and their propensity to induce iAT post-ablation.


## Thoughts

We won't be able to replicate the 3D mesh creation process because:

1. We don't have the original patient MRI scans.
2. The 3D meshes were generated by a 3rd party service.

Our replication can start at the complete 3D meshes themselves. We then use openCARP to run the electrophysiology simulations, provided we can obtain the 3D meshes from JHU.

### Questions

- What part of the pipeline do we want to replicate for our own use? MRI image to 3D Mesh + Simulations? Or do we just care about being able to simulate the electrophysiology on a given a 3D heart mesh?


## Tasks

- [ ] Obtain 3D meshes from JHU
- [ ] Download openCARP and perform electrophysiology simulations with the given parameters in the study
- [ ] Compare sim results with paper's results

## Terminologies

- PVI
  - Pulmonary vein isolation
  - Procedure to ablate around the openings of the 4 pulmonary veins leading to the R atrium 
  - Electrical signals from the pulmonary veins get isolated from the rest of the heart and prevents A-fib

- rotors
  - fibrotic sites that can sustain an electrical current
  - numerous rotors present disrupts the natural cardiac heartbeat, leading to atrial fibrillation

- arrhythmogenic propensity
  - tendency of a particular region of cardiac tissue to generate and sustain arrhythmias

- Sequential rapid pacing
  - applying electrical stimulation at high frequency to induce rotors

- Locations of Independent Rotors (LIR)
  - self-sustaining rotors

- Locations of Contingent Rotors (LCR)
  - rotors that require other nearby rotors in order to run

- Fibrosis density (FD)

- Fractionated Signal Area in Atrial Muscle (FAAM)
  - Specific types of electrical signals recorded from atrial tissue. Characterized by fractionated appearance (i.e. small deflections in a single activation complex).
  - indicates areas of slow disrupted conduction

- Interval Confidence Level (ICL)
  - quantifies the degree of fractionation in the FAAM
  - measures the percentage of the interval during which fractionated activity is observed in FAAM potentials
  - higher ICL = greater extent of fractionation and slow conduction



## Results

### General

- 122 rotor locations outside of PVI lines in 21 patients
  - 34 in LA + 88 in RA. Indicates that RA is critically involved in causing Afib.
  - 77 LIRs + 45 LCRs
  - LIRs mostly found at atrial septum
  - LCRs mostly found at RA lateral wall

- Ablation at 81 out of 122 total rotor locations ablated to achieve non-inducibility across all patients. Equates to a 34% reduction in ablations needed to achieve non-inducibility in the DTs.

### Relationship between FD, FE and LIR, LCR

- Sites with higher FD and FE likely harbor rotors

- Number of rotor locations and number of LIRs correlated with global fibrosis amount (rather than atrial volume). See Fig 5.

- FD (at 67% cutoff) is a better predictor for a rotor being LIR and LCR. This means that local density of fibrosis (instead of local heterogeneity of the fibrosis) is a better indicator of a rotor's persistence. 

### iAT-inducing locations

- FD (at 55%) also a better predictor of a rotor being an iAT-inducing location

- ICL (at 32%) also good predictor of a rotor being an iAT-inducing location

- iAT-inducing locations have higher ICL% compared to iAT-free locations. More pronounced fractionation and slow conductance are more likely to develop re-entrant circuits leading to iAT.


