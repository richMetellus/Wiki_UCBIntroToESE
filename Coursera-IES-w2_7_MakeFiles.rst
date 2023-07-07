###################
MakeFiles
###################

********
Rules
********

Implicit Rules
===============

Implicit Rules

* *Implicit rules* tell make how to use customary techniques so that you do not 
   have to specify them in detail when you want to use them.
* Certain standard ways of remaking target files are used very often. 
    
    * For example, one customary way to make an object file is from a 
      C source file using the C compiler, *cc*.

Type of implicit rules:

* Built-in rules
* Pattern rules
* Suffix rules

Suffix rules are a more limited way to define implicit rules. 
Pattern rules are more general and clearer, but suffix rules are retained for 
compatibility. [GNU_ImplicitRule]_


Built-in Rules [GNU_CAT_BR]_
--------------------------------

Built-in Rules are predefined implicit rule which are always available 
unless the makefile explicitly overrides or cancels them.

Ex of Built-in Implicit Rules:

1. Compiling C programs
    
    *n.o*  is made automatically from *n.c* with a recipe of the form 
    
    '$(CC) $(CPPFLAGS) $(CFLAGS) -c'.

Looking at the the catalogue [GNU_CAT_BR]_, the built-in rules are a bunch
of convention that *make* follows.

* Rules like convention to produce executable
* convention to produce object files (*n.c* or *n.cc* or *n.cpp* become *n.o*)



***************************************
Variables and Assignment Operator
***************************************

There are different ways that a variable in GNU make can get a value; 
we call them the *flavors* of variables

Flavors of variables

* Recursively Expanded Variable Assignment

    * Variables of this sort are defined by lines using ``=``
    * The value you specify is installed verbatim; if it contains references 
      to other variables, these references are expanded whenever this variable 
      is substituted
    * using *recursive expansion*
      
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

         
* Simply Expanded Variable Assignment
* Immediately Expanded Variable Assignment
* Conditional Variable Assignment


Automatic Variables
=====================


**************
References
**************

.. [GNU_ImplicitRule] https://www.gnu.org/software/make/manual/html_node/Implicit-Rules.html 
.. [GNU_CAT_BR] `GNU Catalogue of Built-in Rules <https://www.gnu.org/software/make/manual/html_node/Catalogue-of-Rules.html>`_
