.. include:: /global.rst

################################################################################################################################################################################
`W1 Lesson 2 Video 8: Development Kits and Documentation <https://www.coursera.org/learn/introduction-embedded-systems/lecture/rk0h9/8-development-kits-and-documentation>`_
################################################################################################################################################################################

****************
Objectives
****************

1. How to look at the manufacture and 3rd party hardware dev kits and important 
   documentation to configure your microcontroller.

2. Development kits and Evaluation kits

You'd hope your hardware has no bug that stall your software progress. 
For this we use:

- Development Kits

    - Development kits are use to evaluate platform options


..
    slide

****************
Slides
****************

.. .. raw:: html
   
..    <!DOCTYPE html>
..    <html>
   
..    <head>
..        <title>Discovery kit and Documentation</title>
..    </head>
   
..    <body>
..        <p><a href="../../source/Embedded_System/_resources/Coursera-IES-BoulderColorado_Lecture_slides/Coursera-IES-W1_8_DevelopmentKitsandDocumentation_C1-M1-V8-Slides.pdf"
..                target="iframe_intro_to_embedded">Open Lessons Slides in target iframe</a></p>
..        <center>
..        <iframe name="iframe_intro_to_embedded" frameborder="0" scrolling="no" width="60%" height="600px" src="">
..        </iframe>
..        </center>
..    </body>
   
..    </html>
   

****************************
**Transcribed Note**
****************************

Silicon Manufacturers
=============================

- Microcontroller chips are a type of

    - **Integrated Circuit (IC)** --or--
    - **Application-Specific Integrated Circuit (ASIC)**

- Example of Silicon Chip Manufacturers

    - Silicon Labs, Texas Instruments, Atmel, NXP Semiconductors



When it comes to pick a chip, there are factors before committing a product to a particular chip choice


- :strike:`Inside the IC, there's also a chance that different parts of silicon were built by different 
  people`
        
        - :strike:`Arm is a good example of that`

            - :strike:`Arm build cpu cores and other technologies for many silicon manufactures`  

        - .. image:: ../../_images/W1_8_ARM_Cortex-M0+_insideNXP_MKL25Z_chip.PNG
             :scale: 50%




Picking a Chip/microcontroller
================================

Microcontrollers have many features that you need to know about before making a selection,
especially because there will be software implications. **Some important features include:**

- the word size, (8-bits, 16-bit, 32-bit)
- the number of registers, 
- flash and RAM sizes, 
- branch prediction support, 
- instruction and data cache support,
- Floating point arithmetic support, 
  or DMA support.

**Questions to ask before picking a micro**

- How much memory will I need for my application?
- How fast does the application need to run?
- What kind of mathematical support do I need?

The process of creating this feature and operation requirements checklist is 
called a specification, or a features spec.

- Always evaluate your initial design correctly at the beginning of your project, 
  that includes prototyping and proof of concepts, otherwise in the middle of the 
  design process you might have to make significant software/hardware changes.

**Find Information During the Chip Selection Process**

How to pick out a platform or evaluate a particular chip

Make Use of Selector Guide
-----------------------------

.. image:: ../../_images/W1_8_KL2x_Devices_Selector_Guide_Example.PNG
   :scale: 75%

Many manufacturers provide selector guide, which help narrow down choices by interactively
selecting feature for our design.

Make Use of Product Brief
----------------------------------

If you find some interesting parts from the selector guide, you can obtain some 
more information using the product brief.

The product brief:

- gives a concise overview for quick evaluation of a platform for design with more details
- more market, and less dense tha strict product specifications
- Talks about use cases
- is nice on the eyes

Once you have slimmed down your selections to a subset of models, you can start getting into 
much more detailed documentation sources like the datasheet.

The Datasheet
-------------------------

Datasheet

- is a document that gives technical specifications on a chip or a family of chips with a 
  breakdown of all the differences between each version or part number within the family. 
- contains details Technical Specifications
    
    - Electrical

        - gives voltage and current operating rate, where we get information on power specs of 
          various conditions and operating modes.
        - .. image:: ../../_images/W1_8_ElectricalTechnicalSpecification.PNG
             :scale: 75%

    - Timing
        - .. image:: ../../_images/W1_8_TimingTechnicalSpecificationExample.PNG
             :scale: 75%
        - Timing characteristics are provided for our operation of the microcontroller.
        - You will find info on the limit of the processor's clock frequencies. 
        - There are timing diagrams that show expected time delays before certain digital signals are asserted. 
            
            - These quantities are usually measured in micro or nano seconds. 

        - Timing characteristics are very important for a software engineer to know
            about so they may predict or calculate runtime requirements and performance

    - Environmental

        - .. image:: ../../_images/W1_8_EnvironmentalTimingSpecification.PNG
             :scale: 75%
        - The datasheet even includes some extra data on how environmental effects, 
          such as temperature, can affect the device or the operation characteristics.

    - Physical Package

        - .. image:: ../../_images/W1_8_PhysicalPackagingTechnicalSpecification.PNG
             :scale: 75%
        - Detail the pinpoint mappings for a peripheral device you might use.
        - This is a table that describes which pins match up to the general IO ports of the microcontroller, 
          and subsequently, which pins can connect to certain peripheral devices. 

**The datasheet does not typically provide information on configuration, Use the 
Family Technical Reference Manual**

The Family Technical Reference Manual 
---------------------------------------

Family Technical Reference Manual. 

- this manual describes information on general platform components and configuration. It contains:

    - Configuration details
    - Operation Description
    - .. image:: ../../_images/W1_8_FamilyReferenceManualSampleEx.PNG
         :scale: 75%

- You may then have a sub-family reference manual which covers particular settings 
  of a specific device. 
- These are manuals that you need to look up to know how you go about configuring 
  a chip using Bare-Metal Firmware.

    - Used to write Bare-Metal Firmware

Sometimes the Family Technical Manuals are missing information or issues are 
found with the chip after the release of the product. This is called the 
Chip Errata. 

++if chip is behaving badly, consult the errata sheet
This is a document that contains additional and corrective information for our 
particular device set

Errata sheet
--------------------------

Errata sheet

.. image:: ../../_images/W1_8_ChipErrataExample.PNG
   :scale: 75%

- This is a document that contains additional and corrective information for our 
  particular device set

    - It will list problems with the description of the configuration or the hardware's operation. 
      They usually provide a workaround in the errata sheet. 

- It is important that if something is operating unexpectedly with your chips 
  that you consult the errata.

**++Now the chip is chosen, Start on Proof of concept and Software Prototyping**

Proof of Concept and Prototyping
===================================

**A proof of concept**

- A proof of concept (POC) is a demonstration, the purpose of which is to verify that certain concepts
  or theories have the potential for real-world application(techopedia.com)
- a POC is designed purely to verify the functionality of a single or a set of
  concepts to be unified into other systems
    
    - shows that a product or feature can be developed, 

Many businesses just create development kits and module boards 

- so that prototyping can be done easily, speeding up the time it takes chip manufacturers
  to get their products in customer hands for evaluation

Some Development Kits for Prototyping
----------------------------------------

Freedom-KL25z
^^^^^^^^^^^^^^^^^^^

.. image:: ../../_images/W1_8_NXP_FRDM-KL25Z_dev_kit.PNG

This board is a product of NXP and has many impressive features including 

- ARM Cortex-M0+ Processor
- some supplemental external hardware,

    - a capacitive touch slide, 
    - an RGB LED,
    - and an accelerometer. 

- These are purposely added so that some simple applications can be developed 
  with external hardware as part of the evaluation process for the microcontroller.
    
    - R. Thus **this is a microcontroller development kit**

- has a secondary processor which is used to run as an onboard program debugger adaptor that 
  can flash our target processor, the KL25Z

    - This adaptor is called OpenSDA.
    - .. image:: ../../_images/W1_8_KLZ_onboardDebugger.PNG
         :scale: 50%

- The OpenSDA 

    - has a specialized, dedicated connection to the processor target for debug 
      support and for the programmer interface.
    - The user will connect to this open SDA platform by connecting a USB cable 
      to the host machine and running a special integrated development environment 
      for this processor

        - .. image:: ../../_images/W1_8_KLZ_onboardDevelopmentSupport.PNG
             :scale: 50%


**Nordic nRF24L01 chip**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The wireless module featuring the Nordic nRF24L01 chip. This is not a microcontroller 
development kit

.. image:: ../../_images/W1_8_nRF24L01+QFN_NordicChip.PNG

This chip

- has an RF antenna
- can be utilized to evaluate communication protocols to an antenna
- The PCB layout is built in a way that it can easily connect through microcontroller boards 
  using a few extra wires (R,using the simple interface
- This is a module board

Due to need for faster prototyping, some dev kits have become even more widely 
available at a cheap price with free software tools and integrated programmer 
debugger hardware on the kit itself. 

Ex]

- The Freedom Board mentionned earlier
- The MSP432 from Texas Instruments

    - .. image:: ../../_images/W1_8_TexasInst_MSP432_Programmer_Debugger.PNG
    - it comes with a dedicated onboard emulator for programming, debugging, and energy measurements.
        
        - TI called their technology XDS110 EnergyTrace on-board emulator. 
        - This EnergyTrace technology is an added system to evaluate energy uses of the 
          target processor during application. This is in addition to debugging and installing.


Summary
==========

As an Embedded Software Engineer, you

1. Acquire the hardware

    - acquire module boards or microcontroller dev kits
    - you figure out the connection and interface requirement between your boards
    - Now this give access to a platform

2. now you can start developing a software proof of concept without hardware 
   overhead. You will need:

    - compiler
    - Some programmer debugger hardware. (Might of dev kit itself)
    - and the executable installer software

