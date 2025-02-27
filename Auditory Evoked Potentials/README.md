# Auditory Evoked Potentials
Brainstorm custom-made process to analyse auditory evoked potentials (AEP).
<BR>Developed mainly to analyse auditory brainstem responses (ABR; an early component of the AEP) but can be used for later potentials such as cortical auditory evoked potentials (CAEP).

## How the processes work
AEPs rely on averaging many hundreds or thousands of stimulus-presented time-locked trials/epochs and all the processes below were made for preprocessed, individual epochs. 
Therefore each individual process takes in X number of epochs and generates one or two files of a single measurement type (e.g. one residual noise measurement from 6000 ABR epochs).
<BR><BR>The individual processes are most useful if the intention is to calculate a single measurement type rather than several at once.
If the intention is to calculate several measurement types (e.g. residual noise, weighted average and noise per epoch from X number of epochs), the combined processes are most useful. 
This is due to the fact that the combined processes were made from various combinations of individual processes. Hence, users can combine individual processes to generate their own combined processes. 
<BR><BR>The following highlights several points the user needs to be aware of before using any of the processes. 
1. The x-axis of files generated by the residual noise and noise per epoch processes = epoch number but the units will = Time (s). Unfortunately, this cannot be changed.
2. ABR Fsp process degrees of freedom = 15. However, this can be easily amended by the user.
3. The time window of files generated by ABR Fsp process = 1.
ABR Fsp process generates a single value and if the time window is not specified Brainstorm will cause an error in opening the file. Therefore, the time window had to be hardcoded to 1. 

### Individual processes
1. **process_abr_fsp.m**
<BR>Calculates the Fsp which is the ratio between the estimated magnitude of the ABR versus the averaged background noise. It is a method to evaluate, statistically, the SNR of the averaged recording.
As the process calculates a single value for each channel, the resulting file will look like the below when opened. 
![EEG_All_CodingPractice1_AP_C60_ABR_Fsp_W_10](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/31dcd18c-9220-4dc2-bff5-27b417698baa)

2. **process_epoch_weighting.m**
<BR>Calculates epoch-specific weighting by taking the inverse of epoch variance. The weighting is then applied to each epoch.
Therefore, with an input of 100 raw epochs, the output will be 100 weighted epochs.

3. **process_noise_per_epoch.m**
<BR>Calculates the level of noise present in each channel per epoch. The noise is calculated as the standard deviation (std) of each channel per epoch.
Therefore the output will be noise (i.e. std) on the y-axis and the epoch number on the x-axis.
![EEG_All_CTRL1_L_CTRL1_L_C45P10_low_files_NEpoch_6000](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/991bbb28-9e7a-4ac5-b27b-f2b2d440c39a)
Screenshot of Brainstorm file output from running the process on left/right channel ABR epochs (Left = Green and Right = Red).
In this case, there were 6000 ABR epochs hence the x-axis goes until 6000. The amplitude shows the noise at each epoch from both channels.

4. **process_residual_noise.m**
<BR>Calculates classic and weighted versions of residual noise of each channel after a given number of epochs.
<BR>Classic residual noise = std of each channel over N epochs divided by the square root (sqrt) of the Nth epoch number.
<BR>Weighted residual noise = (std of weighted channels over N epochs divided by sqrt of the Nth epoch number)/(mean of epoch weightings).
![EEG_All_CTRL1_L_CTRL1_L_C45P10_low_files_RNoise_C_6000](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/68b8d498-8a90-4f45-bad6-4b3c25ce3b9f)
Classic residual noise (CRN) figure. Left = Green and Right = Red. 
![EEG_All_CTRL1_L_CTRL1_L_C45P10_low_files_RNoise_W_6000](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/4c2f5c1a-7122-487d-a174-6a1948fb49b2)
Weighted residual noise (WRN) figure. Left = Green and Right = Red. 
<BR>Screenshots of Brainstorm file outputs from running the process on left/right channel ABR epochs. The amplitude shows the characteristic inversely decreasing residual noise over epochs from both channels.
The difference between the figures is that the WRN is much more resilient to noise over the epochs compared to the CRN.
The residual noise curves in WRN are smooth for the entire 6000 epochs but the CRN is showing bumps indicating when there was a rise in noise (e.g. participant movement).
The changes in residual noise coincide with the noise per epoch figure shown above.

5. **process_weighted_averaging.m**
<BR>Calculates epoch-specific weighting by taking the inverse of epoch variance. The weighting is then applied to each epoch.
<BR>The final calculation of weighted average = (sum of weighted epochs)/(sum of epoch weightings).
![EEG_All_CTRL1_L_CTRL1_L_C45P10_low_files_Avg_1___timeoffset__detrend__bl_6000](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/150c14b7-689e-4076-a9a4-d0b159e544ac)
Classically averaged left ear chirp ABR (45 dBnHL with ipsilateral background noise at +10 dB SNR) from 6000 epochs using Brainstorm's averaging process. Left = Green and Right = Red. Both channels are quite noisy.
![EEG_All_CTRL1_L_CTRL1_L_C45P10_low_files_WAvg_6000](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/c3a6c2a4-2e77-4122-bc51-ecd2719ada43)
Weighted averaged left ear chirp ABR (45 dBnHL with ipsilateral background noise at +10 dB SNR) from 6000 epochs using the author's weighted averaging process. Left = Green and Right = Red.
Unlike the classically averaged ABR, the weighted ABR shows less noise (because noisy epochs were given less weighting), and a more pronounced wave V peak, indicating a higher SNR. 
![CTRL1_L_CTRL1_L_C45P10_low_files_Extract_values__all_Left_2_files_WAvg_vs_Avg](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/a78a42da-0ed0-4e97-a705-871584733ac0)
Classically (green) vs weighted averaged (red) ABR overlayed to show the significance of epoch weighting in improving SNR.

6. **process_weighted_merging_rn.m**
<BR>Merges the left and right ABR channels and produces one ABR waveform by using residual noise as the weighting factor. Refer to the third reference article for the mathematical formula. 
![CTRL1_L_CTRL1_L_C45P10_low_files_Extract_values__all_EEG_2_files](https://github.com/park-minchul/Brainstorm-Custom-Processes/assets/134780775/f0ab123d-eb39-4029-a30b-466cdf81e846)
Weighted averaged ABR (left = green and right = red) and weighted merged ABR (yellow) according to residual noise of each channel. The merged channel is weighted by residual noise.
Hence, whichever trace had the higher residual noise will get a higher weighting factor in the merging process.
In this case, the merged channel is almost exactly in between the two channels implying the residual noise between the two channels must have been quite similar.

### Combined processes
1. **process_wavg_rnoise_nepoch.m**
<BR>Combined process of individual processes 5, 4, and 3. Used mainly for CAEP analysis. 
<BR>Outputs 4 files: weighted average, classic and weighted residual noise, and noise per epoch in this order. 
2. **process_wavg_rnoise_nepoch_fsp.m**
<BR>Combined process of individual processes 1, 3 to 6. Used mainly for ABR analysis. 
<BR>Outputs 7 files: weighted average, weighted merging, classic and weighted residual noise, noise per epoch, and classic and weighted Fsp in this order. 

### Additional resources
**Relevant Brainstorm forums written by the author**
1. [Custom processing for ABR weighted averaging](https://neuroimage.usc.edu/forums/t/custom-processing-for-abr-weighted-averaging/35626)
2. [Custom process that involves merging of channels](https://neuroimage.usc.edu/forums/t/custom-process-that-involves-merging-of-channels/40638)

**References**
<br>Several articles were used for writing these processes, the below highlights the most important three. 
1. [Evaluating residual background noise in human auditory brain‐stem responses](https://pubs.aip.org/asa/jasa/article/96/5/2746/963118/Evaluating-residual-background-noise-in-human)
2. [Quality Estimation of Averaged Auditory Brainstem Responses](https://www.tandfonline.com/doi/abs/10.3109/01050398409043059)
3. [Acoustic change complex for assessing speech discrimination in normal-hearing and hearing-impaired infants](https://www.sciencedirect.com/science/article/pii/S1388245723002195?via%3Dihub)

### Cite As ###
MinChul Park (2023). Auditory evoked potentials, [GitHub](https://github.com/park-minchul/Brainstorm-Custom-Processes/tree/main/Auditory%20Evoked%20Potentials), University of Canterbury, Christchurch, New Zealand.
