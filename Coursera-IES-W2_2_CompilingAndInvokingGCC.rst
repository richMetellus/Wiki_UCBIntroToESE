.. include:: ../global.rst

#############################################################
Compiling And Invoking GNU Compiler Collection (GCC)
#############################################################

****************
Objectives
****************

Objectives

#. Learn about GCC to compile/build a software project.
   
   - Building and compiling are often used interchangeably because by definition, compilation is the
     process of converting high level software language to architecture specific binary encoded 
     operations.

      - :strike:`Some engineers might include the process of linking into this definition.`

#.  Gain experience with each of the five steps of building a software project

..
    slide

****************
Slides
****************

.. .. raw:: html
   
  ..  <!DOCTYPE html>
  ..  <html>
   
  ..  <head>
  ..      <title>Compiling and Invoking GCC</title>
  ..  </head>
   
  ..  <body>
  ..      <p><a href="https://docs.google.com/presentation/d/1e_uSpeoBAKdg7hGevjOKvgs4rdel6fBfeEhVtOvRPZ8/"
  ..              target="iframe_intro_to_embedded">Start Slide at the Beginning</a></p>
  ..      <center>
  ..      <iframe name="iframe_intro_to_embedded" frameborder="0" scrolling="no" width="800px" height="600px" src="https://docs.google.com/presentation/d/1e_uSpeoBAKdg7hGevjOKvgs4rdel6fBfeEhVtOvRPZ8/">
  ..      </iframe>
  ..      </center>
  ..  </body>
   
  ..  </html>
   

****************************
**Transcribed Note**
****************************

**Build Process (linear)**

.. drawio-image:: ../../_images/src/Coursera-IES_W2_1_BuildProcess.drawio.svg

Recall from previous videos, building a software project requires five main steps: 

#. pre-processing, 
#. compiling, 
#. assembling, 
#. linking, and 
#. locating.

We will invoking command line applications to run each of these steps.

Sometimes the steps of building might be combined into a single application
depending on your compiler toolchain.

  .. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio

- :strike:`GCC Toolchain` GNU combines pre-processing and compiling into the application called ``gcc``.
- The assembler is the application ``as``
- The linker and locator are combined into an application called ``ld``

**++** you can perform the full build by just invoking ``gcc`` directly

GCC Tool Check
===================

.. sidebar::
  
  Machines can install many compiler toolchains. Some of these GCC compilers have very specific 
  information about an architecture, vendor, OS, or even the ABI in the name
  in this format ``<ARCH>-<VENDOR>-<OS>-<ABI>-gcc``

  Example:

  * ``arm-none-eabi-gcc``

    - Arch    = Arm
    - Vendor  = N/A
    - OS      = None (Bare-Metal)
    - ABI     = AEABI

  * ``arm-linux-gnueabi-gcc``

    - Arch    = Arm
    - Vendor  = N/A
    - OS      = Linux OS
    - ABI     = GNUEABI

  on My yocto build VM, I have the following::

    ssyscs@centos8StreamTarponBuildVM:~$ ls -l /usr/bin/*gcc
    -rwxr-xr-x. 3 root root 1262456 Jul 22 11:10 /usr/bin/gcc
    -rwxr-xr-x. 3 root root 1262456 Jul 22 11:10 /usr/bin/x86_64-redhat-linux-gcc

    ssyscs@centos8StreamTarponBuildVM:~$ gcc --version
    gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-15)

  
  on My f21arm VM

    - Explicitly there is no cross-compile toolchain but the yocto sdk provide the toolchain
      
      .. code-block:: bash

         root@f21-arm:~# ls -la /usr/bin/*gcc
         -rwxr-xr-x. 2 root root 858544 Feb 12  2015 /usr/bin/gcc
         -rwxr-xr-x. 2 root root 858544 Feb 12  2015 /usr/bin/x86_64-redhat-linux-gcc
         root@f21-arm:~# ls -la /usr/bin/ar
         ar           arch         arecord      arecordmidi

         root@f21-arm:~# gcc --version
         gcc (GCC) 4.9.2 20150212 (Red Hat 4.9.2-6)


         root@f21-arm:~# echo $PATH
         /opt/Xilinx/Vivado/2016.3/bin:/usr/lib64/ccache:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
         /opt/microchip/xc16/v1.23/bin:/opt/mxe/usr/bin:/opt/Xilinx/SDK/2016.3/bin:
         /opt/Xilinx/SDK/2016.3/gnu/microblaze/lin/bin:/opt/Xilinx/SDK/2016.3/gnu/arm/lin/bin:
         /opt/Xilinx/SDK/2016.3/gnu/microblaze/linux_toolchain/lin64_be/bin:
         /opt/Xilinx/SDK/2016.3/gnu/microblaze/linux_toolchain/lin64_le/bin:
         /opt/Xilinx/SDK/2016.3/gnu/aarch32/lin/gcc-arm-linux-gnueabi/bin:
         /opt/Xilinx/SDK/2016.3/gnu/aarch32/lin/gcc-arm-none-eabi/bin:
         /opt/Xilinx/SDK/2016.3/gnu/aarch64/lin/aarch64-linux/bin:
         /opt/Xilinx/SDK/2016.3/gnu/aarch64/lin/aarch64-none/bin:
         /opt/Xilinx/SDK/2016.3/gnu/armr5/lin/gcc-arm-none-eabi/bin:
         /opt/microchip/xc16/v1.23/bin:/opt/Xilinx/14.7/ISE_DS/EDK/gnu/microblaze/lin/bin:
         /opt/Xilinx/14.7/ISE_DS/EDK/bin/lin:/opt/Xilinx/14.7/ISE_DS/ISE/bin/lin:
         /root/bin:/opt/microchip/xc16/v1.23/bin




**Tools needed:**

* gcc -> gcc-4.8

  - for code run on the host machine.

* ``arm-none-eabi-gcc``
  
  - for code run on the target processor.
  - need this tool to investigate the architecture specifics of the assembly code we produce.

**++** In general, you can see all the tools of the cross toolchain by running the 
command 

* ``ls -la /usr/bin/arm-none-eabi*``
* You can verify the gcc version by running ``gcc --version``.
* Run the command ``which gcc`` to see the location of the gcc binary.
* You can check the man page of a particular command if it is provided by running ``man <program-command>``
  ex: ``man gcc``


Typical Build Process (non-Linear)
======================================

This simplified linear install process is not typical for a software project. Usually, you'll
have a more complex environment with lots of C files, assembly files, and library files coming together.

  - your library files usually have a .a or a .lib.

|
|

.. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
   :page-index: 1

**++** Depending on how particular you control your build system, the underlying GCC programs can be 
used to invoke different parts of the build steps.

In this example,

.. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
   :page-index: 2
   :width: 800
   :height: 500

* the assembler (as) and linker (ld) will be the entry point for some of the specific project files 
  into our overall build generation

**++** The underlying GCC program can be used to invoke different parts of the build steps in
another manner

* For instance, GCC can be used for wrapping assembly or C file compilation.

  * .. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
       :page-index: 3
       :width: 800
       :height: 500 

**++** GCC could also pass control down to the linker file without having to invoke the linker(ld) 
directly.

* This means that all necessary options must be passed on to gcc and down to each stage of 
  the build process. 

* .. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
     :page-index: 4


Compilation Proper
---------------------

| C code gets converted into architecture specific assembly.
| This conversion is not a one to one mapping of lines but instead a decomposition of C operations 
| into numerous assembly operations.


 .. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
    :page-index: 5
    :format: svg

Assembly to Machine Code
----------------------------

.. drawio-image:: ../../_images/src/Coursera-IES_W2_2_CompilingInvokingGCC.drawio
  :page-index: 6

* The binary coded operations are referred to as operation codes, or op-codes
  
  * Some op-codes also have some associated information about the operands, or the data, they are operating on.


Coding Example and Compilation
================================


Let us now interact with the tools to compile a simple hello world executable. In this example, 
we are going to use ``printf``, but only for the case of an example. 

.. important::

   As we gain experience with writing embedded software we will not arbitrarily include library functions. 
  
   - Embedded systems have limited memory and processing resources so you want to make sure that 
     pre-compiled library code does not impede your efficiency as you can not modify them.

   - Also while library functions might be run time optimized, there's often hardware that we can 
     use to improve performance even more. We will want to make sure we use only optimized code for 
     our application.

**++** gcc command will have the following form ``gcc [options] <file-you-want-to-compile>``

**General Compiler Flags**

* .. csv-table:: GCC Flags
     :file: ../../_resources/Courses/UCBoulder-ESE/c1m2v2_GeneralGCCFlags.csv
     :header-rows: 1
     :widths: 30, 70

|

Let's now write and compile a basic application

#. Write a file ``main.c`` with the following content
   
   .. literalinclude:: ../../_resources/Courses/UCBoulder-ESE/c1m2v2_main_helloWorld.c
      :language: c
      :linenos:

#. compile with the following ``gcc -std=c99 -Werror -o main.out main.c``
  
  - This command compile and link our single input file ``main.c`` , using a ``c-standardization c99``,
    and use the ``-o`` option to store the output executable as ``main.out``
  
  - The ``-Werror`` flags option causes compilation to fail if any warnings occurs.

    - This is an important flag for quality driven software. We can use this to prevent developers
      from building software without resolving all of their warning messages first.
    
    - Warnings can sometimes be resolved by the compiler, but this could introduce a very subtle bug. 

#. Run the program with ``./main.out`` and "Hello World!" should be printed.

Let's compile with different option

4. Compile for c89 standard, no linking

   .. code-block:: bash
      :caption: compile, no linking

      # rm -f ./main.out
      # gcc --std=c89 -Wall -v -c main.c -o main.o

   - ``-Wall`` flags means all warnings are considered warnings and not as errors.
   - ``-v`` verbose. This tell the application to output more logging information.
   - ``-c`` cause the program to compile or assemble, but stop before linking

.. _objectfile:

#. An :term:`object file` ``main.o`` will be created
   
   .. note::
      
      A very important point to note about this output file is that when we use GCC this way, 
      it automatically searches it's own libraries and includes them as part of the compilation 
      process.
      
      * in our case, since we imported/included the libary file <stdio.h> to use printf, the standard io library 
        will be included and that library exists in one of the GCC included paths.

          - see `difference between header file vs Library file <https://www.differencebetween.com/difference-between-header-file-and-vs-library-file/#:~:text=The%20difference%20between%20a%20header%20file%20and%20library%20file%20is,functions%20in%20the%20header%20file.>`_
          
      .. seealso::
          
          * :ref:`C Libraries and Module <CLibrariesAndModules>`

            * this really explains a lot about how to create libaries and the
              type of libaries (static vs shared(dynamic))
      
      .. seealso::

         * I have made reference on programming language and
           the glibc (GNU C Library) 
           :ref:`What is Programming and How Is a programming Language created <ProgrammingIntro>`
             
            * :ref:`standard lib section <standardLibrary>`
      
**Important questions to ask yourself about the execution of this program**

* Ask yourself how is ``main()`` being invoked and where does it return to after main is done?
   
   .. important::
      
      ``main()`` is not actually the entry point into your executable. There is initialization code  
      that is compiled and run before main(), and it's responsible for invoking main(). 
      After main() returns, this library code cleans up your application and exits.



**Cross-compiling main.c**

#. Use the cross-compiler with the same name file from before, with some architecture specific flags.
   
   .. code-block:: bash
      
      $ arm-none-eabi-gcc main.c -o main.out --spec=nosys.specs -std=c99 -mcpu=cortex-m0plus -mthumb -Werror
   
   - In this example, we compile and link for arm


Here's a list of some of those important architecture specific flags that we use for ARM processors.

* .. csv-table:: ARM Architecture Specific Compiler Flags
     :file: ../../_resources/Courses/UCBoulder-ESE/c1m2v2_ArchitectureSpecificCompilerFlags.csv
     :header-rows: 1
     :widths: 30, 70

* These architecture flags are important for informing the compiler and the linker about specific 
  libraries to include as well as the target architecture in cpu to cross compile for.

**++** If I try to run this executable ``main.out`` that was cross-compile it will fail because 
of an Exec format error.

* However we can use the cross compiler ot give us some insight into a translated assembly of our
  C program
  
  .. code-block:: bash
     
     $ arm-none-eabi-gcc -std=c99 -mcpu=cortex-m0plus -mthumb -Wall -S main.c -o main.s

  - stop GCC at compilation proper stage with the ``-S`` option.

  - add the ``-g`` at the end of the command above, this will add debug symbols and add a lot more
    information we can help us debug executables later.