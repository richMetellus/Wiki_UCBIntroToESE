.. _CLibrariesAndModules:

.. include:: ../global.rst
   :start-after: markupExtenstionStart
   :end-before: markupExtenstionEnd

################################################
Libraries And C Modules [1]_
################################################


***************
Objectives
***************

Goals

#. Learn about how to design modulare software
    
    i. creating our own libraries
    #. Re-use libraries across software projects
    #. Write code to be testable and portable


**********************************
What are Libaries?
**********************************

:term:`Software Library` or libaries

* Libraries are just a collection of software and source code, or precompiled 
  code format.

    * We can design our own libraries to create reusable modules, accross different
      software projects.

* Libraries are a collection of useful reusable code — this could be commonly 
  used algorithms, data structures, and mechanisms for input and output. [2]_
* library files can't be executed — libraries are utilised.
* The library filenames always start with lib, and end with an extension 
  of ``.a`` for archive, static library. ( for shared object, 
  dynamically linked library it's ``.so``).
* A library is basically just an archive of :term:`object files`
* Example of Librarry:

    * C standard libary:
        
        * has module that provide function for performing input and output
          operation: ``#include <stdio.h>``
          
:term:`Modules`

* Software organization that each mdodule has encapsulated ceratin functionality
  within a libary

    * create portable code
    * ex:

        ``#include <math.h>``

            * this perform math operations like square root

.. admonition:: Advice

    You will want to code modules to encapsulate specific functionality, so they
    can be easily reused.

    * This can contain application specific functions or just constant data

    You would want to **design in pieces or blocks** to help split up the work
    and ease your validation process.

    * With each module, you can write software tests to validate their 
      operation before integrating them In to a larger application, ie, 
      a unit test.

****************************
Create Your own Library
****************************

Create A module: Header File and Implementation File
=======================================================

First, you need to know how to write a module.

Each module will contains 2 files

* a header file (.h)

    * this file contains function declarations, macros, and even certain derived
      data type definitions, like enumerations or structures.
    
    .. code-block:: c
        :caption: memory.h

        #ifndef __MEMORY_H__
        #define __MEMORY_H__
        
        char memzero(char * src, int length);
        
        #endif /* __MEMORY_H__ */
        


* an implementation file (.c)

    * the source file contains functions definitions and other c code or the 
      actual implementation details

        .. note::

            The c file should not be directly accessible by software projects.
            Anything you want accessible should be declared in the c files 
            associated header file.
    
    .. code-block:: c
        :caption: memory.c

        #include "memory.h"
        char memzero(char * src, int length){
          int i;
          for(i = 0; i < length; i++){
            *src++ = 0;
          }
        }
        
Header File - Include Guards - #ifndef
--------------------------------------------

Within the top and bottom of header file, there are some special lines
that form what we called "include guards"

.. code-block:: c
    :caption: memory.h
    :linenos:
    :emphasize-lines: 1-2, 6

    #ifndef __MEMORY_H__
    #define __MEMORY_H__

    ...

    #endif /* __MEMORY_H__ */


Include guards

* These usually include the special ``#ifndef``, ``#define``, and ``#endif`` 
  directives, that contain the entire contents of the file in between these 
  statements

* protects against redundant includes

    * If you don't include this in your header file, you won't protect against 
      circular or redundant includes
    
    * Ex: Say you create a ``main.c`` that have #include "memory.h" duplicated

        * With ``ifndef`` include guards, You have protection
          
          .. list-table:: Importance of Include Guards
             :align: right
             
             * - memory.h
               - main.c
               - main.c (post pre-processing)

             * - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_memory.h
                    :language: c
                    :emphasize-lines: 1-2, 6
               - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_mainWith2Include.c
                    :language: C
                    :emphasize-lines: 1, 2
               -  .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_WithIncludeGuard_mainPreprosessingResult.c
                    :language: C
                    :emphasize-lines: 1, 2
        
          * To illustrate this, let's pretend a programmer mistakenly include 
            the header twice (IRL, it usually different header file that have common
            #include "moduleName.h"). See `Wikipedia article <https://en.wikipedia.org/wiki/Include_guard>`_
            
            with the include guard, in main.c 

            1. the first ``#include "memory.h"`` will make the preprocessor 
               copy the content of memory.h in main.

               i. The preprocessor register __MEMORY_H is not defined, then now 
                  define it
               ii. The preprocessor copy the content in main.c
            
            2. the preprocessor see the second ``#inlcude "memory.h"``

               i. The preprocesor will skip and not copy again the content that are
                  in between the include guard since ``__MEMORY_H__`` was already defined
                  in step ``1.i``
                  
                  * thus protecting ``char memzero(char * src, int length);`` from
                    being included twice.

        * Without include guards:
          
           .. list-table:: Importance of Include Guards - No Include guards
             
             * - memory.h
               - main.c
               - main.c (post pre-processing)

             * - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_memoryNoIncludeGuard.h
                    :language: c
                    :emphasize-lines: 1, 4
               - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_mainWith2Include.c
                    :language: C
                    :emphasize-lines: 1, 2
               
               - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_NoIncludeGuard_mainPreprosessingResult.c
                    :language: C
                    :emphasize-lines: 1, 2

Header File - Include Guards - #pragma once
----------------------------------------------

An alternative to the ``#ifndef`` include guard, and a good example of the use of a 
pragma, is the ``#pragma once`` directive. 

The ``#pragma once`` directive. 

* is a nonstandard directive, that **only certain compilers support**.
* This feature allows us to use only a single line to create an include guard. 
  
  * However, because of the limited support, using only this option would **not** 
    make your code **portable** to other compilers or compiler versions.

  
* Ex: Say you create a ``main.c`` that have #include "memory.h" duplicated

    * With ``#pragma once`` include guard, You still have same protection (same result)
      
      .. list-table:: Importance of Include Guards - pragma once
          :align: right
          
          * - memory.h
            - main.c
            - main.c (post pre-processing)

          * - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_pragma_memory.h
                :language: c
                :emphasize-lines: 1-2, 6
            - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_mainWith2Include.c
                :language: C
                :emphasize-lines: 1, 2
            -  .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_WithIncludeGuard_mainPreprosessingResult.c
                :language: C
                :emphasize-lines: 1, 2

Header File - Module Description
---------------------------------

* Header files are the **interface**
  
  * Anything you want to give access to, put in header file

* Include informative comment in header file

  * function Description:

    * Input: type and description
    * Return: Type and description

.. admonition:: Advice
   
   When you define your own modules, 
   
   * give each module a good name, relating to its functionality.
   * Make your header files descriptive, by including a description of the
     information it covers at the top of the file.

      * Also do that for your function prototype by including informative comment
        on how to use each function.

   * Give the associating include guard preprocessor directive, a similar name 
     in all caps.
   * Avoid defining variables in the header file, 
      
      * because every time it is included, a symbol with that name will be declared.

.. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m1v4_description_memory.h


*****************************
Libraries Providers
*****************************

.. 4'11 (4 minutes 11 sec mark)

.. card:: Precompiled Libaries

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p16" 
          frameborder="0" width="100%" height="500" allowfullscreen="true" 
          mozallowfullscreen="true" webkitallowfullscreen="true">
       </iframe>


**C Standard Libaries**

* C programming has a large variety of **prewritten** code called the 
  standard libraries.

  * Example of standard library files:

    * Standard IO ``#include <stdio.h>``
    * Standard Lib ``#include <stdlib.h>``
    * Math ``#include <math.h>``

  * see `difference between header file vs Library file <https://www.differencebetween.com/difference-between-header-file-and-vs-library-file/#:~:text=The%20difference%20between%20a%20header%20file%20and%20library%20file%20is,functions%20in%20the%20header%20file.>`_
          
* Often provided with your compiler toolset and are precompiled.

  * You access them through header definitions.

.. seealso::

   * I have made reference on programming language and
     the glibc (GNU C Library) 
     :ref:`What is Programming and How Is a Programming Language created <ProgrammingIntro>`
       
      * :ref:`standard lib section <standardLibrary>`
 


**Third Party Libaries**

* You may also include 3rd party libary that you downloaded for some specific 
  function. 

* R. Example of 3rd party lib

  * `boost C++ Libaries <https://www.boost.org/>`_


**Be Careful Using Libraries (Optimization and Other Caveats)**


   
Regardless of the source of a library, an important reminder is that 
embedded software needs to be efficient and take up as little code space 
as possible.

.. grid:: 2

    .. grid-item-card:: Library Inclusion Code Example

        .. raw:: html

           <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p17" 
              frameborder="0" width="100%" height="500" allowfullscreen="true" 
              mozallowfullscreen="true" webkitallowfullscreen="true">
           </iframe>


    .. grid-item-card::  Important Questions
        
        .. admonition:: Advice
            
            When you include a library, you should ask yourself, 
            
            * if this library precompiled:
            
              * Is it compiled for my architecture? 
              * Was it designed to be optimized for my architecture?

            * If you have the library source code, you should wonder:

              * what software features does this use? 
              * And what other code does this include? 
   

**Librariy Optimization And HW Accelerators**

.. card:: Library Optimization And HW Accelerators

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p20" 
          frameborder="0" width="100%" height="500" allowfullscreen="true" 
          mozallowfullscreen="true" webkitallowfullscreen="true">
       </iframe>

More things to be aware of:

#. The standard libraries provided with your compiler, will likely be pretty 
   optimized for the instruction set architecture you are using. 

   * This means  that they're not likely optimized for your specific 
     embedded platform (Platform can be KLZ, STM32).

#. There are hardware blocks (Hardware accelerators) that may allow us to get
   even better performance than just software solutions.

#. Some libraries may require large amounts of memory to operate on, and the 
   embedded platform might not have that much memory.
   
   * Software organization often rewire useful functions from these libraries 
     (ex: dorado sysmemcopy)


******************************
Type of Compiled Libraries
******************************

When it comes to compiled libraries there are two types, 

* static libraries 

* and shared libraries. 

++ The library filenames always start with lib, and end with an extension 
of ``.a`` for archive, static library. (for shared object, 
dynamically linked library it's ``.so``).

.. important::
   
   you need the declarations of the library's header files to properly link 
   against that library. Otherwise, the linker will try to search its own 
   paths for the undefined symbols

Static Libraries
========================

.. card:: Static Library

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p23" 
          frameborder="0" width="100%" height="500" allowfullscreen="true" 
          mozallowfullscreen="true" webkitallowfullscreen="true">
       </iframe>

Static Libraries

* Static libraries are libraries that are compiled and directly **linked into 
  your output executable at compile time**.

  * These include the standard libraries that are provided with your 
    compiler toolchain.

* In static linking, the linker makes a copy of all used library functions code 
  to the executable file in the linking step, before run time. [2]_
* Static libraries can be created using the archiver, GNU tool ``ar``

Shared Libraries
========================

.. card:: Shared Library

    .. add &slide=id.p<slidenumber> to include the slide in the iframe

    .. raw:: html

       <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p24" 
          frameborder="0" width="100%" height="500" allowfullscreen="true" 
          mozallowfullscreen="true" webkitallowfullscreen="true">
       </iframe>

Shared libraries 

* Shared libraries are libraries that **linked dynamically at runtime with your executable.**
  
  * These shared libraries exist in your platforms memory already, and are **not** 
    installed with your executable.

* You can create shared libraries with the shared option from GCC.

  .. warning::
     
     Sometimes embedded manufacturers also provide a section of memory dedicated 
     to storing precompiled libraries on the hardware. If you do install a 
     shared library on your embedded system, it is important that it is put in 
     a region of memory that will not be overwritten.

* Shared Library are useful when running multiple programs using the same libary.

*******************************************************
Cross Compilation and How Are Libraries Included?
*******************************************************


You might ask yourself

#. how does cross compilation for my ARM architecture affecting including 
   libraries into my project?

   .. card:: Build Process Library
 
     .. add &slide=id.p<slidenumber> to include the slide in the iframe
 
     .. raw:: html
 
        <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p26" 
           frameborder="0" width="100%" height="500" allowfullscreen="true" 
           mozallowfullscreen="true" webkitallowfullscreen="true">
        </iframe>
   
   * These libraries has been precompiled for a given architecture by a specific 
     compiler. 
     
     * **By using the ARM compiler, we already have libraries defined for the 
       ARM ISA**. 
       
       * To complicate this further, is that there are many different flavors of 
         the ARM ISA (armv7, armv8).

#. How do you know if your compiled library is using ARM Thumb encoding, or 
   something like ARMv7? 
   
   * If the library was designed right, it will support multiple architectures, 
     and this is provided via flags at compilation.


.. seealso::
   
   See the `ARM GNU Toolchain <https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain>`_

***************************************************************
How Do You Structure Your Project and Design Your Module
***************************************************************

.. card:: Build Process Library

  .. add &slide=id.p<slidenumber> to include the slide in the iframe

  .. raw:: html

    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p27" 
        frameborder="0" width="100%" height="500" allowfullscreen="true" 
        mozallowfullscreen="true" webkitallowfullscreen="true">
    </iframe>

You might ask,

#. How Do you design your file system around your software files?
#. What are the architecture and platform dependencies?
#. Where do you set the flags (command line, in software preprocessor)?
   
   * The choice is up to you.

Compile Switch and Architecture Dependent Compilation
========================================================

Here is the scenario: 

* you have a build system, that supports two boards, with two different 
  micro controllers, 

  * an MSP 
  * and KL25Z. 

* We have a main file (``main.c``) that wants to be architecture independent

  .. list-table:: Architecture Specific Include
     
     * - msp_platform.h
       - kl25_platform.h
       - platform.h
       - main.c

     * - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_msp_platform.h
            :language: c
            :emphasize-lines: 4
       - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_kl25_platform.h
            :language: c
            :emphasize-lines: 4
       - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_platform.h
            :language: c
            :emphasize-lines: 6-11
       - .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v4_platform_main.c
            :language: c
            :emphasize-lines: 1, 5

  * this file include an intermediate file called ``platform.h``

    * This platform file helps us to select between which 2 different
      architecture of the platform we support based on a compile time switch.
    
    * Given the swich ``-D<PreprocessorVariable>`` in the build system, the 
      correct architecture gets included.

      .. card:: Build Process Library

         .. add &slide=id.p<slidenumber> to include the slide in the iframe
 
         .. raw:: html
 
           <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRvYdcXo7lJkAfPboML8aM6kaPEpqEUsewpYI-fbR7Tv1fA59a9evm5oXlZzqJsLnMVU3ZoACAE5LDv/embed?start=false&loop=false&slide=id.p30" 
               frameborder="0" width="100%" height="500" allowfullscreen="true" 
               mozallowfullscreen="true" webkitallowfullscreen="true">
           </iframe>

    .. note::
      This method require us to create the same fumction name for both of the 
      architecture specific files (msp_platform.h, kl25_platform.h) so that higher
      levels (main.c) are compativle with the  ``initialise`` function

There are other options to perform this architecture dependent compilation. 

* By putting compiled times switches directly in the code (directly into main.c 
  in our case), instead of intermediate files. 
* Or even using version control to help create different file trees.

.. admonition:: Advice
   
   For effective design of software libary:

   * You should take time to think through code design, before trying to write 
     any implementation. 
  
   * Think about things like
     
     * how your code with interface with each other, 
     * the purpose of a module, 
     * the purpose of the functions, 
     
     These will help you with your software classification and segmentation.

*********************
References
*********************

.. [1] `Coursera UC Boulder ESE Week2 Lesson 4 - Creating Header and Implementation files <https://www.coursera.org/learn/introduction-embedded-systems/lecture/d79O6/4-creating-header-and-implementation-files>`_
.. [2] `MediumBlog - Static Libraries by Chiara Caprasi <https://medium.com/@chiaracaprasi/static-library-in-c-programming-76a6af5ff0e7>`_
.. [3] `Randu - Create static and shared(dynamic) libaries <https://randu.org/tutorials/c/libraries.php>`_
