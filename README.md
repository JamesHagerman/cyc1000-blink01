# Blink for CYC1000 (Intel Cyclone 10 LP) Development Board

This project is a very tiny Quartus 19.1 project that shows the bare minimum configuration you need to be able to design FPGA logic programs and run them on the board

Consider this an updated version of this other project I put together for a cheap Cyclone II dev board: https://github.com/JamesHagerman/Altera-Cyclone-II-EP2C5T144-blink

The VHDL is copied from that other project. I created a new project in Quartus using the `TEI0003 Basic Example` (https://shop.trenz-electronic.de/en/Download/?path=Trenz_Electronic/Modules_and_Module_Carriers/2.5x6.15/TEI0003/Reference_Design/19.1/test_board) as a reference for the project details.

*Note: The main oscillator on the CYC1000 runs at 12MHz (https://wiki.trenz-electronic.de/display/PD/TEI0003+TRM for reference) while the `EP2C5T144 ` Cyclone II board's oscillator runs at 50MHz...*

*Note: The rest of this has NOT been updated for the CYC1000!*

## Basic compile

`ctrl-l` (that's an L)

## Basic pin assignment

`Assignments -> Pin Planner` then change the `Location` field. Recompile after you change pin assignments

## Basic programming

`Tools -> Programmer` then select you hardware, then press `Device Detect` OR (if device detect stalls for 30 seconds) just manually add the `EP2C5T144` device

## Programming directly to the config memory

This is the pain in the butt. You need to convert the `.pom` output from your compile step to a `.jic` file first. Actually, all of this is described in: [https://www.altera.com/en_US/pdfs/literature/an/an370.pdf](https://www.altera.com/en_US/pdfs/literature/an/an370.pdf)

1. Open `File -> Convert Programming File`
2. Select `JTAG Indirect Configuration File (.jic)` from the `Programming file type:` select box
3. Select `EPCS4` in the `Configuration device:` select box
4. Provide a `File name:` for the output `.jic` file
5. In the `Input files to convert` list, highlight the `SOF Data` and select `Add File...`
6. Navigate to and select the `.sof` file generated when you compiled the design (eg. `blink01.sof`). Click `Open`
7. Again, in the `Input files to convert` list, highlight the `Flash Loader` and select `Add Device...`
8. Navigate through the list of devices to find your FPGA (eg. `Cyclone II` and `EP2C5`). Click `OK` to select that device and go back to the `Convert Programming File` window
9. Click `Generate` to convert the `.sof` input to a `.jic` file


Now that you have a compiled `.jic` file, open the `Programmer` window, and select `View -> Show Device Tree` to show the details about what you're going to flash.

1. Click `Add File...` on th left hand side
2. Select your new `.jic` file
3. On the line listing your `.jic` file, check the `Program/Configure` box
4. Make sure only your new device configuration is listed.
5. Click `Start` to flash the `.jic` file to the `EPCS4` flash
6. Disconnect the USB-Blaster and power cycle the board. It should load your compiled design from the flash chip and start executing it.
