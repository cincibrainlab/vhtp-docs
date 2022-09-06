Defining Parameters
^^^^^^^^^^^^^^^^^^^

The key component of the VHTP Preprocessing tool is its flexibility!

We allow users to customize their own pipeline methods and parameters.

In the VHTP base directory, there is a subdirectory labeled 'parameters'.  Within this folder is where you can create a matlab function file that will dictate what steps are taken during preprocessing and each step's parameters.

.. code-block::
	:caption: The following is an example of the setup of a typical parameters file:
	
	function [PARAMS] = parameters_InfantTest()
		clear PARAMS;
		%FILTER HIGHPASS
		PARAMS.filter_highpass.function = @eeg_htpEegFilterEeglab;
		PARAMS.filter_highpass.method='highpass';
		PARAMS.filter_highpass.hipassfilt = 0.3;

		%%FILTER NOTCH
		PARAMS.filter_notch.function = @eeg_htpEegFilterEeglab;
		PARAMS.filter_notch.method = 'notch';
		PARAMS.filter_notch.notchfilt=[57 63];
		% PARAMS.filter_notch.saveoutput = true;

		%%FILTER LOWPASS
		PARAMS.filter_lowpass.function = @eeg_htpEegFilterEeglab;
		PARAMS.filter_lowpass.method = 'lowpass';
		PARAMS.filter_lowpass.lowpassfilt=100;
		%PARAMS.filter_lowpass.saveoutput = true;

		% %% WAVELET DENOISING
		 PARAMS.waveletdenoise.function = @eeg_htpEegWaveletDenoiseHappe;
		 PARAMS.waveletdenoise.isErp = true;
		 PARAMS.waveletdenoise.filtOn = true;
		 PARAMS.waveletdenoise.saveoutput = true;

		%%CHANNEL REMOVAL 
		PARAMS.channel_removal.function=@eeg_htpEegRemoveChansEeglab;
		PARAMS.channel_removal.threshold = 3;
		PARAMS.channel_removal.removechannel = true;
		PARAMS.channel_removal.saveoutput = true;
		PARAMS.channel_removal.minimumduration = 0;
		PARAMS.channel_removal.type = 'Event';
		PARAMS.channel_removal.automark = true;
	end


We start with a base structure entitled 'PARAMS'. We then add levels of fields within the PARAMS structure. 

As you can see each member of the overall PARAMS structure has a similar format. The PARAMS structure will consist of first-level and second-level fields.

Field Levels and Their Setup
""""""""""""""""""""""""""""
#. First-level fields
	* Each first-level field of the overall PARAMS structure (filter_highpass, filter_notch, filter_lowpass, waveletdenoise, and channel_removal in our example above) are named similar to their respective preprocessing method.  These first-level fields also need to be one continuous string whether it is a single word or two words connected via an underscore character. 

#. Second-Level Fields
	* The second-level fields will vary for the most part between different processing steps.  Each step will need to have a second-level 'function' field as this indicates the function used for that step.  Aside from that the other second-level fields will differ due to them representing that function's parameters.  As we can see in the example above, the 'Channel Removal' and 'Wavelet Denoising'.

		#. Function
			* The 'function' second-level field indicates the function that will be called for this particular step of processing.  

			* Each available method can be found in the Preprocessing :doc:`Methods` page.

		#. Function Parameters
			* The second-level fields that are not the function itself will be the function's inputs.  Each of these fields must match the parameter's name exactly and have a value that matches the type that the function input requires (numeric input has a numeric value and so on). 

			* Each of the inputs for every function can be found on the Preprocessing :doc:`Methods` page.