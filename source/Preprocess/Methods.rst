Available Steps
===============

Here you will find the available methods and their respective parameters and usage for the EEGLAB module of preprocessing.

.. module:: EEGLABMethods

Artifact
--------

Artifact Subspace Reconstruction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegAsrCleanEeglab

Component Removal
^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegRemoveCompsEeglab

Segment Removal
^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegRemoveSegmentsEeglab

Wavelet thresholding artifact removal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegWaveletDenoiseHappe

Channel
-------

Channel Interpolation
^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegInterpolateChansEeglab

Channel Removal
^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegRemoveChansEeglab

Rereference
^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegRereferenceEeglab

Component
---------

Independent Component Analysis
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegIcaEeglab

Epoching
--------

Epoch Creation
^^^^^^^^^^^^^^

.. collapse:: eeg_htpEegCreateEpochsEeglab

    .. mat:autofunction:: eeg_htpEegCreateEpochsEeglab

.. collapse:: eeg_htpEegCreateEpochsHabEeglab   

    .. mat:autofunction:: eeg_htpEegCreateEpochsHabEeglab

.. collapse:: eeg_htpEegCreateErpEpochsEeglab   

    .. mat:autofunction:: eeg_htpEegCreateErpEpochsEeglab

.. collapse:: eeg_htpEegEpoch2Cont   

    .. mat:autofunction:: eeg_htpEegEpoch2Cont

Epoch Removal
^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegRemoveEpochsEeglab

Filter
------

Bandpass Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegBandpassFilterEeglab

Cleanline Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegCleanlineFilterEeglab

Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegFilterEeglab

Filter EEG using FastFC
^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegFilterFastFc

Frequency Interpolation EEG 
^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegFrequencyInterpolation

High Pass Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegHighpassFilterEeglab

Low Pass Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegLowpassFilterEeglab

Notch Filter EEG using EEGLAB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegNotchFilterEeglab

Resample
--------

Resampling
^^^^^^^^^^^^^^^
.. mat:autofunction:: eeg_htpEegResampleDataEeglab



