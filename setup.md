# Setup for MicroBlaze on the Nexys A7
this setup is put together by me, but wouldn't have been possible without these tutorials:
- https://github.com/viktor-nikolov/MicroBlaze-DDR3-tutorial
- https://digilent.com/reference/programmable-logic/guides/getting-started-with-ipi

## Getting started in Vivado

Make a new Project, give it a name

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/c67f9092-bff2-4040-aaa2-af2394caf408)

RTL Project, Do not specify sources at this time

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/38f74de0-fc16-49a7-9d9e-8abb310cb6a7)

select the xc7a100tcsg324-1 under Parts, or find the Nexys A7 under Boards

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/f2d72100-1305-4d85-b040-e4a10d9a965a)

on the next page, click `Finish` to create your project

With your new project made, click `Create Block Design` on the left sidebar of Vivado

use "system" for a name if you don't have a good one off hand

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/abc01979-6435-4333-9bc2-c66853a5c852)

find your "Board" window, then find the `DDR2 SDRAM` connection and drag it into the empty Block Design

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/fe03cf64-7c3d-4c94-9b7a-141a42052a95)

if everything goes well, you will have a mig_7series_0 connected situated like the picture below

unfortunately, the auto-wiring that Vivado just did isn't quit right see [here](https://github.com/viktor-nikolov/MicroBlaze-DDR3-tutorial/blob/main/README.md#1-mig-input-reference-clock-must-be-200-mhz) for details

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/1300ce56-3942-41e9-8712-4d87ca507a53)



