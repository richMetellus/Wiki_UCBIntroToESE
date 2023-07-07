.. include:: /global.rst
   :start-after: markupExtenstionStart
   :end-before: markupExtenstionEnd

######################################################################################################################################################################################
`Preprocessor Directives <https://www.coursera.org/learn/introduction-embedded-systems/lecture/VDPBC/3-preprocessor-directives>`_
######################################################################################################################################################################################

***************
Objectives
***************

Goals

#. Learn about different preprocessor directives and 
#. How they can help with improving software quality.
#. How they can be used to create compile time switches
#. Go through Example of how to invoke the :term:`preprocessor` and analyze the output.



..
    slide

****************
Slides
****************

.. .. raw:: html
   
..    <!DOCTYPE html>
..    <html>
   
..    <head>
..        <title>Preprocessor Directives</title>
..    </head>
   
..    <body>
..        <p><a href="https://docs.google.com/presentation/d/1MMtnFfWkAEsNnYShwj5ZMFCxFdl366Ti5XsL_IcFf-g/edit?usp=sharing"
..                target="iframe_intro_to_embedded">Restart Slides From the Beginning</a></p>
..        <center>
..        <iframe name="iframe_intro_to_embedded" frameborder="0" scrolling="no" width="60%" height="600px" src="https://docs.google.com/presentation/d/1MMtnFfWkAEsNnYShwj5ZMFCxFdl366Ti5XsL_IcFf-g/edit?usp=sharing">
..        </iframe>
..        </center>
..    </body>
   
..    </html>


**********************************
Transcribed Notes and More
**********************************

Preprocessor and Preprocessor Directives Definition
=======================================================

:term:`Preprocessor <preprocessor>`

* is a macro processor that is that is used automatically by the C compiler to transform your 
  program before actual compilation (Preprocessor directives are executed before compilation.)
    
    - It is called a macro processor because it allows you to define :term:`macros`

* A :term:`preprocessor` does not do any translation or generation of architecture specific code. 
  It does however, help us with providing specific control of compilation from within files, 
  as well as provide macros that can help with code reuse and readability.

    .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
        :width: 800
        :height: 500
    
    * The preprocessor takes your original files and transforms them based on the directives you 
      have defined within a file.



* The pre-processor stage is the first stage in the build process

  .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
     :page-index: 1
     :width: 800
     :height: 500


**Preprocessor Directives**

* Start with ``#`` sign

* 4 Major Types of Preprocessor Directives

    #. Macro Expansion
        
        * can be used  to define constants or feature, as well as define macro functions
        
            * Macro is a term often used interchangeably with the application of a preprocessor directive.
            * :term:`Macros <macros>` are pieces of code in a program that is given some name. Whenever this name is 
              encountered by the compiler, the compiler replaces the name with the actual piece of code.
        
        * ``#define``
            
            - use to define a macro.
    
    #. File Inclusion
       
       * This type of preprocessor directive tells the compiler to include a file in the source 
         code program. There are two types of files that can be included by the user in the program: 
            
            * Header files 
            * or Standard files

       #. ``#include``

    #. Conditional Compilation

        * are used for conditional compilation.

        #. ``#ifdef``, ``#endif``
        #. ``#if`` , ``#else``, ``#elif``, ``#endif``

            * When combining with the ``#ifdef`` directives and conditional compilation, you can provide compile
              times switch functionality into your build.
        
    #. Miscellaneous Directives

        #. ``#undef``

            - cancel a macro definition that was previously set with ``#define``  directive

        #. ``#pragma``

            * allows us to provide instructions to the compiler through the code rather than at the command line.

        #. ``#warning``, ``#error``
            
            * For stopping or printing warning messages


Preprocessor's Role
=====================

.. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
   :page-index: 2

* The preprocessor takes your original files and transforms them based on the directives you have
  defined within a file. 
    
    - You can think of this transformation as a search and replace for many directives.
    - :strike:`The preprocessor is bundled with the gcc application.``


**++** You can tell gcc to stop at the preprocessing stage by providing the ``-E`` option

* ``gcc -E -o main.i main.c``

    * Stop after preprocessing
    * output the preprocesed file to a ``*.i`` extension


The ``#define`` Directive
===========================

The ``#define``

* Used for defining constants, features or macro functions

The ``#define`` as a Constant
---------------------------------

The ``#define`` as a Constant

* Syntax

    - ``#define <USER-DEFINE-MACRO-NAME> <MACRO-VALUE>``

        - MACRO-VALUE can be a constant, a string, or even a C statement

* Constants Examples:
  
  .. code-block:: c

     #define LENGTH      (10)
     #define NO_ERROR    (0)
     #define ERROR       (1)

     /* Macro defined as another macro */
     #define UART_ERROR  ERROR

* More examples

  .. code-block:: c
     :linenos:

     #include <stdio.h>  
     #define PI 3.1415
     main()
     { 
        printf("%f",PI); 
     }

  .. code-block:: c

     // output
     main.c:3:1: warning: return type defaults to 'int' [-Wimplicit-int]
         3 |      main()
           |      ^~~~

     3.141500

  .. code-block:: c
     :caption: define and undefine example
     :linenos:

     #include <stdio.h>  
     #define PI 3.1415  
     #undef PI  
     int main() {  
        printf("%f",PI);  
     }

  .. code-block:: c

    // output:
    main.c: In function 'main':
    main.c:5:16: error: 'PI' undeclared (first use in this function)
        5 |    printf("%f",PI);
          |                ^~
    main.c:5:16: note: each undeclared identifier is reported only once for each function it appears in

* This is useful for maintaining constants in your code as it's bad practice to put random
  constants around in your code and many random constants throughout your code will be hard to maintain.
  Other engineers can look at the macro definition and understand the number you chose.
    
    - Every time you change that defined value, it will be reflected in every use of the macro in your code. 
      The engineer only needs to worry abut the original file where the macros are defined.


.. warning::

    If there is a bug in your preprocessor definition, you will not normally see it as the preprocessor 
    output usually goes directly into the compiler. See this :ref:`example <Example of Unwanted Behavior Due to Macro Function Substitution.>`


Macro substitution example 1

.. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
   :page-index: 4


The ``#define`` as a Macro Function
-----------------------------------------

* Syntax: Provide Macro Function name, parameters, and operation

    - ``#define <MACRO-FUNCTION>(<PARAMS>) (<OPERATION>)``

* Examples:
  
  .. code-block:: c
     
     #define SQUARE(x) (x*x)
         …
     int y_sqrd;
     int y = 2;
     y_sqrd = SQUARE(y);

     // y_sqrd will be equal 4
     

Macro substitution example 2:

.. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
   :page-index: 5


Example of Unwanted Behavior Due to Macro Function Substitution.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Macro Function Issue

Because the macro expansion does a direct substitution, you can introduce bugs by operating 
C operations as an input instead of just values.

.. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
   :page-index: 6

if  we passed in ``y++`` and the macro function ``SQUARE`` instead of y,

* the y++ would not just get a post increment once, it would be repeated twice during the substitution. 
  This will yield 2 y++ operation
* And instead of getting a result of y equals 3, and a square of 4, we get ``y = 4 and a y_sqrd = 6.``

|
|

``#define`` / ``#undef`` as a Feature
-----------------------------------------

You don't have to define a macro with any value. Instead you can use just a macro as a Boolean 
expression to represent a feature.  

* directive use for Booolean Compilation Conditions
* Syntax
    
    * ``#define <FEATURE-NAME>``

* Examples:
  
  .. code-block:: c
     :caption: macro as feature

     /* Define feature for the MSP */
     #define MSP_PLATFORM

     #define TEN      (10)
     /* Undefine the Constant TEN */
     #undef  TEN

     #define KL25_PLATFORM
     /* Undefine the Feature */
     #undef KL25_PLATFORM

|
|


``#if``, ``#else`` Directive and other Conditional Directive
================================================================

..
    .. container:: twocol

    .. container:: leftside
        
        ``#if...else`` Directive
        
        * will be used to perform conditional operations at compile time.
        * Conditionally compile blocks of code
        
            #. ``#ifdef``
            #. ``#ifndef``
            #. ``#elif``
            #. ``#else``
            #. ``#endif`` - End of block (**required**)
        
                - similar to ``}`` of c-style of if-else block.
        
        * Useful for debugging
        * "Turn off" large amounts of code

    .. container:: rightside
        
        .. code-block:: c
            
            #define COMPILE_CODE
            …
            #ifdef COMPILE_CODE
                // Code will be compiled
            #endif

            #define DO_NOT_COMPILE_CODE
            …
            #undef DO_NOT_COMPILE_CODE
            #ifdef DO_NOT_COMPILE_CODE
                // Code will be NOT be compiled
            #endif
            

.. list-table::
   :class: my-class
   :name: my-name

   * - ``#if...else`` Directive
       
       * will be used to perform conditional operations at compile time.
       * Conditionally compile blocks of code 

           #. ``#ifdef``
           #. ``#ifndef``

            - provides a negative test
            - checks if macro is not defined by #define. If yes, it executes the code.
            - syntax::

                #ifndef MACRO  
                //code  
                #endif  

           #. ``#elif``
           #. ``#else``
           #. ``#endif`` - End of block (**required**) 
            
            - similar to ``}`` of c-style of if-else block. 
      
       * Useful for debugging
       * "Turn off" large amounts of code
       
     - .. code-block:: c
          
          #define COMPILE_CODE
          …
          #ifdef COMPILE_CODE
               // Code will be compiled
          #endif
          
          #define DO_NOT_COMPILE_CODE
          …
          #undef DO_NOT_COMPILE_CODE
          #ifdef DO_NOT_COMPILE_CODE
               // Code will be NOT be compiled
          #endif

#if-else directive example

.. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v3_PreprocessorDirectve_CompileTimeSwitch.c
   :caption: ``if`` ``#else`` directive
   :language: c
   :linenos:



* In line ``5``, f the feature macro, MSP_PLATFORM is defined, the preprocessor will include a 
  specific MSP startup routine. 
* On the other hand, if the Kinetis feature is defined, then the Kinetis start up routine will be included 
  and compiled. 
* If neither of these are defined, then we'll use another preprocessor directive to alert the 
  engineer that an error must have occurred and compilation should be stopped
   


``#include`` Directive
========================

the #include directive, 

* is used to include and reference other files where you may have software routines in.
* When you do a #include, the preprocessor actually does a similar copy and paste that we saw at the 
 ``#define`` directive
    
    * it does it with the whole contents of the header file you were including from.

* Declarations get copied into file.
    
    * Copies the header file contents into the top of file where ``#include`` preprocessor directive 
      was written.
    * There are other information that is added as well.

* Examples:

    - include file from local directory

        * ``#include “uart.h”``
    
    - Include file from a library path or include path:
	    
        - #include <stdio.h>


Example of #include substitution

.. list-table::
   :class: my-class
   :name: my-name
   :widths: 30 20 30

   * - .. code-block:: c
          :caption: my_file.h
          :linenos:
          :emphasize-lines: 1-2

          #define LENGTH (10)
          void clear(char * ptr, int size);

       .. code-block:: c
          :caption: my_file.c
          :linenos:
          :emphasize-lines: 1-2
          
          #include “my_file.h”
          char arr[LENGTH];
          
          void clear(char * ptr, int size){
           int i;
            for(i = 0, i < size, i++){
              ptr[i] = 0;
            }
          }
 
     - | 
       |
       |
       |
       |
       |
       |
       |
       |

        .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
          :page-index: 7
       
     - |
       |

        .. code-block:: c
           :caption: my_file.i
           :emphasize-lines: 1, 3
           :linenos:
           
           void clear(char * ptr, int size);
           
           char arr[10];
           
           void clear(char * ptr, int size){
             int i;
             for(i = 0, i < size, i++){
               ptr[i] = 0;
             }
           }
          

We have 2 files

* a header file ``.h``

    - which contains a macro and a function declaration.

* an an implementation file ``*.c``

    - which contains a function definition, an array declaration and an include statement for the 
      header file
    
    - when you pass the .c file through the preprocessor, you will see similar preprocessor output
      as in the right


The ``#error`` directive
===========================

The #error preprocessor directive 

* indicates error. 

    - The compiler gives fatal error if #error directive is found and skips further compilation process. 

    .. code-block:: bash
       :caption: #error

       #include<stdio.h>  
       #ifndef __MATH_H  
       #error First include then compile  
       #else  
       void main(){  
           float a;  
           a=sqrt(7);  
           printf("%f",a);  
       }  
       #endif
       


The ``#pragma`` Directive
===========================

The ``#pragma`` directive

* is used to provide specific instructions for the compiler from the code itself and not 
  the command line.
    
* The #pragma directive is used by the compiler to offer machine or operating-system feature

* Gives a specific instruction to the compiler
    
    - Controls compilation from software instead of command line

* This type of directives are compiler-specific, i.e., they vary from compiler to compiler. 

* Adds options to compiler for specific function
    
    * ``#pragma GCC push_options``

* Causes an error during compilation if code uses these functions
	
    * ``#pragma GCC poison printf sprint fprintf``

* Compile a function with a specific architecture::

	#pragma GCC target (“arch=armv6”)  -or-
	                   (“cpu=cortex-m0plus”)


**++** Pragmas are important if you want to provide a specific option for a specific piece of 
code that general compile options should not do.

An example:

.. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v3_PreprocessorDirectives_pragmaEx.c
   :language: c 
   :linenos:
   :emphasize-lines: 3

.. code-block:: bash
   :caption: output

   # gcc main.c -o main.out

   main.c: In function ‘main’:
   main.c:7:5: error: attempt to use poisoned "printf"
       7 |     printf("Hello world! \n"); // Std-library function call
         |     ^



Predefined Macros
===================

* ANSI C defines a number of macros. Although each one is available for use in programming, 
  the predefined macros should not be directly modified.

* They fall into three classes: 
    #. standard predefined macros, 
    #. common predefined macros, and 
    #. system-specific predefined macros.


.. list-table:: Standard Predefined Macros
   :header-rows: 1
   :class: my-class
   :name: my-name

   * - Macro
     - Description

   * - ``__DATE__``
     - Expand the current date at compile time as a character literal in 
       "MMM DD YYYY" format. (eg. "Dec 17 1995")


   * - ``__TIME__``
     - The current time as a character literal in "HH:MM:SS" format.


   * - ``__DATE__``
     - The current date as a character literal in "MMM DD YYYY" format.


   * - ``__FILE__``
     - This contains the current filename as a string literal.


   * - ``__LINE__``
     - This contains the current line number as a decimal constant.

   * - ``__STDC__``
     - Defined as 1 when the compiler complies with the ANSI standard.


Example:

.. code-block:: c
   :caption: pre-defined macros example, "main.c"

    #include <stdio.h>

    int main() {

       printf("File :%s\n", __FILE__ );
       printf("Date :%s\n", __DATE__ );
       printf("Time :%s\n", __TIME__ );
       printf("Line :%d\n", __LINE__ );
       printf("ANSI :%d\n", __STDC__ );

    }

.. code-block:: bash
   :caption: pre-defined macros output

   File :main.c
   Date :Sep 19 2022
   Time :16:17:57Line :8
   ANSI :1
   

Compile Time Switch using Directive
===============================================

Compile time switch

* is a method of controlling **WHAT** content in your source files is compiled

    * Uses combination of #if-else and ``#define`` directives
    * .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v3_PreprocessorDirectve_CompileTimeSwitch.c
         :caption: ``if`` ``#else`` directive
         :language: c
         :linenos:
    
    *  .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
          :page-index: 8
          :width: 800
          :height: 500

* Syntax: add extra option ``-D`` with a macro name immediately following (no space) to gcc command.

    - ``-D<MACRO-NAME>``
    - Ex: ``$ gcc -DMSP_PLATFORM -o main.out main.c``

* By providing a specific option and compile time, the compiler will integrate architecture 
  specific code into an executable.


**++** Compile time switches allow us make our software more portable without maintaining 
different software repositories

* a compile time switch can allow us to use the same code for different embedded platforms

**A visual illustration of Compile time switches**

Imagine you have a handful of different embedded systems running the same applications

* Some could have two different versions of the processor or maybe a different micro-controller 
  all together

* You can see here are two hardware platforms with similar software except for the UART library

    .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
        :page-index: 9
        :width: 800
        :height: 500

    - Regardless of the platform, the code is in the same software repository. We then have some 
      preprocessor directives in a device library file and this chooses the right platform file 
      to include.

            .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
               :page-index: 10
               :width: 800
               :height: 500

* Based on a compile time switch or defined parameters, we can include a specific firmware library 
  for interacting with UART.

.. tabs::
    
    .. tab:: KL25z_PLATFORM Select
        
        .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
           :page-index: 11

    .. tab:: MSP_PLATFORM Select

        .. drawio-image:: ../../_images/src/Coursera-IES-W2_3_PreprocessorDirectives.drawio
           :page-index: 12
         

- This design has provided a very portable top layer of software with a very flexible firmware 
  layer that different versions of code can be compiled given an intended platform


Some Pros and Cons of using directives
========================================


.. list-table:: Pro and Cons of Directives
   :header-rows: 1
   :class: my-class
   :name: my-name

   * - Pros
     - Cons

   * - Pros
       
       #. They do offer less overhead and greater improvements for writing maintainable and 
          readable programs. 
       #. Help in design portability and platform independence into your software.


     - Issues associated with using Directives

       #. They don't perform type checking. 
       #. They can potentially increase bugs. 
       #. And they definitely increase your code size. 


.. note::

    There have been some new C features in newer C standards that provide similar functionality. 
    But macros are still widely used in software projects, especially with helping to design 
    portability and platform independence into your software.

   

****************
References
****************

#. https://www.geeksforgeeks.org/cc-preprocessors/
#. https://developerinsider.co/preprocessor-directives-c-programming/
#. https://www.tutorialspoint.com/cprogramming/c_preprocessors.htm
#. https://gcc.gnu.org/onlinedocs/cpp/Predefined-Macros.html


glossary
===========

.. glossary::

    preprocessor
        A preprocessor is a macro processor that is that is used automatically by the C compiler to transform your 
        program before actual compilation.

    :term:`macros`
        Macros are pieces of code in a program that is given some name. Whenever this name is 
        encountered by the compiler, the compiler replaces the name with the actual piece of code.