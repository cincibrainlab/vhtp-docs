Preprocessing Walkthrough Introduction
======================================

Welcome to the Preprocessing walkthrough section!

Here we will present an in-depth example of how you can use the VHTP software suite's Preprocessing tool.


Initialization
--------------

To begin our process, we must choose our steps and define them in a parameters file. We will have one ‘parameters’ file for each set of processing steps we would like to use. This is flexible; you can have one parameters file that you use for all files; multiple parameters files that can be used separately for different paradigms; or multiple parameters files that represent different stages of processing for the same paradigm. 

The parameters files will be stored in the vhtp/parameters directory.    More information on the components of the parameter file can be found on the Preprocessing :doc:`DefiningParameters` page.

#. Creation of Parameters File
	* Create a new MATLAB function file.  Save this file with the name “parameters_(Name_That_Is_Meaningful_To_you).m”.  
	  
	  Example: 'parameters_Resting' vs. 'parameters_Resting_Post_ICA' vs. 'parameters_ERP'.
	
	* This function name will be what shows up as a paradigm to be selected in the graphical user interface.
	
		.. code-block::
			:caption: At the beginning of every parameters function file, edit the top row to reflect your file name. The resulting top two lines should look like this (with “Resting” replaced by the name of the file)

				function [PARAMS] = parameters_Resting()
				clear PARAMS;
			
		The output for the function will be a parameters structure that just contains all of our preprocessing steps and the inputs for those steps.
	  
		.. code-block::
			:caption: Beneath this, each step should be added as a block of code similar to the following:
				
				PARAMS.filter_highpass.function = @eeg_htpEegFilterEeglab;
				PARAMS.filter_highpass.method='highpass';
				PARAMS.filter_highpass.highpassfilt = 1;
				
		In this example, we have the function we want to run (first line) and two parameters (subsequent two lines) that tell it how we want it to perform.

		The code for each step will start with a function attribute that is just '@' followed by the name of the actual VHTP function you would like to use for that step.
		
		The parameters beneath the function will be used as inputs to the function.  In our situation here, we are telling it what type of filter we want to use ('highpass') and the frequency at which we want to filter (1 Hz).  Any possible optional input for the function can be specified in this manner, as long as it is the format of 'PARAMS.(name_you_specified).(optional_input_name)'.  
		
		We can repeat this same setup of PARAMS.x where x is a user-defined unique name consisting of a continuous string of characters.
		
		Note that if you elect to save output data for a specific step, the filenames will include the user-defined name for that step.
			
			Example: /output/Resting/eeg_htpEegFilterEeglab/subject_whatever_filter_highpass.set
		
		If you need to refer to the available functions and their inputs, please visit the :doc:`Methods` page.
		
		
	* Our final parameters file looks like this with the full set of steps:
		.. code-block::
			
				function [PARAMS] = parameters_Resting()
				clear PARAMS;

				%%FILTER HIGHPASS
				PARAMS.filter_highpass.function = @eeg_htpEegFilterEeglab;
				PARAMS.filter_highpass.method='highpass';
				PARAMS.filter_highpass.hipassfilt = 1;

				%%FILTER LOWPASS
				PARAMS.filter_lowpass.function = @eeg_htpEegFilterEeglab;
				PARAMS.filter_lowpass.method = 'lowpass';
				PARAMS.filter_lowpass.lowpassfilt=80;

				%%FILTER NOTCH
				PARAMS.filter_notch.function = @eeg_htpEegFilterEeglab;
				PARAMS.filter_notch.method = 'notch';
				PARAMS.filter_notch.notchfilt=[57 63];

				%%RESAMPLE
				PARAMS.resample.function = @eeg_htpEegResampleDataEeglab;
				PARAMS.resample.srate = 250;

				%%Rereference
				PARAMS.rereference.function = @eeg_htpEegRereferenceEeglab;

				%%CHANNEL REMOVAL 
				PARAMS.channel_removal.function=@eeg_htpEegRemoveChansEeglab;
				PARAMS.channel_removal.threshold = 3;
				PARAMS.channel_removal.saveoutput = true;

				%%CHANNEL INTERPOLATION
				PARAMS.channel_interpolation.function = @eeg_htpEegInterpolateChansEeglab;
				PARAMS.channel_interpolation.method = 'spherical';

				%%SEGMENT REMOVAL
				PARAMS.segment_removal.function = @eeg_htpEegRemoveSegmentsEeglab;
				PARAMS.segment_removal.saveoutput = true;

				%%EPOCH CREATION
				PARAMS.epoch_creation.function = @eeg_htpEegCreateEpochsEeglab;
				PARAMS.epoch_creation.epochlength = 2;
				PARAMS.epoch_creation.epochlimits=[-1 1];

				%%EPOCH REMOVAL
				PARAMS.epoch_removal.function = @eeg_htpEegRemoveEpochsEeglab;

				%%ICA
				PARAMS.ica.function = @eeg_htpEegIcaEeglab;
				PARAMS.ica.method = 'binica';

				%%REMOVE COMPONENTS
				PARAMS.component_removal.function=@eeg_htpEegRemoveCompsEeglab;
				PARAMS.component_removal.maxcomps = 24;
				end
	


#. The graphical user interface (GUI)
	* Once we have filled in our parameters file with all the steps’ functions and inputs, we are ready to move to the GUI!
	* The top right of our GUI has some settings we need to take a look at before we start processing.
	
		.. figure:: ./PreprocessImages/InitialOptions.PNG
		   :scale: 100
		   
		   The most important section is the requirements section.  You will see a label that says 'EEGLAB' next to a light.  The red light indicates that we need to add EEGLAB to our MATLAB path.  To do this we click the 'Fix Path' button and a file explorer window appears.  We select our EEGLAB directory and we are good to go!  The light should now be green.
		   
		   The other options are for file selection and preprocessing.
		   
		   We have the ability to set the GUI to single file mode.  This will allow us to select a specific data file to preprocess rather than an entire directory of data files.  The first checkbox, 'Start New,' should be checked if you plan to run **all** the steps you set in your parameters file.  Please see the :ref:`Paradigm and Workflow Style Selection` for more details.
		   
		   The second, 'Ignore Subfolders,' should be selected if your data files are at the root of your input directory and **not** housed in subfolders within the input directory.  Please see the :ref:`Selecting and Preparing Data` section for tips on selecting your input data.
			
Selecting and Preparing Data
----------------------------

#.	Setting up Directories

	* You now are ready to **use** the GUI!
	
	* •	To start the GUI, right-click the file “vhtpPreprocessGui.mlapp” and select “Run” (not “Open”) 
		- Alternatively, you can can also type 'vhtpPreprocessGui' in the MATLAB console and hit enter.
	
	* You will specify a "core" directory that houses the files you want to process.  You can perform every step you need to on these files. You are also able to retrieve the output files from each step and create an entirely new core directory with those files.
		* Within your core directory, once output is generated it will be place within a subdirectory entitled 'preprocess'.  This 'preprocess folder will hold all the output for each parameter set used.	
		* Also, if the 'use subfolders' checkbox is checked it will use any .set files it finds in subfolders that exist in this main 'core' directory.
			* Example: core directory has three folders within named 'group 1' and 'group 2' with each of these folders housing their own .set files.
	
	* Let us take a look at how we set this core directory in the GUI
	
		.. figure:: /PreprocessImages/ConfigureDatasets.png
		   :align: center
		   :scale: 90
		   
		   We can see two separate fields and two hyperlink options.
			
		   The first field is where we specify our core directory. The second field is for exporting all of our step outputs.  This is only used if you plan to export your data through the GUI by clicking the 'Export Data' button.  It will zip up the 'preprocess' output folder and store it in the folder you select for this export directory field.
			   
		   If we are in single file mode, the first field will have us selecting a specific data file rather than an entire core directory of files.
			   
		   In single file mode, 'Select Folder' will change to 'Select Files' and you must select a single data file.
			   
		   Once we have values in the two fields, we can click the 'Save Snapshot' link to save these settings.  Upon clicking 'Load Snapshot', the fields will be populated with any saved values.
		   
		   Once values are selected, some details about the input and output folder will be provided to you.
		   
	* Once output is generated, it will be place within a subdirectory entitled ‘preprocess .’ Within this subdirectory, the parameters file you use will have its own subdirectory created to house output. Inside of this parameters subdirectory, you will have separate folders of each step’s output.
		* For example, if you use a parameters file named Resting that calls the function eeg_htpEegFilterEeglab with a user-defined name of filter_highpass then your output will be saved in: /output/Resting/eeg_htpEegFilterEeglab/
			* The filename will be: originalfilename_filter_highpass.set 
	
	* We have set our input directory and export directory and now can select our preprocessing method!
	
Paradigm and Workflow Style Selection
-------------------------------------

#. Selecting Paradigm
	* Once we select an input folder/file and an export folder, we will be able to select a paradigm to load the parameters for

	.. figure:: PreprocessImages/SelectParadigms.png
		:scale: 90
		:align: center
		  
		Every parameters file will appear in this list as a paradigm you can use.  As you can see, our list consists of our example 'Resting' paradigm since we created our parameters file.
		
		If you make changes to parameters files, you can click “Reload” to ensure the most recent changes are reflected.
		
#. Select Workflow Style
	* The “Workflow Style” choices allows flexibility to how you use the steps. You will likely begin by using “All Steps (in Order).” Please see below for a breakdown of each type of preprocessing option.

		.. figure:: PreprocessImages/SelectWorkflow.png
		  :scale: 90
		  :align: center
	
	* All Steps
		- The 'All Steps' option will perform every step listed in your parameters file in sequential order. 
		- For each step, the function you sue will have a 'saveoutput' parameter that you must specify to **true** if you want that step's output to be saved.
		- If the input is a directory of files, the first file will undergo all steps and then it will move on to the next file until all files have completed all steps.

	* Selected Step
		- The 'Selected Step' option will perform the step you select from the list of available steps.  The steps listed are generated from those you specify in your parameters file. As you can see, all of our steps from our parameters file we set up back in the :ref:`initialization` section are present!
		
			.. figure:: PreprocessImages/SelectStep.png
				:scale: 90
				
				Click ‘Edit Parameters’ to make changes to the parameters file, such as adding/removing a step and/or changing parameter values.
				
				If we do make changes to the parameters file, we must save the file, and click the 'Reload' hyperlink by the paradigm list to ensure our changes are applied.
		
		- The step details panel will show the parameters and function used for the selected step in the selected step list.
			
			.. figure:: PreprocessImages/StepDetails.png
				:scale: 90
				:align: center
				
				The 'Edit function' hyperlink will open up the specific function file so you can edit it should you need to make changes.

	* Continue from Step
		- The 'Continue From Step' option will perform the selected step and all subsuquent steps listed in your parameters file.  Therefore, if you selected the fourth step, steps 4 through the last step will be performed.
		
			.. figure:: PreprocessImages/SelectContinuation.png
				:scale: 90
				:align: center
				
				The Continue From Step list will be a dropdown populated with all of the steps specified in your parameters file.  Any of them will be available to start from for preprocessing your data starting at a specific step.
		
Processing Execution
--------------------

#. Execute Workflow
	* After you have selected the data, the paradigm, and the workflow style, you can commence with preprocessing by clicking the 'Execute' button.

		.. figure:: PreprocessImages/Execute.png
		  :scale: 90
		  :align: center
		  
		  If you would like to test your steps without any output being saved, check the 'Dry Run' checkbox.  Leave this unchecked to ensure output is saved for every step that has specified 'saveoutput' as **true**.

	* Here is a look at our GUI after completing the setup
	
		.. figure:: PreprocessImages/FinalProduct.png
		   :scale: 90
		   :align: center
		   
Exporting Data
--------------

#. Export Data
	* Now that we have executed our preprocessing for our data files, we can export them!
	
	* The output will all be housed in a main new subfolder created in our input directory entitled 'preprocess'.  
	
	* Within this, each paradigm we use will have its own subfolder. Right now, we just see our 'Resting' folder.
	
		.. figure:: PreprocessImages/ParadigmOutput.png
		   :scale: 90
		   :align: center
		   
	* Within a paradigm subfolder, we will find our actual output.  Each subfolder, as seen below, is named after the step's function.  Within are the .set files for that step, if the parameter saveoutput was set to ‘true.’ .
	
		.. figure:: PreprocessImages/StepOutput.png
		   :scale: 90
		   :align: center
	
	* The folder that we selected for the output directory (see :ref:`Selecting and Preparing Data`) will be where our exported files will end up.  All the files and output for our paradigm will be exported into a .zip file with the date appended to the file name.

		.. figure:: PreprocessImages/Export.png
		   :scale: 90
		   :align: center
		   
		   The 'Export Dataset' button will zip the output data and place it in the output folder.
		   
		   The BIDS export will include the input file/files as the source files, and all output will be copied to the derivatives subdirectory and placed under a subdirectory named after the selected paradigm.
		   
Examining Step Output
---------------------

#. Inspecting file outputs
	* We can see in the :ref:`Exporting Data` section that each function used will have its own subdirectory that houses the data.  If we go into each function subdirectory, we can see our output files with their custom labels.
		
		.. figure:: PreprocessImages/FunctionOutputFiles.png
		   :scale: 90
		   :align: center
		   
		   As we can see, the 'eeg_htpEegFilterEeglab' subfolder has all output files that underwent this function.  Each file has the original file name, followed by the user-defined name for the preprocessing step listed in the parameters file.
		   
    
#. Inspecting output structure contents
	* We can then load a file via EEGLAB to inspect the file's processing history.
   
	* This information is stored in EEG.vhtp. You can view these details by using MATLAB’s variable view or by typing EEG.vhtp in the console, as shown below.
	
		.. figure:: PreprocessImages/vhtpStructOutput.png
		   :scale: 90
		   :align: center
		   
		   You can see multiple fields within the EEG.vhtp structure. 
		   
		   The inforow structure contains information about the data file itself, including fields like net type, raw trials, raw sampling rate, raw events, raw event codes, and others.
		   
		   The stepPreprocessing structure contains all the steps from the parameters file and a 1 or 0 value to indicate if the file has undergone that step.
		   
		   The 'eeg_htpEegFilterEeglab' field is a structure that contains details about how this function was used to alter the data. Though you may run multiple filters, all use the same function, so all filtering parameters will be housed in the same structure.
		   
		   The 'prior_file' field lists the name of the data file prior to this step. In this case, we have 0013_rest_notch.set open. We highpass and lowpass filtered our data prior to notch, so the file that entered the notch filter was 0013_rest_filter_lowpass.set. 
		   
		   The 'stepPlacement' field is an integer providing context about what step this was in the process. In this case, notch filtering was our third step listed in the parameters file.
		   
	* The stepPreprocessing structure has the following format:
	
		.. figure:: PreprocessImages/stepPreprocessingStructOutput.png
		   :scale: 90
		   :align: center
		   
		   As we can see, since this output is for the third step and we were in 'All Steps' mode the first three steps have a value of 1 followed by all zeros.
		   
   * The eeg_htpEegFilterEeglab struct has the following format:
   
		.. figure:: PreprocessImages/functionStructOutput.png
		   :scale: 90
		   :align: center
		   
		   The fields in this structure represent the input parameters for the eeg_htpEegFilterEeglab function along with some completion details for the various filtering methods. There is also a qi_table field that provides when the file has been through the function and at what time.