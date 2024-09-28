<h1 align="center">
<img src="https://raw.githubusercontent.com/enjoy-digital/litex/master/doc/litex.png" alt="Litex icon" width="170">
<br>Hazard3 support in Litex<br>
</h1>

![RP2350 SOC with Hazard3 Cores](https://www.raspberrypi.com/app/uploads/2024/08/RP2350-colour-Medium.jpeg)


## RISC-V

RISC-V is an open standard instruction set architecture based on established RISC (Reduced Instruction Set Computer) principles.

Comparing to ARM and x86, a RISC-V CPU has the following advantages: 

- Free : RISC-V is open-source.
- Simple : RISC-V is much smaller than other commercial ISAs.
- Modular : RISC-V has a small standard base ISA, with multiple standard extensions.
- Stable : Base and first standard extensions are already frozen. There is no need to worry about major updates. 
- Extensibility : Specific functions can be added based on extensions. There are many more extensions are under development. 

## Hazard3

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

Hazard3 uses AHB5 from the AMBA protocol specification for high-speed, high-bandwidth communication between processor, memories, DMA controllers, and other high-speed peripherals.


## Litex

![litex](images/litex-soc.png)

The use of a combination of hardware construction languages SpinalHDL and Verilog HDL to implement a 32-bit Linux-capable RISC-V processor. LiteX and SpinalHDL are two intertwined frameworks in the design flow. The CPU core was created with SpinalHDL, while the integration of IP and CPU cores was performed with LiteX. Verilog source code was generated with the configured 32-bit RISC-V architecture after the design was completed on the high-level framework. This 32- bit RISC-V architecture was successfully built on a Nexys4DDR FPGA.

Using the LiteX framework, the Hazard3 core will be created with all FPGA primitives and a project file. The LiteX framework allows to create FPGA cores/System-on-Chips, experiment with different digital design architectures, and build complete FPGA-based systems. Using LiteX in conjunction with the ecosystem of cores, which includes VexRiscV, CVA5 etc makes building complicated SoCs easier than with previous methods, while also improving portability and flexibility

The Hazard3 SoC will be generated with the following configuration after calling the SpinalHDL creation process and the LiteX build process: 

- Single core Hazard3 cores (RV32IMAC) with Supervisor mode.
	- 8k, 2-way L1 I and D 
	- Memory Management Unit 
	- 32-bit Wishbone Bus, 4.0GB Address Space 
- L2 cache:
	- 8KB, 128-bit bus width L2 
	- UART, SD-card peripherals
	- Custom accelerator built into CPU core

Timing constraints and IO assignment was generated alongside the Verilog RTL as a XDC file. The design was synthesized and implemented using Vivado 2016.4. The design’s resource utilization was shown in Table 3 for each configuration.

All DRC violations were either resolved or waived, and the STA (Static Timing Analysis) report was cleaned. The FPGA board is fed by a 100MHz crystal, whereas the Hazard3 core complex is driven by a Xilinx Mixed-Mode Clock Manager (MMCM) at 75MHz.

Over UART, the Linux Image with all the tests would be sent and loaded into the system main memory. That procedure is handled by the first stage bootloader. The Linux image was created with Buildroot.



## Details

Read more about Hazard3 👉 [Documentation - Hazard3]

[Documentation - Hazard3]: https://github.com/Wren6991/Hazard3



## Quick start

```sh
python3 -m pipenv shell
pipenv install
inv serve
```


## License

- [MIT License]

[MIT License]: (docs_sample/license.md)
