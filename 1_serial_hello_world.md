# Serial Hello World

In this tutorial we will be setting up Vitis 2023.2 to build, debug, and run code on our Microblaze cpu.

# Getting starting in Vitis.

## Seting up your workspace

In order to do anything in Vitis you need to open up a workspace, which will be a folder to contain your vitis project.

Choose where you want your vitis project to be and open up your workspace with Vitis(availible under the toolbar at File|Open Workspace).

## Creating a Vitis platform

Next create a platform component(toolbar at File|New Compenent|Platform).

Decide on the name of your platform and then click next to the Flow screen. Select `Browse` and select the `.xsa` created in the [Vivado MicroBlaze Setup](https://github.com/THE9rtyt/microblaze_phys333/blob/main/setup.md).

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/0dcafa34-db50-497b-9b40-079f8b65837e)

It is normal for the next page to take a moment to parse the data from hardware we just gave it. On the next page, select `standalone` and `microblaze_0`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/ce60670a-21f0-457e-9239-412ae3439292)

Click next and then finish to complete your platform setup.

Finally, we will build the platform.

To the build/run/debug control are found under the `FLOW` panel. Click build.
- To find this panel you might have to enable the drop down and pull up on the top edge to see the panel. See image below for more detail.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/cdd93c08-e2da-4f11-9fb5-f515af7f8a3e)

## hello_world application

Next create an application from an example application. Find the examples on the left edge of Vitis with the icon below.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/a1911cfa-0a64-4bdd-b32e-eb52edd3d295)

Find and click the example named `Hello World`. Then click `Create Application Component from Template`, this will pop up a setup window.

Click next through the pages, nothing should need to be changed here, leave the name and location defaults. Select your newly made platform on the hardware page, and click next until the windows closes. 

Find and click `helloworld.c` on the left sidebar under hello_world -> Sources -> helloworld.c to open up the code of our new application.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/7e164db3-7c72-40f8-8059-f82fa7fb6d69)

It's a simple program that just prints 2 lines into a serial port with a baudrate of 9600.

## Running hello_world

The hello_world application will be ready to build immidietly under the flow panel. After that completes, click debug to load the cpu and execute the program on your fpga.

After it loads the fpga, the debug view will pull itself up. from here you should also see your helloworld.c again and the cpu will be paused at the beginning of `main()`.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/ce5c03e6-7a69-497b-b7fd-88ee121134cf)

Across the top of the debug view we find our processor execution controls and can hit the first blue button like below to have our microblaze cpu run our application.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/49685b32-a4ec-4d00-8a46-d0331723cd00)

It will send in the 2 serial messages to a listening serial monitor. I used PuTTY myself but any serial monitor like the one in Arduino IDE should work. Make sure to set your baudrate to 9600!
