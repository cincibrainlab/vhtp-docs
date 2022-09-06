Preprocessing
^^^^^^^^^^^^^

Welcome to the Preprocessing Section for the VHTP Pipeline.
===========================================================

This section is an introduction and reference for walkthroughs and the methods of the Preprocessing tool for the VHTP software suite.

The Preprocessing tool is utilized to perform user-defined steps for cleaning raw EEG data. This tool is a reworking of our previous pipeline with focus on flexibility and ease of use for processing and output. Whereas the previous pipeline developed by the Cinci Brain Lab team consisted of multi-step "stages", we now leave it up to the user to create their own pipeline with available methods. If you need to add your own functions and also rework currently provided function files, this is allowed and can be easily done and ready to go at next startup of the Preprocessing Tool.

The sections listed below are split up into three sections: Methods, Parameter Definition, and Walkthrough.

The :doc:`Methods` page is a highly detailed page split up into various preprocessing steps that gives each available function for that step.  We also provide all the inputs and outputs and variable types should you wish to call these functions in your own custom script.

The :doc:`DefiningParameters` page gives details about our custom parameter setup that allows the flexibility and plug and play nature of our Preprocessing tool.

The :doc:`Walkthrough` page is a basic walkthrough that explains the various components of the Preprocessing tool required setud and usage of its Graphical User Interface (GUI).



.. toctree::
   :caption: Sections:
   
   Methods
   DefiningParameters
   Walkthrough