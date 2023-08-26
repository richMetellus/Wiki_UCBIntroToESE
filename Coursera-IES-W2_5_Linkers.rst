.. _CLibrariesAndModules:

.. include:: /global.rst
   :start-after: markupExtenstionStart
   :end-before: markupExtenstionEnd

################################################
Linkers
################################################


***************
Objectives
***************

Goals

#. Discuss the importance of linkers in the build process
   


*******************************************
The Generalized Typical Build Process
*******************************************

.. card:: Build Process Last 2 steps

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe 
         src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p4" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe>

The two steps of linking and locating occur in the last phase of our build process

* Typically the process of linking and locating is done in one stage.

++ Recall with this generalized build process:

* Assembly and C files get compiled from the same project into objects. 
* Compiled library code is pulled in during linking

* the job of the linker is to take your compiled object files and to combine 
  them into a single :term:`object file`, or an executable. 

* The locater then maps this object into specific address locations, producing 
  an executable program that can be installed into your embedded processor

++ The :term:`linker file`  

*  is input into our linking and locating stage
* and is responsible for telling the locator how to map our executable into 
  the proper addresses. 

* this file is passed in with the ``-T`` flag.


******************************************************
Linkers Responsiblity 
******************************************************

More details on what the linker is doing:

.. card:: Linker iner Details

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe 
         src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p5" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe>

The linker is a program and its Responsiblities are:

* make executable file\**s** (lplural - like in the case of muti processes software program).

   * combine all the compiled object files into a single object file containing 
     your entire program. This is the executable

* resolves linkage issues, such as such as the use of symbols or identifiers 
  which are defined in one translation unit and are needed from other translation units

  * :term:`Object files <object file>` need to be combined into a single executable where all
    references between :term:`object files` need to be resolved.

    -  We call these references symbols.
    -  Now, this job is performed by the linker.

****************************
Linking Process
****************************

Linking process:

* The process of linking is performed directly from the 
  ``ld`` application (GNU Linker) or indirectly from the compiler application 
  GCC.

  * gcc merely acts as a front-end to ld at link time; it passes all the 
    linker directives
    
    * You can invoke gcc for all the steps in the build process


* Linking process combine all object files into a single executable

   * Individual :term:`object file` that is compiled cannot be executed. (specially
     not linked to main function)
   
      * These objects contain raw object code with symbol references and a 
        symbol table, or a binary file.
        
        .. card:: Object file (main.o, and memory.o) Pass to linker

           .. raw:: html

              <iframe 
                src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p6" 
                frameborder="0" width="100%" height="500" 
                allowfullscreen="true" 
                mozallowfullscreen="true" 
                webkitallowfullscreen="true">
              </iframe>



   * The linking process is **not as simple** as appending the contents of these object 
     files into one location. 
      
      * Each object file is made up of different memory segments that specify 
        different types of code that exist in your file. 
        
        * For example, code memory, which includes your functions, is managed 
          differently than how your data memory is managed. To complicate this 
          even more, cross-references or cross-memory segments need to be 
          resolved. 

* You can think of linking process as merging code, mapping symbols, and 
  assigning addresses.

  .. image:: ../../_images/c1m2v5_linkers_crossreferencing_scrnsht.png
     :target: https://medium.com/@chiaracaprasi/static-library-in-c-programming-76a6af5ff0e7


Linking Object Files: Function & Variable -> Symbols
========================================================

The code you designed was likely written with many modules, defining many 
functions and variables. 

.. card:: Function Reference in other file

   .. raw:: html
   
       <iframe 
         src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p8" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe>
   
* These functions and variables are likely used across many other files, or even 
  modules. 
  
  * functions might be referenced in other files
    (main.c make ``memzero()`` function call)

* The functions and variables become important references that the compiler 
  tracks the symbols.

:term:`Object files <object file>` need to be combined into a single executable where all
references between :term:`object files` need to be resolved.

-  We call these references symbols.
   
   * The symbols defined in one file and used in another need to be mapped so 
     that the location of the symbol's address is known and assigned properly to 
     all uses of that symbol.


| So Function & Variable becomes (->) symbols
| Linker resolve symbols

.. card:: main.o and memery.o Object code

   .. raw:: html
   
       <iframe 
         src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p9" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe>

**The linker will need to locate the symbols**

* by searching through all of the object sources that were provided, trying to 
  find the one that matches. 

  * If it was not found there, 
    
    * it will try to search through its library paths to find that symbol. 
      
      * If it cannot resolve it on its own, the linker will throw an error and exit.
        
        .. card:: main.o and memery.o Object code

           .. raw:: html
           
               <iframe 
                 src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p10" 
                 frameborder="0" width="100%" height="500" 
                 allowfullscreen="true" 
                 mozallowfullscreen="true" 
                 webkitallowfullscreen="true">
               </iframe>

Linking Static and Shared (dynamic) Libary
===========================================

.. grid:: 2

    .. grid-item-card:: Library Inclusion Code Example

        .. raw:: html
           
           <iframe 
             src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p11" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>


    .. grid-item-card:: Library Inclusion Code Example

        .. raw:: html
           
           <iframe 
             src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p12" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>

Pre-compiled static libraries 

* will directly link at linking time into your output executable. 
  
* You need to provide the ``-l`` or ``-L`` flag to include these libraries ( whether
   static or dynamic). 

* However, dynamically linked symbols will contain paths to dynamic libraries 
  already installed on your device. 
  
  * This could cause issues if there was an incompatibility of the library that 
    is installed on the device and the library headers you are including.

  * System with Embedded OS most likely have library already installed on the 
    device.

    * So there is not reason to statically compile and upload.
      Save code space by just dynamically link at runtime.

.. Admonition:: TODO
   
   ToDo: Find Answer to these questions

   #. So How Do you know if a program was linked to a static or shared library?
      
      A: On linux, you can use ``ldd`` application

      see this `stkovf - How One Can tell if a program linking to a static or dynamic library <https://stackoverflow.com/questions/4281360/in-linux-how-can-i-tell-if-im-linking-to-a-static-or-dynamic-library>`_


   #. Would a microcontroller (not mpu) without OS would include some installed
      shared library (just like Linux/Unix based system)?

      A: No. See this `stackovf question <https://stackoverflow.com/questions/11707912/static-versus-shared-libraries-in-small-embedded-systems-using-c-without-os-ass>`_

How is reference to Main resolved?
===================================

.. card:: main.o and memery.o Object code

    .. raw:: html
    
        <iframe 
          src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p13" 
          frameborder="0" width="100%" height="500" 
          allowfullscreen="true" 
          mozallowfullscreen="true" 
          webkitallowfullscreen="true">
        </iframe>

You already have experience using statically linked libraries, and you probably 
did not realize it. Ask yourself:

1. What happened before main is called? And How do we return from main?
   
   * There are startup routines that run before main.
     
     * THey are usually defined in some C standard libraries and are automatically
       included in your build as a static libary.
     
     * Method to test this:
       
       * tell GCC to stop auto-link of standard libs with ``-nostdlib`` flag
         
         * This flag stops the linking of startup routines and libraries. 
           You can also use the verbose flag, -v, to see more details of the 
           compilation and linking process.
         * Without standard lib, you would have to define your own initialization
           and exit software routines.


Symbols Resolved -> Relocatable File, Symbols replace by address -> Executable File
=====================================================================================


.. grid:: 2

    .. grid-item-card:: Relocatable File

        .. raw:: html
        
            <iframe 
              src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p14" 
              frameborder="0" width="100%" height="500" 
              allowfullscreen="true" 
              mozallowfullscreen="true" 
              webkitallowfullscreen="true">
            </iframe>
        
        After all linking is done and a final object file has been created with all 
        symbols resolved (found and included in just one file), the output is called a 
        relocatable file.
        
        Relocatable file 
        
        * A relocatable file holds code and data suitable for linking with other 
          object files to create an executable or a shared object file
        
        * contains many sub segments of code blocks.        

    .. grid-item-card:: Executable File

        .. raw:: html
        
            <iframe 
              src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p15" 
              frameborder="0" width="100%" height="500" 
              allowfullscreen="true" 
              mozallowfullscreen="true" 
              webkitallowfullscreen="true">
            </iframe>
       
               
        The relocatable file contains many sub segments of code blocks
        
        * the defined blocks of code will need to be mapped into the architecture's 
          memory regions by assigning a specific address to various groups 
          of symbols.
        
          * the locator will need some special direction on how to perform this general 
            file to specific address mapping since each microcontroller architecture
            is different.
                
            * Hence, the linker file/script

**********************
Linker File
**********************

.. grid:: 2

    .. grid-item-card:: Linker File Job

        .. raw:: html
           
           <iframe 
             src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p16" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>


    .. grid-item-card:: Linker Script Details

        .. raw:: html
           
           <iframe 
             src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p17" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>

The linker file

* is architecture-dependent
* is used in the memory relocation process
* will contains details that tell what the physical memory map of your embedded
  system is.

  * Linker file specifies:

    * segment name
    * region name
    * memory size
    * access permission



Example of Linkeer Script Content
===================================

your program executable will be split into

* many memory regions/segment during installation. 
  
  * These segments are the physical parts of memory on your 
    microcontroller.
  

* many sections.

  * Example of different section:

    * code
    * initialization data
    * stack
    * heap
    * data section

  * Each "code" sections are then mapped into these physical memory segments.
     
     * Each section specifies the location the compiled region should map
       into physical memory ``MAIN (ex: Flash)`` or ``SRAM_DATA``

     .. card:: main.o and memery.o Object code
     
         .. raw:: html
         
             <iframe 
               src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p21" 
               frameborder="0" width="100%" height="500" 
               allowfullscreen="true" 
               mozallowfullscreen="true" 
               webkitallowfullscreen="true">
             </iframe>
     
the linker needs to 

* know where code memory should be assigned. 

* and  Also, for the  data that code uses, exact addresses need to be given to your program so it 
  can find the data it's trying to operate on.

  * hence the memory region address start ``origin=<0xaddresspace>`` and lenght.


* Each emory segment provides the starting location and the length of memory in bytes.

  * The total number of memory segments must be equal to or larger in size than 
    what the total compiled code and data sections add up to, 
    
    * otherwise you will have errors at installation.

      * To prevent that put small region overallocated check in your linker script
        
        .. card:: main.o and memery.o Object code

           .. raw:: html
    
              <iframe 
                src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p21" 
                frameborder="0" width="100%" height="500" 
                allowfullscreen="true" 
                mozallowfullscreen="true" 
                webkitallowfullscreen="true">
              </iframe>     

        .. code-block:: c
           :caption: linkerfile.ld example of invalid memory space check
           
           HEAP_SIZE  = DEFINED(__heap_size__)  ? __heap_size__  : 0x0400;
           STACK_SIZE  = DEFINED(__stack_size__)  ? __stack_size__  : 0x0800;
           
           __StackTop   = ORIGIN(SRAM_DATA) + LENGTH(SRAM_DATA);
           __StackLimit = __StackTop - STACK_SIZE;
            ASSERT(__StackLimit >= __HeapLimit, “Region SRAM_DATA overflowed!")
        
        * In this example, we verify that the heap and stack are not overflowing
          into one another in data memory. If they do, the linker throw an error
           

Memory regions permission
--------------------------

Each memory region specifies access permissions such as 
read, write, and execute for memory blocks.

Data memory

* Typically, your data memory, which will be in the SRAM, will be set as read 
  and write, or RW.

  * This is because you will often read, modify, write data from SRA

Code Memory:

*  will have read execute or RX permissions. 
  
  * This is because we will read our instructions from code memory like flash and execute them on the CPU.

Some Reasons to make certain memory regions read-only:

1. Prevent an accidental overwrite of your code memory to prevent your program to
   be corrupted

#. Security
   
   * Prevent hacker from adding new code into the code memory region
   * Some processors even cause a fatal error or exception if the processor 
     tries to execute code out of SRAM or data memory regions.

******************************
Memory Segment in Action
******************************

An example of where we took a basic linker file, assigned some arbitrary sizes, 
and provided physical memory spaces

.. card:: Arbitary size and mapping example

   .. raw:: html
   
       <iframe 
         src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p28" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe>

* the linker file shows how the physical data memory and code memory 
  are mapped into our address space

  * in this case, our data memory is in the SRAM, and the main code will go into the flash.
    
    .. card:: main.o and memery.o Object code

       .. raw:: html
       
           <iframe 
             src="https://docs.google.com/presentation/d/e/2PACX-1vQ6kuVxGcwI0b7mBkgyLBjaN-5WdX2zsBGz8_dy-k846bYMxOFWuLOgJ4vvFDTm3AqzwzJGHjs3lbbz/embed?start=false&loop=false&slide=id.p29" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>
    
    * Then each code and data section specify their intended map locations. 
    * Each of these will have varying size and will take up space in our main 
      and SRAM memory segments. 
      
      * It is likely that not all of the physical memory will be needed

***********************
Linker Flags and Examples
***********************

 If interested in seeing how the memory allocation was done. 
 For this, you can tell the linker to produce a map file. 

* .. csv-table:: GCC Flags
     :file: ../../_resources/Courses/UCBoulder-ESE/c1m2v5_LinkerFlags.csv
     :header-rows: 1

If you choose to invoke the linker indirectly through the compiler, you can 
still specify these flags by providing the ``-Wl`` or ``-Xlinker`` flag with 
GCC. And this allows us to pass flags down through GCC to the linker.

************************************
Executable File Formats
************************************

When linking and locating is finished, you will produce an output executable 
file that can go to your installer. 

The format of this file varies depending on the installer and architecture.

* some pretty typically output files like 
  
  * Executable and Linkable Format (ELF)
  * Common Object File Format (COFF)
  * Intel Hex Record
  * Motorola S Record (SREC)
  * ARM Image Format (AIF)

.. note:: Rich's Note

   These formats are not exclusive to just an executable. For example,
   the Elf (formerly name Extensible Linking Format) file format, 
   first published in Application Binary Interface (ABI) 
   specification, is a common standard file format for executable files,
   object code, shared libraries, and core dumps.
   `Linux Hint Blog on Elf <https://linuxhint.com/understanding_elf_file_format/>`_

***********************
Other References
***********************

1. `Red Hat Book on GNU Linker <https://www.eecs.umich.edu/courses/eecs373/readings/Linker.pdf>`_
   
   * I also have a copy on ndsu sharepoint.


2. `StackOvf - Why Need Linker script and Startup code <https://stackoverflow.com/questions/41365110/why-need-linker-script-and-startup-code#:~:text=The%20linker%20script%20is%20input,some%20assumptions%20about%20the%20environment.>`_
