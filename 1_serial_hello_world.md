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

Find and click `helloworld.c` on the left sidebar under hello_world -> Sources -> helloworld.c.

![image](https://github.com/THE9rtyt/microblaze_phys333/assets/83201905/7e164db3-7c72-40f8-8059-f82fa7fb6d69)

## Running hello_world

The hello_world application will be ready to build immidietly under the Flow panel. After that completes, click debug to load the cpu and execute the program on your fpga.

After it loads the fpga, the debug view will pull itself up. from here you should also see your helloworld.c again and the cpu will be paused at the beginning of `main()`.

#TODO: picture of debug view paused on first line of app.