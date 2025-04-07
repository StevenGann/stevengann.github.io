---
layout: post
icon: fas fa-file
order: 0
---

# Steven Gann THIS IS A TEST
Hardware Validation Engineer | Embedded Systems Specialist

## Contact
- 📧 StevenGannJr@gmail.com
- 🌐 [LinkedIn](https://www.linkedin.com/in/sgann2012/)

**Objective:**  
To help form the cutting edge of the digital technology industry, developing hardware and software, and sharpening the line where they meet.

**Education:**  
Florida Atlantic University Boca Raton, FL

MS in Computer Engineering, minor in Business Administration

**Technical Skills:**

**Software:** Altium, Visual Studio, VS Code, Eclipse, Code Composer Studio, Altera Quartus, Logisim, Libero SoC Design Suite, MPLAB, Atmel Studio, Nvidia MODS, FreeCAD

**Languages:** C, C++, C#, Rust, Java, Assembly (MSP-430, PIC16, PIC24, AVR, x86, ARM, Z80), Python, Bash, OpenSCAD  

**Development Platforms:** Windows, GNU/Linux (Debian, Arch), Unix, FreeRTOS, Android (Java and .NET), iOS (Objective-C), CUDA

**Technology Proficiencies:** SIMD, .NET, Object Oriented Programming, Data Oriented Design, Hardware emulation, Realtime DSP, AI applications (LLMs, Agentic AI, RAG), CUDA, OpenGL

**Work Experience:**

**NVIDIA Corporation** **Santa Clara**, CA  
_Hardware Validation Engineer_ **2023-Present**  
As a Hardware Validation Engineer at Nvidia, I work within the Server Product Team to develop compute platforms for datacenter applications. In this role, I interact with a broad cross-functional network across NVIDIA and spearhead validation and debug efforts across multiple product families and platforms, including GPU families like Hopper and Blackwell, and the Grace CPU platform. This position has exposed me to the engineering concerns of high performance and high reliability computing, integration of complex compute systems into flexible software ecosystems, and the validation process for high power voltage regulators, PCIe, NVLink, and high-speed DRAM.

**Microchip Technology Inc.** Chandler, AZ  
_Senior Product Engineer_ **2022-2023**  
As a Product Engineer at Microchip, I worked within the 8-bit Microcontrollers division. I was involved in the New Product Development process from business plan to hardware development and then to manufacturing and sustaining business. I worked with multiple cross-discipline teams to ensure that resources are allocated, quality processes are followed, and the development lifecycle of each product proceeds on schedule until the product has not only been released to customers but has had all critical issues corrected and profit margins have met goals.

_Applications Engineer_ **2017-2022**  
As an Applications Engineer at Microchip, I worked within the Memory Products Division. I specialized in serial EEPROM and NOR Flash products, and my responsibilities included contributing to Design Objective Specifications, product validation, customer application support, and development of new training collateral and development tools for both internal and external customers.

Starting in 2020, my responsibilities expanded to leading the MPD Apps Software Development team. In this role, I implemented an Agile-based project management flow and led the team in development and maintenance of several open source customer training projects, the Serial EEPROM and Serial Flash modules for MPLAB Code Configurator, and the Microchip Memory Explorer development tool. I oversaw the adoption of git, Jira, and Confluence and provided the entire MPD Apps team with training on all of these tools.

**Logus Microwave Inc.** West Palm Beach, FL  
_Embedded Systems Engineer_ **2015-2017**  
At Logus Microwave, my team worked to develop the future of RF switching technology with embedded processing and IoT technologies. Beyond my hardware design and software development projects, I also worked to improve our ATE workflows with software automation and embedded processing. I have also had the great experience of working in teams from widely varied cultural and linguistic backgrounds.

**Florida Atlantic University** Boca Raton, FL  
_Graduate Teaching Assistant_ **2014-2015**  
During my graduate program at FAU, I was hired as a teaching assistant for Logic Design and Microprocessors under the distinguished Dr. Bassem Alhalabi. My duties involved grading and assisting with students' understanding of digital logic circuits and MSP-430 programming in Assembly and C.

**Notable Projects:**

**NVIDIA Blackwell Platform**

**<https://nvidianews.nvidia.com/news/nvidia-blackwell-platform-arrives-to-power-a-new-era-of-computing>**

I was heavily involved in the bring-up validation for three of the initial SKUs for Blackwell datacenter GPUs. My contribution to these SKUs was predominantly in regression testing new firmware versions and debugging of the proprietary GPU testing tool our team used for running validation tests. I was responsible for curating a suite of tests to balance coverage versus test time and ensuring these tests were run on each new release of the firmware, investigating all test failures to identify if they were meaningful failures, isolate the root cause, and lead debug efforts as needed.

**Microchip Memory Explorer  
<https://www.microchip.com/en-us/development-tool/DM160237>  
<https://www.microchip.com/en-us/development-tool/EV20F92A>**

**<https://www.microchip.com/en-us/development-tool/dm160232>**

Intended as a replacement for the antiquated Serial Memory Starter Kit, the I2C and SPI evaluation kits were produced to allow customers to read, write, and configure their serial memory devices during prototyping, as well as explore the features provided by each product in their respective families. I inherited and completed the original I2C Eval Kit GUI software and was the sole developer of the original SPI Eval Kit GUI software. Both of these were then superseded by the newer Memory Explorer software which I developed as part of a team of developers I was tasked to oversee. The original two GUIs provided a series of challenging obstacles as we were committed to an existing hardware design and firmware and in the case of the I2C GUI there was far too much existing development to justify a clean slate approach. The rewrite, Memory Explorer, provided its own challenges as we needed to work around unique internal customer requirements while also providing modernized UI/UX.

**EERAM Rally Mini Demo  
<https://github.com/MicrochipTech/EERAM-Rally-Mini-Demo>**

This project was based of a canceled demo that was being prototyped by another team, so we inherited the basic hardware configuration and only needed to update it to add support for the hot-swappable EERAM demo modules. The primary design objective was to recreate the functionality of an older, more expensive demo that had been built for trade shows and other venues, in a package that could be produced cheaply and deployed to field sales engineers. This was an especially fun project because dealing with audio and graphics on the PIC18 had many of the same constraints as doing the same on a 6052, Z80, or legacy ARM chips found in the various game consoles I’ve written homebrew games for in the past.

**SuperFlash Chip Erase Timing Demo  
<https://github.com/MicrochipTech/SuperFlash-ExplorerDemo>**

The purpose of this project was to provide a demonstration platform that showcases the industry-leading SuperFlash erase time capability while being easily reproducible by customers to assure them that there was no trickery involved. The greatest challenge of this project was ensuring compatibility with a broad selection of competitor NOR Flash products, each with their own nuances in command set and timing constraints. A more surprising challenge was our own internal supply as the base development board is owned by another business unit and on several occasions changed the PIC24 variant that ships with the board, requiring me to ensure the provided firmware was compatible across as much of the PIC24 family as possible by use of a custom HAL on top of the MPLAB Code Configurator HAL.

**EERAM Curiosity Demo  
<https://github.com/MicrochipTech/EERAM-Curiosity-Demo>**

This demo project was intended to showcase the record/replay capability of Microchip’s unique EERAM products. The fit internal customer requirements, we were required to build it around a development board with only one button and a potentiometer. We developed a daughterboard for the development board to host the showcased product and an array of LEDs to provide interactivity. This design kept the BOM cost low while providing a flashy and intuitive demo that could be deployed in the field.

**SWI Connector Demo  
<https://github.com/MicrochipTech/SWI-Connector-Demo>**

The objective of this project was to illustrate a customer story of embedded a single-wire EEPROM as part of a connector for the purpose of verifying a connector is compatible and produced by an authorized manufacturer. This project provided multiple challenges, most notably being the internal customer requirement of being able to ship units internationally. To avoid export restrictions, we made use of the XTEA cipher for the signature verification on the dummy connectors. The second major challenge was providing an intuitive, sales-friendly UI/UX for the demo on a development board with only two buttons.

**Industrial Inkjet Control Interface**

**<https://graphtech.us/industrialinkjetprinters/ez-inkjet-65/>**

I worked with GraphTec as a consultant engineer to assist them with embedded system design issues they had been facing. Most notably, the microcontroller controlling the UI for their premiere inkjet control system had a handful of bugs that their in-house developer had not been able to solve yet. I identified most of the issues as stemming from timing problems in an irregular active control loop and restructured the firmware with a more relaxed interrupt-driven approach that fixed all the known edge case bugs.

**Digitizing and Power over Ethernet Revisions of RF Switches**

While at Logus Microwave, my primary duty was in the design, development, and production of updated version of several products. The simplest of these was to replace a purely TTL circuit for the company’s patented motor control with an MSP-430 MCU to improve switching reliability and monitor the lifespan of the mechanism. The most ambitious was designing a version with PoE and a web interface, and then a variation that adapted the design to a single square inch. These were my first serious design projects as a professional engineer, and they represented formative learning experiences while I applied much of my academic theory to real-world applications.