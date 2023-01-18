Available Steps
=====================

Here you will find the available methods and their respective parameters and usage for the EEGLAB module of preprocessing.

.. module:: EEGLABMethods

Artifact Subspace Reconstruction
--------------------------------
.. mat:autofunction:: eeg_htpEegAsrCleanEeglab

QA EEG1 vs. EEG2 Comparison
---------------------------
.. mat:autofunction:: eeg_htpEegAssessPipelineHAPPE

Channel Interpolation
---------------------
.. mat:autofunction:: eeg_htpEegInterpolateChansEeglab

Channel Removal
---------------
.. mat:autofunction:: eeg_htpEegRemoveChansEeglab

Component Removal
-----------------
.. mat:autofunction:: eeg_htpEegRemoveCompsEeglab

Wavelet thresholding artifact removal
-------------------------------------
.. mat:autofunction:: eeg_htpEegWaveletDenoiseHappe

Epoch Creation
--------------

.. collapse:: eeg_htpEegCreateEpochsEeglab

    .. mat:autofunction:: eeg_htpEegCreateEpochsEeglab

.. collapse:: eeg_htpEegCreateEpochsHabEeglab   

    .. mat:autofunction:: eeg_htpEegCreateEpochsHabEeglab

.. collapse:: eeg_htpEegCreateErpEpochsEeglab   

    .. mat:autofunction:: eeg_htpEegCreateErpEpochsEeglab

Epoch Removal
-------------
.. mat:autofunction:: eeg_htpEegRemoveEpochsEeglab

Filtering
---------
.. mat:autofunction:: eeg_htpEegHighpassFilterEeglab

.. mat:autofunction:: eeg_htpEegLowpassFilterEeglab

.. mat:autofunction:: eeg_htpEegNotchFilterEeglab

.. mat:autofunction:: eeg_htpEegFilterEeglab

.. mat:autofunction:: eeg_htpEegFilterFastFc

.. mat:autofunction:: eeg_htpEegBandpassFilterEeglab

.. mat:autofunction:: eeg_htpEegCleanlineFilterEeglab

Independent Component Analysis
------------------------------
.. mat:autofunction:: eeg_htpEegIcaEeglab

Segment Removal
---------------
.. mat:autofunction:: eeg_htpEegRemoveSegmentsEeglab

Rereference
-----------
.. mat:autofunction:: eeg_htpEegRereferenceEeglab

Resampling
---------------
.. mat:autofunction:: eeg_htpEegResampleDataEeglab

Simulate EEG signal
-------------------
.. mat:autofunction:: eeg_htpEegSimulateEeg