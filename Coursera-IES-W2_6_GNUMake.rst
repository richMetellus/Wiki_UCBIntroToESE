##############################
Build Management - GNU Make
##############################


**************
Objectives
**************


* Learn how to create a build system to simplify and improve our process of
  building a software target.

* Learn the process software team used to create a software product

*****************************************************************
Motivation for GNU Make or Using any Build Management Software
*****************************************************************

Manually Building software can be tedious:

1. Many compiling and architecture or Platform dependencies:

    1. Each compile or link command that is issued takes hundreds of characters with 
       over ten flags

        i. **Many GCC flags**
        ii. **Many independent commands**
        

    2. software companies and projects usually support multiple build targets 
       and architectures, both of which affect how we build.

        i. **Many build targets**
        ii. **Many supported architectures**
    
    * Example of building in KDS IDE
      
      .. grid:: 2

         .. grid-item-card:: KDS IDE Build Compilation output Example   

             .. raw:: html
                
                <iframe 
                  src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p6" 
                  frameborder="0" width="100%" height="500" 
                  allowfullscreen="true" 
                  mozallowfullscreen="true" 
                  webkitallowfullscreen="true">
                </iframe>     
     

         .. grid-item-card:: Many GCC Flags     

             .. raw:: html
                
                <iframe 
                  src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p7" 
                  frameborder="0" width="100%" height="500" 
                  allowfullscreen="true" 
                  mozallowfullscreen="true" 
                  webkitallowfullscreen="true">
                </iframe>
      

2. Large software project or team:

    * Many source files
        
        * Take Linux Kernel Example:

            * over 23,000 ``*.c`` files
            * ``*.h`` files: 18,000+
            * ``*.S`` files: 1,400+

Building manually can

* cause consistency issues
* waste development time.

For all these reasons, **Process of iteratively manually building file by file is not 
scalable for large teams or software project**


****************************************
Build Management Software Tools 
****************************************

Build management software or Build Automation 

* provide a simple mechanism to control and build consistent target executable 
  images for your software applications.

* provides a mean to automated the build process

  .. card:: Build Management Software (def and function)

     .. raw:: html
     
        <iframe 
            src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p8" 
            frameborder="0" width="100%" height="500" 
            allowfullscreen="true" 
            mozallowfullscreen="true" 
            webkitallowfullscreen="true">
        </iframe> 
     
GNU Make:
============

**What is GNU Make and Its Distinction from GCC?**

GNU (GNU's not Unix) Make

* is a (free) tool that controls the generation of executables and other non-source
  files of a program from the program's source files. [2]_

* is a build management software.

    * Determine what files need to compiled/recompiled in a project given a
      :term:`Makefile`
    
    * helps generate build dependencies and other files.

* can be invoked with the command ``make`` in a linux terminal
    
    * the location of make can be found via ``which make``
    * the version can be found with the command ``make -v``

* Can use ``make`` to automate all operations related to the build process:
    
    .. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
       :page-index: 2
       :align: right
       :width: 500
       :height: 350

    * Preprocessing
    * Assembling
    * Compiling
    * Linking
    * Relocating


* :term:`Make` is not GCC since :term:`Make` is a tool that is independent of the compiler or architecture
  but make is also part of the GNU Toolset
  |
  GNU (GNU's not UNIX)

  * is a collection of software, a toolset/toolchain
  * contains GCC = GNUs Compiler Collection.

  .. list-table:: GNU Toolset
     :header-rows: 1

     * - Name
       - Symbol
       - ARM Executable (ARM GNU Toolchain) or command
         
     * - Assembler
       - as
       - arm-none-eabi-as
     
     * - Compiler
       - gcc
       - arm-none-eabi-gcc

     * - Linker
       - ld
       - arm-none-eabi-ld

     * - Make
       - make
       - make


* GNU Make can be thought of as an abstraction interface to building.

  * need to provide one or more :term:`Makefiles`
  
  .. card:: Build Management Software

    .. raw:: html
    
      <iframe 
          src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p12" 
          frameborder="0" width="100%" height="500" 
          allowfullscreen="true" 
          mozallowfullscreen="true" 
          webkitallowfullscreen="true">
      </iframe> 


* can use GNU Make to do more than just compile but also generate software 
  dependencies, statistical information and many other items.

  .. card:: Build Management Software

    .. raw:: html
    
      <iframe 
          src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p13" 
          frameborder="0" width="100%" height="500" 
          allowfullscreen="true" 
          mozallowfullscreen="true" 
          webkitallowfullscreen="true">
      </iframe> 
  
  * The dependencies:
    
    * source dependencies (non-autogenerated depedencies)
      
      * ``.c`` and ``.h`` files are dependencies to :term:`recipe`

    * auto-generated dependencies:

      * These dependencies can be auto-generated and output to ``.dep`` or ``.d``
        files automatically by make.

* Make can be configured to use whichever Complier Toolchain and build process 
  of your choosing.

  * You could use vendor provided toolchains (SDK) instead of gcc
 
* Make could even be configured to support multiple versions of a compiler or 
  even multiple compilers with the same Makefile.



***************************
MakeFile
***************************

Makefiles

* one or more files used to tell ``make`` how to build a particular project
* provide special directions and procedures to ``make`` in order to create an 
  executable file from a multitude of input files.

* contains specific recipes for building particular targets.

  * Make have build *target* or build *rules*
  * invoke make syntax: ``make <target>``

    * target contain recipe (a action/command or set of action that ``make`` carries out
      ) for how to build a particular executable or non-source  files

* many different target or rule can be executed for any given instance of ``make``
  that is run

  * typical targets:

    * direct object compilation
    * complete build ``make all``
    * clean ``make clean``

      * removes all generated files.
  
  * if no target is specified, make will default to the first defined target
    in the makefile::

      $ make

* Can define variables/constants to use during compilation.

  * Compiler Instance
  * Compiler/Linker Options
  * Architecture to build for

*********************************
Make vs IDE
*********************************

**How does the command-line Make system differ from what we did with our 
integrated development environments?**

* Some IDE auto-generate their Makefiles

  * Depends on the project configuration, IDE will generate:

    * all of the specific flags
    * linker files

      .. grid:: 2

         .. grid-item-card:: KDS IDE Build Compilation output Example   

             .. raw:: html
                
                <iframe 
                  src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p16" 
                  frameborder="0" width="100%" height="500" 
                  allowfullscreen="true" 
                  mozallowfullscreen="true" 
                  webkitallowfullscreen="true">
                </iframe>     
     

         .. grid-item-card:: Many GCC Flags     

             .. raw:: html
                
                <iframe 
                  src="https://docs.google.com/presentation/d/e/2PACX-1vQKXjWTU3zTA9tzIqIEe1azv-wFQWNCSF1DFpaLxVjGz1uDYTWhMZxobAFkwMUl61pVd5SSGUkbTBM2/embed?start=false&loop=false&slide=id.p17" 
                  frameborder="0" width="100%" height="500" 
                  allowfullscreen="true" 
                  mozallowfullscreen="true" 
                  webkitallowfullscreen="true">
                </iframe>
      
* IDE is good

  * for very simple interface adn fast

* IDE is not good for

  * maintainability and portability.

.. important::
   
   Software teams DO NOT use IDE autogenerated build systems,
   they create their own.

   .. note:: Rich's note
      
      I have seen a great article about why you should create your own c/c++ 
      environment

      `5 reasons to build your own c/c++ environment <https://www.embedded.com/5-reasons-to-build-your-own-c-c-environment/>`_


********************
Demo
********************

An example of a project demo with cross-compilation using Make and
makefile.

Githup repo: https://github.com/afosdick/ese-coursera-course1

* This is also a submodule under _resource.

The folder structure

.. code-block::
   
   ricky-wsl@Rich-LenovExtX1:Embedded_System/_resources/Courses/UCBoulder-ESE/ESE-1_Repo/demos/m2(master)$ tree v6-v7/
   v6-v7/
   ├── Demo_Commands.txt
   ├── MKL25Z128xxx4_flash.ld
   ├── Makefile
   ├── main.c
   ├── my_file.c
   ├── my_file.h
   ├── my_memory.c
   ├── my_memory.h
   └── sources.mk
   

.. tab-set::

    .. tab-item:: Makefile
        :sync: key1

        .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/ESE-1_Repo/demos/m2/v6-v7/Makefile
           :language: c

    .. tab-item:: sources.mk
        :sync: key2

        .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/ESE-1_Repo/demos/m2/v6-v7/sources.mk
           :language: c

Pre-req for fulfilling this demo:

1. Have the cross compiler ``arm-none-eabi-gcc`` installed
   
   * You can search for it on ubuntu using this command syntax 
     ``sudo apt-cache search <package-name>``

     .. code-block:: bash
        
        ricky-wsl@Rich-LenovExtX1:$ sudo apt-cache search arm-none-eabi
        [sudo] password for ricky-wsl:
        binutils-arm-none-eabi - GNU assembler, linker and binary utilities for ARM Cortex-R/M processors
        gcc-arm-none-eabi - GCC cross compiler for ARM Cortex-R/M processors
        gcc-arm-none-eabi-source - GCC cross compiler for ARM Cortex-R/M processors (source)
        gdb-multiarch - GNU Debugger (with support for multiple architectures)
        libnewlib-arm-none-eabi - C library and math library compiled for bare metal using Cortex A/R/M
        libstdc++-arm-none-eabi-dev - GNU Standard C++ Library v3 for ARM Cortex-R/M processors (headers)
        libstdc++-arm-none-eabi-newlib - GNU Standard C++ Library v3 for ARM Cortex-R/M processors (newlib)
        libstdc++-arm-none-eabi-picolibc - GNU Standard C++ Library v3 for ARM Cortex-R/M processors (picolibc)
        mbed-test-wrapper - utility to wrap the mbed test loader for use by yotta targets
        picolibc-arm-none-eabi - Smaller embedded C library for ARM development
        ubertooth-firmware - Firmware for Ubertooth
        ubertooth-firmware-source - Source code for the Ubertooth firmware

   * You can install it using ``sudo apt-get install gcc-arm-none-eabi``

Invoking make using the command syntax ``make <target>``

* ``make all``

  * this will build the entire project using a phony target

    * :term:`phony target` is one that is not really the name of a file; 
      rather it is just a name for a recipe to be executed when you make an 
      explicit request
  
  * The generated output will be create:

    * Object file/code: ``main.o``, ``my_file.o``, ``my_memory.o``
    * Executable: ``demo.out``, 
    * map file: ``demo.out.map``, 

* ``make clean`` will remove all non-source and generated files
  
  .. warning::
     
      Be careful how you implement clean as you don't want to delete source files
      and other important files.

* You can define your build target to run individual build files: ``make main.o``

  * The main.o object file will be built (no linking yet). 
  * Only a single command is issued.
