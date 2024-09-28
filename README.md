<h1 align="center">
<img src="https://raw.githubusercontent.com/enjoy-digital/litex/master/doc/litex.png" alt="Litex icon" width="170">
<br>Hazard3 support in Litex<br>
</h1>

# Below components are involved in the project

## RISC-V ISA

RISC-V is an open standard instruction set architecture based on established RISC (Reduced Instruction Set Computer) principles.

Comparing to ARM and x86, a RISC-V CPU has the following advantages: 

- Free : RISC-V is open-source.
- Simple : RISC-V is much smaller than other commercial ISAs.
- Modular : RISC-V has a small standard base ISA, with multiple standard extensions.
- Stable : Base and first standard extensions are already frozen. There is no need to worry about major updates. 
- Extensibility : Specific functions can be added based on extensions. There are many more extensions are under development. 

## Hazard3 Processor

![RP2350 SOC with Hazard3 Cores](https://www.raspberrypi.com/app/uploads/2024/08/RP2350-colour-Medium.jpeg)

Hazard3, currently used in RP2350 SOC from Raspberry Pi Pico2, is a configurable 3-stage RISC-V processor with :

- RV32I: 32-bit base instruction set
- Extensions :
    - M: integer multiply/divide/modulo
    - A: atomic memory operations
    - C: compressed instructions 
    - Zicsr: CSR access
    - Cryptography instructions
    - Priviledged Machine instructions
    - Debug instrutions
    - some Bit-Manipulation instructions 
    - some custom instructions

Hazard3 uses Advanced High-performance Bus (AHB) from the Advanced Microcontroller Bus Architecture (AMBA) protocol specification for high-speed, high-bandwidth communication between processor, memories, DMA controllers, and other high-speed peripherals.


## Litex SOC Framework

The use of a combination of hardware construction languages SpinalHDL and Verilog HDL to implement a 32-bit Linux-capable RISC-V processor. LiteX and SpinalHDL are two intertwined frameworks in the design flow. The CPU cores can be created with SpinalHDL/Verilog, while the integration of peripheral IPs and CPU cores is performed with LiteX. 

The LiteX framework allows to create FPGA cores/System-on-Chips, experiment with different digital design architectures, and build complete FPGA-based systems. Using LiteX in conjunction with the ecosystem of cores, which includes VexRiscV, CVA5 etc makes building complicated SoCs easier than with previous methods, while also improving portability and flexibility


# Implementation Details of the project

The objective of the project is to integrate Hazard3 Processor as a Softcore in Litex Framework with Linux support. 
Following steps are involved :

1. Verilog source code of Hazard3 will be integrated with the configured 32-bit RISC-V SOC architecture using Litex SOC framework. This 32- bit RISC-V SOC will be synthesized on a Nexys4DDR Xilinx FPGA.


2. The Hazard3 SoC will be generated with the following configuration after calling the Verilog code and the LiteX build process: 

    - Single core Hazard3 cores (RV32IMAC) with Supervisor mode.
	    - 8k, 2-way L1 IandD 
	    - Memory Management Unit 
	    - 32-bit Wishbone Bus, 4.0GB Address Space 
    - L2 cache:
	    - 8KB, 128-bit bus width L2 
	    - UART, SD-card peripherals
	    - Custom accelerator built into CPU core

3. Timing constraints and IO assignment will be generated alongside the Verilog RTL as a XDC file. The design will be synthesized and implemented using Vivado/Yosys. 

4. All DRC violations will be either resolved or waived, and the STA (Static Timing Analysis) report will be cleaned. The FPGA board would be fed by a 100MHz crystal, whereas the Hazard3 core complex would be driven by a Xilinx Mixed-Mode Clock Manager (MMCM) at 75MHz.

5. Over UART, the Linux Image with all the tests would be sent and loaded into the system main memory. That procedure is handled by the first stage bootloader using Litex BIOS. The Linux image will be created with Buildroot.


## Budget Details for the Project


| Project Component | Details                                            | Budget |
|-------------------|----------------------------------------------------|--------|
| Hardware          | FPGA boards, Prototyping Materials, Test Equipment | 20000  |
| Gateware          | HDL code development for integrating Hazard3       | 10000  |
| Firmware          | Litex Bios/Bootloader code development             | 10000  |
| Software          | Linux code development for Hazard3                 | 10000  |


Total Cost of Project = Hardware Cost + Gateware Cost + Firmware Cost + Software Cost = 20000 + 10000 + 10000 + 10000 = 50000 €


## External Resources

Read more about Hazard3 👉 [Documentation - Hazard3]

[Documentation - Hazard3]: https://github.com/Wren6991/Hazard3



## Build Documentation

```sh
python3 -m pipenv shell
pipenv install
inv serve
```


## License

- [MIT License]

[MIT License]: (docs_sample/license.md)
