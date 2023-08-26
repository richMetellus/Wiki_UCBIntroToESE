###################
MakeFiles
###################

Automating the build process is just like writing software.

**Objectives**

1. Write a makefile that store all configurations, flags, and commands to 
   generate the build.
    
    .. raw:: html
       
          <iframe 
            src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p3" 
            frameborder="0" width="100%" height="500" 
            allowfullscreen="true" 
            mozallowfullscreen="true" 
            webkitallowfullscreen="true">
          </iframe> 

2. Version control your makefiles 
    
    * This allows us to record in our repo history exactly what we built with 
      our software projects at any particular command
    * Do not version control the output(s) of the makefiles.


    .. raw:: html
    
       <iframe 
         src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p4" 
         frameborder="0" width="100%" height="500" 
         allowfullscreen="true" 
         mozallowfullscreen="true" 
         webkitallowfullscreen="true">
       </iframe> 

       
Makefiles and Syntax
*********************

Make is given direction on what to compile via one or more **makefiles**.

.. raw:: html

    <iframe 
      src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p5" 
      frameborder="0" width="100%" height="500" 
      allowfullscreen="true" 
      mozallowfullscreen="true" 
      webkitallowfullscreen="true">
    </iframe> 

* You could give your makefile any name as long as you pass the file into make, 
  but there are some defaults names that GNU make looks for:
    
    * `makefile`, `Makefile`, `sources.mk`, `includes.mk`

* Each makefile may consistently set up **build target** or **build rules** that 
  generate outputs

* The syntax and the typical contents are as follow:

    .. raw:: html
   
        <iframe 
          src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p7" 
          frameborder="0" width="100%" height="500" 
          allowfullscreen="true" 
          mozallowfullscreen="true" 
          webkitallowfullscreen="true">
        </iframe> 
      
    * create variables to make your makefile more readable and portable by 
      reusing the same variable names instead of retyping all the contents.

    * You can define many rules with many command recipes. **In each command in 
      the recipe must start with a tab**. 
    
Deeper Dive in Makefile Content 
*********************************

Makefile Rules
==================

.. raw:: html

    <iframe 
      src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p6" 
      frameborder="0" width="100%" height="500" 
      allowfullscreen="true" 
      mozallowfullscreen="true" 
      webkitallowfullscreen="true">
    </iframe> 

* **In each command in  the recipe must start with a tab**. 


Implicit Rules
---------------

Implicit Rules

* *Implicit rules* tell make how to use customary **standard** techniques so 
  that you do not have to specify them in detail when you want to use them.
    
    * For example, one customary way to make an object file is from a 
      C source file using the C compiler, ``cc``.
    
    * The compiler will take any file ending in *.c* file and make  *.o* file is
      another example of implicit rul

Type of implicit rules:

* Built-in implicit rules
    
    * they might use variable like *CFLAGS* to control the flags giving to the 
      C compiler
    * use **CPPFLAGS** to control the flags giving to the C++ compiler

* Pattern rules

    * You use pattern rules to create your own implicit rules

* Suffix rules

Suffix rules are a more limited way to define implicit rules. 
Pattern rules are more general and clearer, but suffix rules are retained for 
compatibility. [GNU_ImplicitRule]_


Built-in Rules [GNU_CAT_BR]_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Built-in Rules are predefined implicit rule which are always available 
unless the makefile explicitly overrides or cancels them.

Ex of Built-in Implicit Rules:

1. Compiling C programs
    
    *n.o*  is made automatically from *n.c* with a recipe of the form 
    
    ``$(CC) $(CPPFLAGS) $(CFLAGS) -c`` .

Looking at the the catalogue [GNU_CAT_BR]_, the built-in rules are a bunch
of convention that *make* follows.

* Rules like convention to produce executable
* convention to produce object files (*n.c* or *n.cc* or *n.cpp* become *n.o*)




Variables and Assignment Operator
==================================

.. raw:: html

    <iframe 
      src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p15" 
      frameborder="0" width="100%" height="500" 
      allowfullscreen="true" 
      mozallowfullscreen="true" 
      webkitallowfullscreen="true">
    </iframe> 

There are different ways that a variable in GNU make can get a value; 
we call them the *flavors* of variables

Flavors of variables

* **Recursively Expanded Variable Assignment**
* **Simply Expanded Variable Assignment**
* **Immediately Expanded Variable Assignment**
* **Conditional Variable Assignment**

Recursively Expanded Variable Assignment
------------------------------------------

* **Recursively Expanded Variable Assignment**

    * Variables of this sort are defined by lines using ``=``
    * The value you specify is installed verbatim; if it contains references 
      to other variables, these references are expanded whenever this variable 
      is substituted using *recursive expansion*
      
      .. code-block::
         
         foo = $(bar)
         bar = $(ugh)
         ugh = Huh?
         
         all:;echo $(foo)
         
         
      *foo* will be equal to ``Huh?`` since ``$(foo)`` expands to ``$(bar)`` which
      expand to ``$(ugh)`` which finally expands to ``Huh?``
      
        .. admonition:: Rich's Thoughts

            * R. The variable reference are then substituted so foo is ``Huh?``
            * R. this is a good example to show that Makefile statements are not technically 
              executed sequentially (direct call of a specific target is another
              example ``make main.out``)
    
    * Advantages and disadvantages:
      
      .. list-table:: Advantages and Disadvantages of *Recursively Expanded Var*
         
         * - Advantages
           - Disadvantages
         
         * - only sort supported by most other versions of *make* (widely supported)
           - ..
         
         * - Do exactly what it supposed to do (replace verbatim)
             
             .. code-block:: bash
                
                CFLAGS = $(include_dirs) -O
                include_dirs = -Ifoo -Ibar

             ``CFLAGS`` will expand to ``Ifoo -Ibar -O``

           - you cannot use the same variable to append to itself (infinite loop in variable 
             expansion problem and make report an error)

             you cannot do something like ``CFLAGS = $(CFLAGS) - O``

         * - ..
           - can *make* run slower
             
             Any functions referenced in the definition will be executed every 
             time the variable is expanded.
             
             * *wildcard* and *shell* function can give unpredictable results
               because you cannot control when they are called or even how many 
               times.

    * An example of where recursively expanded variable might be used are the
      variable that may be used to point to all the "includes" and "sources".
      You can actually create variables that point to your source files and 
      include paths. This allows us to make even more generic rules for 
      compiling based on these variables.
        
        .. raw:: html

           <iframe 
             src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p16" 
             frameborder="0" width="100%" height="500" 
             allowfullscreen="true" 
             mozallowfullscreen="true" 
             webkitallowfullscreen="true">
           </iframe>  
        
        * Notice how the list is a space separated list, and for the ``INCLUDES`` variable,
          each list element begins with ``-I``followed by the path of the include
          directory.

Simply Expanded Variable Assignment
------------------------------------

* **Simply Expanded Variable Assignment**

    * are defined by lines using ``:=`` or ``::=``
    * Expands once at the time of definition. 
    * This is more like regular programming
      ``=`` assignment in language like C, python.

    * example,
        
        .. list-table::
               
            * - example

                .. code-block:: C
            
                   x := foo
                   y := $(x) bar
                   x := later

              - Equivalent to  

                .. code-block:: C
            
                   y := foo bar
                   x := later
                
                * Notice, x does not expand again, and take the last value it was 
                  assigned. Just like sequential programming.

Immediately Expanded Variable Assignment
---------------------------------------------


* **Immediately Expanded Variable Assignment**
    
    * This type of assignment uses the ``:::=`` operator

Conditional Variable Assignment
---------------------------------

* `Conditional Variable Assignment <https://www.gnu.org/software/make/manual/html_node/Conditional-Assignment.html>`_
    
    * use the ``?=`` operator
    *  This is called a conditional variable assignment operator, because it 
       only has an effect if the variable is not yet defined


Automatic Variables
--------------------

`Automatic variables <https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html>`_

* are variable for which the value is generated by Make, rather than having to
  be set explicitly.

* they are the following:

    * ``$@`` - expand to the file name of the target
    * ``$^`` - space separated list  of all pre-requisites of the target rule
    * ``$<`` - expand to the first pre-requisite of the target rule.

.. seealso:: chatGPT answer on automatic variable on MK-0099... file.


.. raw:: html

    <iframe 
      src="https://docs.google.com/presentation/d/1wgP4U2TOopTy6fhxuMSD0b9xyAaMJO6RsM4FoVtdom4/embed?start=false&loop=false&slide=id.p17" 
      frameborder="0" width="100%" height="500" 
      allowfullscreen="true" 
      mozallowfullscreen="true" 
      webkitallowfullscreen="true">
    </iframe>  

**************
References
**************

.. [GNU_ImplicitRule] https://www.gnu.org/software/make/manual/html_node/Implicit-Rules.html 
.. [GNU_CAT_BR] `GNU Catalogue of Built-in Rules <https://www.gnu.org/software/make/manual/html_node/Catalogue-of-Rules.html>`_
