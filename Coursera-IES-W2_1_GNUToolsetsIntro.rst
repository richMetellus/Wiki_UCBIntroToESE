
##############################################################################################
`Week 2 Vid 1: Introduction to Build Systems using GNU Toolsets <shorturl.at/oEJKN>`_
##############################################################################################



..
    slide
************
Slides
************

.. .. raw:: html
   
..    <!DOCTYPE html>
..    <html>
..      <head>
..         <style>
..             .myButton {
..             background-color: white;
..             color: black;
..             border: 2px solid #555555;
..             }

..             .myButton:hover {
..             background-color: #555555;
..             color: white;
..             }
..         </style>
..        <title>Title of the document</title>
..      </head>
..      <body>
..        <b>Slides in IFrame</b>
..        <br></br>
..        <center>
..        <iframe src="https://ndusbpos-my.sharepoint.com/personal/richelin_metellus_ndus_edu/_layouts/15/Doc.aspx?sourcedoc={6dc9f64e-6332-45c7-afbc-ad5cb8ac3422}&amp;action=embedview&amp;wdAr=1.7777777777777777" 
..         width="730px" 
..         height="480px" 
..         frameborder="0">This is an embedded 
..         <a target="_blank" href="https://office.com">Microsoft Office</a> presentation, powered by 
..         <a target="_blank" href="https://office.com/webapps">Office</a>.
..         </iframe>
..         </center>
        
..      </body>
..    </html>

****************************
**Transcribed Note**
****************************

The build System
=======================

|EmbeddedSystemDevPlatform|\

**The build system**

-  is just a fancy way to describe the environment and the tools needed
   to compile a software project.


The tools that will be using are provided by:

-  **GNU's Compiler Collection** or **GCC** for short. 

-  GNU bundles many useful compilation tools together, and we'll
   often refer to this as our tool chain. 

-  GCC supports a wide array of architectures including ARM.

   -  Allows great flexibility

-  GCC compiler collection include compiler, assembler, linker, etc..


Software Tools 
================

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 6

The list of software tools

#. an Integrated Development Environment (IDE) or an editor (VS Code)
#. Version Control Software like ``Git``
#. Compiler toolchain (GCC)
   
   - We use the compiler on a Linux host VM

      -  To Translate high level languages into architecture specific
         languages
      - |TranslationOfPrograms|
   
            
   - General applications that form the build toolchain (GCC): From source to Executable process 
     
      #. The Preprocessor (For Preprocessing steps)
      #. Compiler (For Compiling steps)
      #. The Assembler (Assembling)
      #. The Linker (Linking steps)
      #. Locator

   - |CompilerToolsetsSteps|

#. Executable Loader

   - Installation of the executable on a target device usually require  other tools.




Build Process and Its Stages
================================

.. 
   Path of drawio on Windows: ``C:\Program Files\draw.io``. This is required for the extension

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg


The Build process usually consist of the following stages:

#. Preprocessing 
#. Compiling 
#. Assembling
#. Linking
#. Locating
#. Using Executable Loader to install the software on target device

The first five steps are what build the software application. 

The preprocessor
-----------------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 1

The preprocessor

1. will take your source files `*.c` and `*.h` and transform them into new files by
   evaluating preprocessor directives and performing macro substitution.


The Compiler Proper
----------------------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 2


2. These modified files, the preprocessed files, and fed them to the compiler proper.
   
   -  This performs a C programming to assembly code translation.

The Assembler
---------------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 3

Next, the assembler

3. converts our assembly code into object code.

   -  :term:`Object code <object code>` looks like confusing binary data and it's not really
      human readable.

      * The :term:`object file` contains the architecture specific machine code. 
      * It cannot be executed until it is linked to a program file that 
        contains a main function

   -  This assembly to object code conversion gets repeated from many
      source files.

The Linker
------------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 4

:term:`Object files <object file>` need to be combined into a single executable where all
references between :term:`object files` need to be resolved.

-  We call these references symbols.
-  Now, this job is performed by the linker.



The linker

4. The linker provides the linked file to a relocator/locator.


The Locator
-----------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 5

The locator

5. The locator takes the linked file that was provided by the linker and then map all the addresses 
   of code and data into the processor's memory space.

The final file should be your target executable

-  and all that needs to happen is to be installed on your target
   system.
- The executable only work on the architecture you compiled it for.

GNU Toolchain
====================


GNU (stands for GNU's not Unix) is an extensible collection of free software, which can be used as 
an operating system or can be used in parts with other operating systems. (Wikipedia)

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 6


2 sets of tools provided by the GNU toolchain we will need:

#. The compiler toolchain, GCC = GNU's Compiler Collection

   - contains many tools (compiler, assembler, linker, etc...)

#. GNU Make.



GCC: Compilation
-------------------

**Compilation (No Linking)**

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 7


Native Compilation
^^^^^^^^^^^^^^^^^^^^^^^

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 8

Native compilation is

-  When you compile your build for the same system you intend to run the
   executable on. No extra hardware needed.

Typically, we do not natively compile embedded projects

-  as we use a host machine to compile our microcontroller software
   builds


Cross Compilation 
^^^^^^^^^^^^^^^^^^^^^^^

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg
   :page-index: 9

Cross Compilation

-  is when you compile an executable on one system and it is intended to
   run on another

-  Compiling a software on a host machine to be run on target.

-  Usually needed as microcontroller don't have an OS or resources for programs like GCC to
   build the code itself.

   -  With the exception of Cortex-ARM processor set which provide that
      ability to compile natively

we may have an on board debugger that can help us with installing.
This is not change how the Cross Compiler operate.
   
   - This varies depends on the development platform or vendor specific board.

|

**++** Building large software projects can be a tedious process

* Sometimes you design your own libraries for different objectives
* Integrate other open source software into your project,
* Set up your build environment,
* Managing the compilation of many files.

 Another useful tool in the GNU Toolchain called ``Make`` can assist with this cumbersome process.


GNU ``Make``
-------------

Make

-  is another tool in the GNU Toolchain that controls the generation of executables and other
   non-source files of a program from the program's source files.

-  Make will help us manage this whole process of building our
   executable.



.. |EmbeddedSystemDevPlatform| image:: ../../_images/W2_1_1_EmbeddedSystemDevPlatform.png
   :width: 6.01458in
   :height: 3.31181in

.. |SoftwareTools| image:: ../../_images/W2_1_SoftwareTools.PNG
   :width: 6.01458in
   :height: 3.31181in

.. |TranslationOfPrograms| image:: ../../_images/W2_1_TranslationOfPrograms.PNG
   :width: 6.01458in
   :height: 3.31181in

.. |CompilerToolsetsSteps| image:: ../../_images/W2_1_CompilerToolsetsSteps.PNG

.. |BuildProcessStages| image:: ../../_images/W2_1_BuildProcessStages.PNG
   :width: 5.60in
   :height: 3.20in
