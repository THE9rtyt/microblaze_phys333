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

Select the `xc7a100tcsg324-1` under Parts, or find the `Nexys A7` under boards.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f2d72100-1305-4d85-b040-e4a10d9a965a)

On the next page, click `Finish` to create your project.

With your new project made, click `Create Block Design` on the left sidebar of Vivado.

Use "system" for a name if you don't have a good one off hand.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/abc01979-6435-4333-9bc2-c66853a5c852)

## Memory Interface Generator

Find your "Board" window, then find the `DDR2 SDRAM` connection and drag it into the empty block design diagram.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/fe03cf64-7c3d-4c94-9b7a-141a42052a95)

If everything goes well, you will have a `mig_7series_0` connected situated like the picture below.

Unfortunately, the auto-wiring that Vivado just did isn't quit right see [here](https://github.com/viktor-nikolov/MicroBlaze-DDR3-tutorial/blob/main/README.md#1-mig-input-reference-clock-must-be-200-mhz) for details

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/1300ce56-3942-41e9-8712-4d87ca507a53)

Delete both `clk_ref_i` and `sys_clk_i`.

Double-click the MIG to reconfigure it.

Click next until you get to the "Memory Options" page.

Disable the "Select Additional Clocks" checkbox.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a1c93ae2-e4a5-4e6e-92df-abeac84bf950)

Click next to the "FPGA Options" page.

Set the "System Clock" option to `No Buffer`

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/b365a0d7-0693-4933-9a5d-fd4111a09ad3)

Click next until you finish the MIG configuration.

- You will have to click "Validate" to enable the next button on the "Pin Slection" page and accept the license on the "Simulation Options" page.

## Configuring Ports

Download [Nexys-A7-100T-Master.xdc](https://github.com/Digilent/digilent-xdc/blob/master/Nexys-A7-100T-Master.xdc) from the [Digilent GitHub](https://github.com/Digilent).

Find your "Source" window and click the plus button to add sources.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/116b8133-3a88-47a3-a81a-4574da3413b6)

Select constraints and add the file your just downloaded. Make sure to select the "Copy constraints files into project" checkbox.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f0ecdb4f-2f6c-48b3-9c5d-08f4190b2c14)

Open the added constraints file in Vivado. uncomment lines 7, 8, and 74 for the `CLK100MHZ` and `CPU_RESETN` ports.

Save your constraints file and go back to the block design diagram.

Add the enabled ports by right clicking an empty part of the Diagram and selecting "Create Port...".

Set the frequency for `CLK100MHZ`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a94c4c95-20a2-4b80-a547-1ab724142c49)

Set the Polarity of the reset button to Active Low(Reset button generates a HIGH signal when not pressed).

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/ebbc4d04-1ef4-414a-b182-8a639487b919)

Right click an empty section of the Diagram, click "Add IP...", and search for "buffer". Double-click on "Utility Buffer" to add it to the diagram.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/1b8f97f4-1884-420b-81b9-f23ac07b8b0c)

Double-click the buffer to open its configuration. Select `BUFG` under "C Buf Type".

Connect `CPU_RESETN` to the mig's `sys_rst` port. Connect the `CLK100MHZ` to the `BUFG_I` port and the `BUFG_O` to the mig's `sys_clk_i` port. You should have something like the image below.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/08391ebd-5b73-4b0d-acc1-63578a19e301)

## Clocking Wizard

Right click an empty section of the Diagram, click "Add IP...", and search for "clocking". Double-click on "Clocking Wizard" to add it to the diagram.Double-click the Clocking Wizard to open its configuration.

Go to the "Clocking Options" tab, scroll over and down if needed and select `No buffer` for the Source of Primary

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/db7939bb-a41f-43f4-9b60-46b71bbf79ca)

Go to the "Output Clocks" tab, enable 2 clocks and set both of them to 200MHz. `clk_out1` will be for the MicroBlaze and all other stuff. `clk_out2` is for the MIG.

Scroll down if needed and set Reset Type to `Active Low`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a561c957-5023-46dd-8534-a87f0069493c)

Connect `clk_in1` to the buffer's `BUFG_O`, `CLK_out2` to the mig's `clk_ref_i`, `resetn` to the `CPU_RESETN` port like the image below.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/60ed167a-88cd-4304-aa33-8c370f4a91c9)


