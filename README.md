This is an I2C internal flash programming examples for MachXO2/MachXO3 and MachXO3D.

The MachXO3 and MachXO2 devices feature internal flash memory that supports single boot, eliminating the need for an external flash and reducing component count. However, an external flash can still be used to store a golden image.For more information
please check FPGA-TN-02155 ([MachXO2 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=39085)) and FPGA-TN-02055 ([MachXO3 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=50123)).

MachXO3D have two sectors for internal flash programming namely CFG0 and CFG1. These sectors can be used for dual-boot instead of using an external flash for golden image. FPGA-TN-02069-1.8 ([MachXO3D Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=52591)) shows more information regarding MachXO3D dualboot and other features.

The set-up for this example is like below:
![image](https://github.com/user-attachments/assets/cca75baf-7aff-42cc-abe5-1f9a9c1d12f0)


STM32F411 discovery board contains bitstream for both MachXO3LF Starter kit and MachXO3D Breakout Board. We are able to fit two bitstreams by not including the blank bits in the bitstream after the comment *NOTE END CONFIG DATA*. We also do not have any contents in the UFM sector. In designs where UFM is used, all bits are needed to be programmed. This information is indicated in FPGA-TN-02055 Table 9.2:

![image](https://github.com/user-attachments/assets/e6fd72ea-ecd5-4e59-8079-ed1e5cad8806)

Doing this allows us to save more space while having two bitstreams in the internal flash:
![image](https://github.com/user-attachments/assets/5848ff2a-93d2-420f-a9d7-547352151b01)


Take note that Lattice Devices have default address of 0x40 at hardware default. This means, you have to update the address of one device before putting them in the I2C bus to avoid address collision. In this example, the I2C address of the MachXO3D is 0x08 and the MachXO3LF is left to be 0x40.

To change the I2C Address of the MachXO3/XO3D, you have to instantiate the EFB IP and activate I2C Primary configuration like below:
![image](https://github.com/user-attachments/assets/ec9c6b31-0d91-42d3-b10a-e46baae17d1d)

![image](https://github.com/user-attachments/assets/e9699adf-33cb-4e73-b26f-cf659993d01f)

Do not forget to port the i2c pins as inout pins of your top module:
![image](https://github.com/user-attachments/assets/99871b34-12c0-428b-9b12-de67c4c9bd46)

Generate your bitstream then program the feature rows via JTAG using Diamond Programmer. For MachXO3D, the feature row is in the .fea file while the MachXO3 feature row is in the .jed file.

This example uses different functions made for programming XO2/XO3 and XO3D. 
![image](https://github.com/user-attachments/assets/969d937f-516a-4304-9b8b-d9a45235184f)


For the XO2/XO3 internal flash programming, the arguments are the I2C_ADDRESS, the array to be programmed, and the length of the array to be programed while for XO3D an additional argument for CFG sector is needed.Additionally, it is always a good practice to check the device ID of all devices in the bus before proceeding with the programming procedure.

This implementation only have lower consumption of flash and ram as we reduced the bitstream only to the relevant bits and did not program the UFM sector:

![image](https://github.com/user-attachments/assets/919adb5f-58e9-4c4e-bbc0-6c3a6b068584)

See sample Console Run below:
![image](https://github.com/user-attachments/assets/c3f6d29c-62bd-42ba-a20e-29f9063cf559)

![image](https://github.com/user-attachments/assets/cbca2fd6-553e-478c-8832-f076db1e4af5)


