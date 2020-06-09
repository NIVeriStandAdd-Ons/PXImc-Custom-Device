## PXImc Custom Device ##

**PXImc Custom Device** provides a high speed, low latency point to point link between another instance of the same custom device on a different controller via PXImc. This functionality is commonly used for [distributed Real-Time Test systems](http://www.ni.com/white-paper/11060/en). 

### LabVIEW Version ###

LabVIEW 2017

### Built Availability ###

This IP is built and available for NI VeriStand 2017 and later [here](https://github.com/NIVeriStandAdd-Ons/PXImc-Custom-Device/releases).

### Quality, Limitations ###

This IP should be considered immature. It has only been tested with PXImc 1.0 and NI VeriStand 2012.

### Instructions for Use ###
Before using the PXImc custom device to communicate data between multiple targets, your targets **must** be synchronized. See [Building Synchronized NI VeriStand Systems](http://www.ni.com/white-paper/14637/en) for an tutorial on how to achieve this.

To use the PXImc Custom Device, simply add it to your system definition file. It will prompt you how many doubles you would like to send and receive and create that many channels. Then, type the interface name to use. Interfaces have one of two different name formats:

1. For the PXImc module,  the name is in the form "PXI***chassis number***Slot***slot number***", where <chassis number> is the number assigned to the chassis in Measurement & Automation Explorer (MAX), and <slot number> is the number of the slot in which the NI PXImc module is installed.
1. For the MXI module, the name is in the form "PXIMC***interface id***", where interface id is a unique interface number starting at 1 going left to right for each MXI module in the chassis. 

Then, setup the local and remote write IDs.

- Provide a numeric value for the write ID for the  custom device.
- Provide the numeric value for the write ID of the custom device you are connecting to.
- Each write connection on the same interface must use unique write IDs. So one instance of this custom device on controller 1 connected to another instance of this custom devce on controller 2 will each need unique write IDs like 1 and 2 to talk to each other over the same cable. They both cannot use write ID 1.

### Example Setups ###

#### 2 Controllers ####
Controller 1's PXImc module connected to Controller 2's MXI module

- Controller 1 PXImc to controller 2: Name PXI1Slot7. Write ID 1. Remote write ID 2.
- Controller 2 MXI to controller 1: Name PXIMC1. Write ID 2. Remote write ID 1.

 
#### 3 Controllers ####
Controller 1 has 2x PXImc modules. First PXImc module connected to Controller 2's MXI module & second PXImc module connected to Controller 3's MXI module. Controller 2 has a PXImc module connected to Controller 3's second MXI module. So each controller can talk directly to each controller.

- Controller 1 PXImc to controller 2: Named PXI1Slot5. Write ID 1. Remote write ID 2.
- Controller 1 PXImc to controller 3: Named PXI1Slot7. Write ID 2. Remote write ID 1.
- Controller 2 MXI to controller 1: Named PXIMC1. Write ID 2. Remote write ID 1.
- Controller 2 PXImc to controller 3: Named PXI1Slot15. Write ID 1. Remote write ID 2.
- Controller 3 MXI to controller 1: Named PXIMC1. Write ID 1. Remote write ID 2.
- Controller 3 MXI to controller 2: Named PXIMC2. Write ID 2. Remote write ID 1.

### Benchmarks Vs. Reflective Memory ###
Using PXIe-1085 chassis with PXIe-8135 controllers, benchmarks were made comparing reflective memory to PXImc in NI VeriStand. First, the maximum non-late loop rates were found with reflective memory and performance metrics were recorded. Then, the same loop rates were met with PXImc and metrics were recorded. Therefore the metrics are directly comparable. In this case, **we did not try for highest loop rates possible on PXImc**, because we were comparing to reflective memory which cannot reach as high of rates.
 
Because reflective memory is natively integrated into NI VeriStand, you can directly map a model or hardware channel from controller 1 to controller 2 (or 3 or 4, etc). Because PXImc is not directly built into NI VeriStand, if you wish to share a hardware or model channel, you must map that channel to a PXImc custom device channel. Therefore there is an extra layer of mappings. Benchmarks were performed with this extra layer and without. The extra mapping numbers are indicated by the "mapped" term in the table below.

CPU % numbers are summed for all 4 cores of the 8135 and each target had about the same CPU %, so only one is shown.

| Doubles Sent From Each Controller to Each Controller | Controllers | Loop Rate (Hz) | PXImc CPU (%) | PXImc Mapped CPU (%) | Reflective Memory CPU (%) | PXImc HP Loop Duration (uS) | PXImc Mapped HP Loop Duration (uS) | Reflective Memory HP Loop Duration (uS) |
|------------------------------------------------------|-------------|----------------|---------------|----------------------|---------------------------|-----------------------------|------------------------------------|-----------------------------------------|
| 10                                                   | 2           | 4000           | 20            | 20                   | 26                        | 35                          | 35                                 | 48                                      |
| 10                                                   | 3           | 3750           | 25            | 25                   | 29                        | 51                          | 51                                 | 69                                      |
| 5000                                                 | 2           | 550            | 7             | 12                   | 34                        | 81                          | 136                                | 1001                                    |
| 5000                                                 | 3           | 200            | 5             | 10                   | 33                        | 135                         | 270                                |2800                                         |

### Dependencies ###

NI-PXImc 1.0 or later

### Version History ###

- ** v 1.0.0 ** Initial version. This version provides very limiting user interface, only allowing one time setup of the custom device settings.
- ** v 2.0.0 ** New features: Updated section names; added channel management features (add or remove, custom channel naming, import / export feature), added version manager (also for imported channel configurations); build folder structure cleaned up

### License ###

*This repository and any materials provided by NI therein are provided AS IS. NI DISCLAIMS ANY AND ALL LIABILITIES FOR AND MAKES NO WARRANTIES, EITHER EXPRESS OR IMPLIED, INCLUDING WITHOUT LIMITATION ANY WARRANTIES OF MERCHANTABILITY, FITNESS FOR  PARTICULAR PURPOSE, OR NON-INFRINGEMENT OF INTELLECTUAL PROPERTY. NI shall have no liability for any direct, indirect, incidental, punitive, special, or consequential damages for your use of the repository or any materials contained therein.*
