This is an I2C internal flash programming examples for MachXO2/MachXO3 and MachXO3D.

The MachXO3 and MachXO2 devices feature internal flash memory that supports single boot, eliminating the need for an external flash and reducing component count. However, an external flash can still be used to store a golden image.For more information
please check FPGA-TN-02155 ([MachXO2 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=39085)) and FPGA-TN-02055 ([MachXO3 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=50123)).

MachXO3D have two sectors for internal flash programming namely CFG0 and CFG1. These sectors can be used for dual-boot instead of using an external flash for golden image. FPGA-TN-02069-1.8 ([MachXO3D Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=52591)) shows more information regarding MachXO3D dualboot and other features.

The set-up for this example is like below:
![image](https://github.com/user-attachments/assets/cca75baf-7aff-42cc-abe5-1f9a9c1d12f0)


STM32F411 discovery board contains bitstream for both MachXO3LF Starter kit and MachXO3D Breakout Board. We are able to fit two bitstreams by not including the blank bits in the bitstream after the comment *NOTE END CONFIG DATA*. We also do not have any contents in the UFM sector. In designs where UFM is used, all bits are needed to be programmed. This information is indicated in FPGA-TN-02055 Table 9.2:

![image](https://github.com/user-attachments/assets/e6fd72ea-ecd5-4e59-8079-ed1e5cad8806)

Take note that Lattice Devices have default address of 0x40 at hardware default. This means, you have to update the address of one device before putting them in the I2C bus to avoid address collision. In this example, the I2C address of the MachXO3D is 0x08 and the MachXO3LF is left to be 0x40.

