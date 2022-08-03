Preprocessing Walkthrough Introduction
======================================

Welcome to the Preprocessing walkthrough section!

Here we will outline an in-depth walkthrough example for how to go about using the VHTP software suite's Preprocessing tool.


Initialization
--------------

To begin our process, we will begin with picking our steps and defining them in a parameters file.  

The 'parameters' subdirectory within the core VHTP project directory is where our main parameters file will be placed.  This file consists of the steps we would like to perform on our data and how each step should be executed.  More information on the components of the parameter file can be found on the Preprocessing :doc:`DefiningParameters` page.

#. Creation of Parameters File
	* Create a new MATLAB function file.  Name this file in the following format 'parameters_x' where x indicates which project/paradigm this master parameters file will be used for.  This can be a project name specifically or a modification on a project's preprocessing with different steps taken.  
	  
	  Example: 'parameters_Resting' vs. 'parameters_Resting_No_ICA'.
	
	* This function name will be what shows up as a paradigm to be selected in the graphical user interface.  This can be seen in the blank section
	
		.. code-block::
			:caption: This should be the beginning of every parameters function file (with the 'Resting' replaced with your preferred project/paradigm name):

				function [PARAMS] = parameters_Resting()
				clear PARAMS;
			
		The output for the function will be a parameters structure that just contains all of our preprocessing steps and the inputs for those steps.
	  
		.. code-block::
			:caption: Each step can be broken up into a block of code like the following:
				
				PARAMS.filter_highpass.function = @eeg_htpEegFilterEeglab;
				PARAMS.filter_highpass.method='highpass';
				PARAMS.filter_highpass.hipassfilt = 1;
				
		In this example, we have a function defined and two parameters.  Each step will require a function attribute that is just '@' followed by the name of the actual VHTP function you would like to use for that step.
		
		As for the parameters themselves, these are the optional inputs for the function you specified.  In our situation here, we are marking a method of filtering ('highpass') and the frequency at which we want to filter (1 Hz).  Any possible optional input for the function can be specified in this manner, as long as it is under the same name of the larger PARAMS structure (PARAMS.filter_highpass in our case).  
		
		We can repeat this same setup of PARAMS.x where x is the name for the step we are performing (can be named anything but must be one, continuous string of characters.  Recommended to use name that mirrors the actual step) for every single preprocessing step we need in our pipeline.
		
		If you need to refer to the available functions and their inputs, please visit the :doc:`Methods` page.
		
	* Once we have filled in our parameters file with all the steps and their functions and inputs, we are ready to move on to the graphical user interface!
		
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
	


#. Initial Options
	* The top right of our GUI has some settings we need to take a look at before we start processing.
	
		.. figure:: PreprocessImages/InitialOptions.png
		   :scale: 100
		   
		   The most important section is the requirements section.  You will see a label that says 'EEGLAB' and next to a light.  The red light indicates that we need to add EEGLAB to our MATLAB path.  To do this we just click the 'Fix Path' button and a file explorer window appears.  We select our EEGLAB directory and we are good to go!  The light should now show green.
		   
		   The other options are for file selection and preprocessing.
		   
		   We have the ability to set the GUI to single file mode.  This will allow us to select a specific data file to preprocess rather than an entire directory of data files.  The first checkbox, 'Start New', should be checked if you plan on running all the steps you set in your parameters file.  Please see the :ref:`Paradigm and Workflow Style Selection` for more details.
		   
		   The second, 'Ignore Subfolders', should be selected if your data files are at the root of your input directory and not housed in any subfolders within the input directory.  Please see the :ref:`Selecting and Preparing Data` section for selecting your input data.
			
Selecting and Preparing Data
----------------------------

#.	Setting up Directories
	* You now are ready to use the Graphical User Interface (GUI)!
	
	* We will right-click and select 'run' on the following file to start the GUI: vhtpPreprocessGui.mlapp
		- We can also type 'vhtpPreprocessGui' in the command console and hit enter to start the GUI.

	* The main setup for the Preprocessing file structure will consist of a core directory that will house the files you want to operate on.  You can perform every step you need to on these files. You are also able to retrieve the output files from each step and create an entirely new core directory with those files.
		* Within your core directory, once output is generated it will be place within a subdirectory entitled 'preprocess'.  Within this subdirectory, the parameters file you use will have its own subdirectory created to house output.  Inside of this parameters subdirectory, you will have separate folders of each step's output.
		
	* Let us take a look at how we set this core directory in the GUI
	
		.. figure:: PreprocessImages/ConfigureDatasets.png
		   :align: center
		   :scale: 90
		   
		   We can see two separate fields and two hyperlink options.
			
		   The first field is where we can specify our core directory. The second field is for exporting all of our step outputs.  This is only required if you plan on exporting your data through the GUI.
		       
		   We just need to click the 'Select Folder' button for the first, or also second, field and a window will pop open for us to select a folder.  We will see text indicating how many files and subdirectories are in our folder selected for the first field.
			   
		   If we are in single file mode, the first field will have us selecting a specific data file rather than an entire core directory of files.
			   
		   In single file mode, we just need to click the 'Select Files' button and a window will pop open for us to select a data file.
			   
		   Once we have values in the two fields, we can click the 'Save Snapshot' link to save these settings.  Upon clicking 'Load Snapshot', the fields will be populated with any saved values.
		   
		   Once values are selected, some details about the input and output folder will be provided to you.
		   
	* We have set our input directory and export directory and now can select our preprocessing method!
	
Paradigm and Workflow Style Selection
-------------------------------------

#. Selecting Paradigm
	* Once we select an input folder/file and an export folder, we will be able to select a paradigm to load the parameters for

	.. figure:: PreprocessImages/SelectParadigms.png
		:scale: 90
		:align: center
		  
		Any paradigm that has an existing corresponding parameters file will appear for selection.  As you can see, our list consists of our example 'Resting' paradigm since we created our parameters file.
		
		The 'Reload' hyperlink can be clicked to ensure that the selected paradigm's parameters have been loaded.
		
#. Select Worfklow Style
	* The workflow style is the main component of how your files will be preprocessed.  There are three options: All Steps (in Order), Selected Step, and Continue from Step.  Please see below for a breakdown of each type of preprocessing option.

		.. figure:: PreprocessImages/SelectWorkflow.png
		  :scale: 90
		  :align: center
	
	* All Steps
		- The 'All Steps' option will perform each and every step listed in your parameters file in sequential order.  
		- If the input is a directory of files, the first file will undergo all steps and then it will move on to the next file until all files are completed with the preprocessing.
		- If in 'Single File Mode',the sole file will undergo all steps and then the preprocessing will be complete.
			- In 'Single File Mode', 

	* Selected Step
		- The 'Selected Step' option will perform the step you select from the list of available steps.  The steps listed are generated from those you specify in your parameters file. As you can see, all of our steps from our parameters file we set up back in the :ref:`initialization` section are present!
		
			.. figure:: PreprocessImages/SelectStep.png
				:scale: 90
				
				If we would like to edit our parameters file to add or remove a step and/or change parameter values, we can click the 'Edit Parameters' hyperlink and our file will be opened for us to make changes.
				
				If we do make changes and edit the parameters file, we will just want to make sure we click the 'Reload' hyperlink back by the paradigm list to ensure our changes are applied!
		
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
		
Execution
---------

#. Execute Workflow
	* All of our data has been selected, our paradigm selected, and also our workflow style selected.  We can now commence with the preprocessing by clicking the 'Execute' button.

		.. figure:: PreprocessImages/Execute.png
		  :scale: 90
		  :align: center
		  
		  Our preprocessing can be performed by clicking 'Execute'. 
		  
		  However, if we would like to run our workflow without any output being saved and check that our expected steps are performed, we can check the 'Dry Run' checkbox.  Leave this unchecked to ensure output is saved.

	* Here is a look at our GUI after completing the setup
	
		.. figure:: PreprocessImages/FinalProduct.png
		   :scale: 90
		   :align: center
		   
Exporting Data
--------------

#. Export Data
	* Now that we have executed our preprocessing for our data files, we can export them!
	
	* The output will all be housed in a main new subfolder created in our input directory entitled 'preprocess'.  
	
	* We first have a subfolder with our paradigm's name, each paradigm that is selected and preprocess with will have its own folder.  Right now, we just see our 'Resting' folder.
	
		.. figure:: PreprocessImages/ParadigmOutput.png
		   :scale: 90
		   :align: center
		   
	* Within a paradigm subfolder, we will find our actual output.  Each subfolder, as seen below, is named after the step's function.  Within are the final .set files.
	
		.. figure:: PreprocessImages/StepOutput.png
		   :scale: 90
		   :align: center
	
	* The folder that we selected for the output directory (see :ref:`Selecting and Preparing Data`) will be where our exported files will end up.  All the files and output for our paradigm will be exported into a .zip file with the date appended to the file name.

		.. figure:: PreprocessImages/Export.png
		   :scale: 90
		   :align: center
		   
		   The 'Export Dataset' button is the regular zip export of data whereas we have a second, specialized option to export your data into a BIDS format.
		   
		   The BIDS export will package the file/files used as input as the source files and all output will be copied to the derivatives subdirectory and placed under a subdirectory named after the selected paradigm.
		   
Examining Step Output
---------------------

#. Inspecting file outputs
	* We can see in the :ref:`Exporting Data` section that each function used will have its own subdirectory that houses the data.  If we go into each function subdirectory, we can see our output files with their custom labels.
		
		.. figure:: PreprocessImages/FunctionOutputFiles.png
		   :scale: 90
		   :align: center
		   
		   As we can see, the 'eeg_htpEegFilterEeglab' subfolder has all output files that underwent this function.  Each file has the original subject id, first 4 digits + 'rest', followed by the preprocessing step listed in the parameters file.
		   
    
#. Inspecting output structure contents
	* We can then load a file (our Notch step output for example, third step in parameters file) in EEGLAB and inspect the even further detail we provide you for managing output's details and history.
   
	* We have a multi-level structure, 'vhtp', that stores the information regarding preprocessing.  This will store all function-related details for every step taken up to the current output file. We can see its format below.
	
		.. figure:: PreprocessImages/vhtpStructOutput.png
		   :scale: 90
		   :align: center
		   
		   In the command console output, we see multiple fields in the overall 'vhtp' structure.
		   
		   The inforow structure contains information about the data file itself ranging from net type, raw trials, raw sampling rate, raw events, raw event codes, and so on.
		   
		   The stepPreprocessing structure contains all the steps from the parameters file and a 1 or 0 value to indicate if the file has undergone that step.
		   
		   The 'eeg_htpEegFilterEeglab' field is a structure that contains details about the function used and all inputs we specified back in the parameters file.  We only have one function field due to the multiple filtering steps all using the same function as set in our parameters file (:ref:`Initialization`)
		   
		   The 'prior_file' field list the input data file to the function.  Since we are looking at the notch output, we had inputted a data file that had undergone both highpass and lowpass filtering (steps 1 and 2, respectively).
		   
		   The 'stepPlacement' field is an integer indicating what number this data file's step was in the parameters file.
		   
	* The stepPreprocessing structure has the following format:
	
		.. figure:: PreprocessImages/stepPreprocessingStructOutput.png
		   :scale: 90
		   :align: center
		   
		   As we can see, since this output is for the third step and we were in 'All Steps' mode the first three steps have a value of 1 followed by all zeros.
		   
   * The eeg_htpEegFilterEeglab struct has the following format:
   
		.. figure:: PreprocessImages/functionStructOutput.png
		   :scale: 90
		   :align: center
		   
		   The fields in this structure represent the input parameters for the eeg_htpEegFilterEeglab function along with some completion details for the various filtering methods and a qi_table field that provides which files have been through the function and at what time.