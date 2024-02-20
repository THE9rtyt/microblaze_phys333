# Setup for MicroBlaze on the Nexys A7
This setup wouldn't have been possible without these tutorials:
- https://github.com/viktor-nikolov/MicroBlaze-DDR3-tutorial
- https://digilent.com/reference/programmable-logic/guides/getting-started-with-ipi

# Getting started in Vivado

## Make a new project

Make a new Project, give it a name.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/c67f9092-bff2-4040-aaa2-af2394caf408)

RTL Project, Do not specify sources at this time.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/38f74de0-fc16-49a7-9d9e-8abb310cb6a7)

Select the `xc7a100tcsg324-1` under Parts, or find the `Nexys A7` under boards. I'm using the 100T version.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f2d72100-1305-4d85-b040-e4a10d9a965a)

On the next page, click `Finish` to create your project.

With your new project made, click `Create Block Design` on the left sidebar "Project Manager" of Vivado.  

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/b425b296-acd0-4200-a1ec-5f38df40f9bc)

Use "system" for a name if you don't have a good one off hand.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/abc01979-6435-4333-9bc2-c66853a5c852)

## Memory Interface Generator

Select the "Board" tab, probably in the top middle of the window, then find the `DDR2 SDRAM` connection and drag it into the empty block design diagram tab, probably on the right side of the screen.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/fe03cf64-7c3d-4c94-9b7a-141a42052a95)

If everything goes well, you will have a `mig_7series_0` connected situated like the image below.

- throughout these tutorials when I share pictures of block diagrams they will always be screenshotted after clicking the `Regernerate` layout button across the top toolbar on the Diagram tab. ![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/727d57b4-eeb9-48bf-820e-d6ce7a9554db)


Unfortunately, the auto-wiring that Vivado just did isn't quite right see [here](https://github.com/viktor-nikolov/MicroBlaze-DDR3-tutorial/blob/main/README.md#1-mig-input-reference-clock-must-be-200-mhz) for details

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/1300ce56-3942-41e9-8712-4d87ca507a53)

Delete both `clk_ref_i` and `sys_clk_i` by right-clicking on their box and selecting delete.

Double-click the MIG to reconfigure it.

Click next until you get to the "Memory Options" page (see below).

Disable the "Select Additional Clocks" checkbox.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a1c93ae2-e4a5-4e6e-92df-abeac84bf950)

Click next to the "FPGA Options" page.

Set the "System Clock" option to `No Buffer`

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/b365a0d7-0693-4933-9a5d-fd4111a09ad3)

Click next until you finish the MIG configuration.

- You will have to click "Validate" to enable the next button on the "Pin Selection" page and accept the license on the "Simulation Options" page.

## Configuring Ports

Download [Nexys-A7-100T-Master.xdc](https://github.com/Digilent/digilent-xdc/blob/master/Nexys-A7-100T-Master.xdc) from the [Digilent GitHub](https://github.com/Digilent).

Select your "Source" tab, probably in the same window the the "Board" tab and click the plus button to add sources.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/116b8133-3a88-47a3-a81a-4574da3413b6)

Select constraints and add the file you just downloaded. Make sure to select the "Copy constraints files into project" checkbox.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f0ecdb4f-2f6c-48b3-9c5d-08f4190b2c14)

Open the added constraints file in Vivado. uncomment the lines for the `CLK100MHZ` and `CPU_RESETN` ports like below, they should be lines 7, 8 and 74.

```
## Clock signal
set_property -dict { PACKAGE_PIN E3    IOSTANDARD LVCMOS33 } [get_ports { CLK100MHZ }]; #IO_L12P_T1_MRCC_35 Sch=clk100mhz
create_clock -add -name sys_clk_pin -period 10.00 -waveform {0 5} [get_ports {CLK100MHZ}];

##CPU Reset Button
set_property -dict { PACKAGE_PIN C12   IOSTANDARD LVCMOS33 } [get_ports { CPU_RESETN }]; #IO_L3P_T0_DQS_AD1P_15 Sch=cpu_resetn
```

Save your constraints file and go back to the block design diagram (tab).

Add the enabled ports by right clicking an empty part of the Diagram and selecting "Create Port...".  

The first port is  `CLK100MHZ`, settings below.

Set the frequency for `CLK100MHZ`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a94c4c95-20a2-4b80-a547-1ab724142c49)

The second port to create is `CPU_RESETN`, again see settings below.

Set the Polarity of the reset button to Active Low(Reset button generates a HIGH signal when not pressed).

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/ebbc4d04-1ef4-414a-b182-8a639487b919)

Right click an empty section of the Diagram, click "Add IP...", and search for "buffer". Double-click on "Utility Buffer" to add it to the diagram.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/1b8f97f4-1884-420b-81b9-f23ac07b8b0c)

Double-click the buffer to open its configuration. Select `BUFG` under "C Buf Type". Click ok the close the window and complete configuration.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/68673ad3-9c14-4837-a9e9-022c50581f69)

Connect `CPU_RESETN` to the mig's `sys_rst` port by clicking and dragging from one pin/port location to another. Connect the `CLK100MHZ` to the `BUFG_I` port and the `BUFG_O` to the mig's `sys_clk_i` port. You should have something like the image below.
- I have found it easier to start dragging from a pin to port than port to pin.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/08391ebd-5b73-4b0d-acc1-63578a19e301)

## Clocking Wizard

Right click an empty section of the Diagram, click "Add IP...", and search for "clocking". Double-click on "Clocking Wizard" to add it to the diagram. Double-click the Clocking Wizard to open its configuration.

Go to the "Clocking Options" tab, scroll over and down if needed and select `No buffer` for the Source of Primary

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/db7939bb-a41f-43f4-9b60-46b71bbf79ca)

Go to the "Output Clocks" tab, enable 2 clocks. `clk_out1` will be for the MicroBlaze and peripherals, set this to 100MHz. `clk_out2` is for the MIG, set this to 200MHz.

Scroll down if needed and set Reset Type to `Active Low`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/0a6437eb-1140-4215-bf40-1bdd06c379bd)

Connect `clk_in1` to the buffer's `BUFG_O`, `CLK_out2` to the mig's `clk_ref_i`, `resetn` to the `CPU_RESETN` port like the image below.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/7da42770-5c0f-4e67-b5c7-1bcd5eb5d3b0)

## USB uart

From the "Board" tab, find the `USB_UART` connection and drag it into an empty section of the block design diagram. It will create its own port to usb_uart.

## MicroBlaze

Right click an empty section of the Diagram, click "Add IP...", and search for "micro". Double-click on "MicroBlaze" to add it to the diagram.

Click on the "Run Block Automation" in the green bar across the top of the diagram to open the MicroBlaze configuration.

Select the `Real-time` Preset. Make sure the Peripheral AXI Port is Enabled, and disable the Interrupt Controller.

The Clock Connection should be set to `/clk_wiz_0/clk_out1`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/dac6a1f0-6356-4a3f-861a-cdead6da03b9)

Click OK and everything is going to get wired up, if everything has gone well it should look like the diagram below.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/9fffd36e-f7b8-4d20-a09e-28e24d92d3b9)

Double-click the microblaze to open its configuration. Click next to get to page 4 "Cache".

Set the Instruction Cache to 16 kB and Data Cache to 32kB. Line Length and Number of Victims to 8 for both caches.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/6d942319-5c0d-43b0-acdb-ab89eb151881)

Click Next to go to page 5 "Debug". Set Number of Read Address Watchpoints to 1(this is to make debugging easier). Click ok the close the window and complete configuration.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/3da1e1ac-850d-407d-a21e-3ee52d1d49dd)

Click on the "Run Connection Automation" in the green bar across the top of the diagram. Select all Automation when the window pops up. Click and let it wire everything together.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f6fea68d-932c-43d9-88bd-e87dbe10e13a)

## Generating Bitstream and Exporting Hardware Design

"Validate Design" with the small checkbox icon on the top bar of the block diagram window.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/07871ac8-b2a8-40fc-b83e-b42d221f42ff)

From the Sources tab, right click on 'system.bd' and click "Create HDL Wrapper", and select "Let Vivado manage wrapper and auto-update" in the pop up.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/70b43e78-0eb9-4993-a558-fc379884551c)

Generate Bitstream. this will likely take a while, if it fails you might've ran out of RAM, a restart and only running Vivado itself is how I overcome this.

Export the hardware design from the drop down menu on the toolbar. It's under File->Export->Export Hardware.

Make sure to check the box to include your bitstream, Note where the `.xsa` file will be placed(default is project root folder) and finish the export.

From here, you're ready to head into Vitis.
