# Technical notes

## ECG/EMG hardware

The team report documents an AD620 preamplifier, notch filter, band-pass filter, post-amplifier, and level shifter, followed by Arduino acquisition and HM-10 BLE transmission. Reported notch attenuation is at the measured center (59.5 Hz), not a separately verified attenuation at exactly 60 Hz. The report calculates an ideal notch center of approximately 55.8 Hz from the listed components.

The measured ECG and EMG passbands are approximately 0.9–98.6 Hz and 8.5–420 Hz. At 500 Hz sampling, the 250 Hz Nyquist limit is below the EMG upper cutoff. Aliasing and eight-bit transmission limit fidelity. Waveform resemblance to BIOPAC is qualitative and does not establish clinical equivalence.

## EEG spectral analysis

Experiment 2 used 200 Hz BIOPAC data. Analysis compares 2–18 s (eyes closed) with 22–38 s (eyes open). The nominal analysis bands were delta 1–5, theta 4–8, alpha 8–13, and beta 13–30 Hz; delta and theta definitions therefore overlap. Welch PSD and Butterworth filtering were used. The report describes forward–backward filtering; detailed filter implementation should be checked against the original MATLAB source before making an exact effective-order claim.

Figures are extracted from Experiment 2: notch circuit/measurement (p. 3), PSD and MATLAB/BIOPAC filter comparison (p. 21, Figures 6–7), and integrated band powers (p. 22, Figure 8). No raw time-series files are included. Artifact effects were not separately removed or quantified.

## Classification protocol

Experiment 3 used Fp1 BrainLink data at 512 Hz: 3 participants, 30 recordings each, and Relax/Focus/Stroop tasks. Twelve-second windows stepped by two seconds yielded 449 segments. All windows from a participant stay in the same LOSO fold; overlapping windows are correlated and are not 449 independent participants or trials.

Features are sums of Welch PSD bins in 13 bands, followed by log10. Because frequency-bin width is fixed, these are proportional spectral-power features; the implementation does not explicitly multiply each sum by the frequency-bin width. Training-only standardization precedes classification.

The 79.3% mean is across three held-out participants, with population SD 15.4 percentage points as computed in the supplied program. This is not a confidence interval. All four compared models use the same batch post-processing: balancing predicted probabilities toward equal class mass, then pseudo-label self-training with shrinkage LDA. It uses unlabeled test features, assumes approximately balanced tasks, and is an offline transductive protocol.

The report gives ExtraTrees results of 60.6% without post-processing, 73.7% with balancing, and 79.3% with balancing plus self-training. These are report-derived ablations, not independently rerun measurements. Hyperparameters and feature choices were compared on the same LOSO dataset; no untouched external test cohort was supplied. Eye/muscle activity remains in the selected bands, limiting interpretation as purely neural mental-state decoding.

## Availability

The private source archive contains the two original reports, classification implementation and environment files, saved aggregate outputs, and 90 participant recordings. It does not include standalone MATLAB, Arduino, Flutter, or circuit CAD source, nor the additional session-validation data and script described in the report. No final-term BCI game is included in this portfolio.
