# A Guide for upgrading the firmware of the CR-10 Max

(Should still be reasonably current, you can (and should) read between the lines and look for newer builds)

## I hope this helps!

So, when I flashed my Max, it was around Sep 2020. The files I have on-hand are from then and a bit old.

I think, things have moved on a bit. I think the releases from Gihub for the Max, will at some point get the upstream Marlin features, like resonance-compensation for smoother printing...

Might be worthwhile to look into that and see if you can get a newer version.

* Looking back at my notes, I used the _Tiny machines firmware_: **CR10Max_BIL_DW7.hex**
  * [Seen here](https://www.tinymachines3d.com/pages/resourcepagecr10max)
  * Tiny Machines 3D link to [this page](https://docs.google.com/document/d/1dQeDrbs_9nVYW5JpdqZJ1GROiEJfFc2BIfmdAVW6ras/edit?pli=1)

## Read that carefully

* The copy of that document points to the 6.2 firmware file, which now exists at [this location](https://insanityautomation.com/Firmware/Creality/Obsolete/CR10Max_BIL_DW6.2.hex.gz)
* So I guess I ended up flashing CR10Max_BIL_DW7 and not CR10Max_BIL_DW6.2 as per that link. (V6.2 can now be found in the "Obsolete" sub-folder)

* [Here](https://insanityautomation.com/Firmware/Creality) you'll get a listing of new and current builds:
  * <https://insanityautomation.com/Firmware/Creality>
  
* The current version (as of Feb. 2024) linked there is:
  * [CR10Max_BIL_DW7.4.7.hex.zip](https://insanityautomation.com/Firmware/Creality/DW746/CR10Max_BIL_DW7.4.7.hex.zip)

  There is a limitation with Windows systems and path depth so the file names need to be shorter than we would prefer. If you get an error compiling due to the path limit, move the folder to the root of your hard drive.
  * Here is a legend to help decode the filenames:
    * BLT = BLTouch
    * ZM = BLTouch connected to ZMin port instead of Probe pin 5 connector harness
    * BIL = Bilinear Leveling
    * UBL - Unified Bed Leveling
    * DZ = Dual Z Steppers
    * Fil = FilamentRunout
    * Slnt = Creality Silent Board
    * H = E3D Hemera Extruder
    * MC = Mosquito Creality mount
    * ME = Mosquito E3D mount
    * Melzi Host option disables local SD card to allow more features and buffer for Octoprint control
    * NF = Noise filtering for machines with cable extensions - reduces homing accuracy!
    * LR = Stock runout replaced with Lerdge
  * This list might be subject to change/expand over time, [source here](https://github.com/InsanityAutomation/Marlin/blob/CrealityDwin_2.0/README.md):
    <https://github.com/InsanityAutomation/Marlin/blob/CrealityDwin_2.0/README.md>

  * If you have a ***vanilla CR10Max, with BL Touch and single OOTB extruder**, then you want the firmware with **BIL** in its name, so I'd say [this 7.4.7](https://insanityautomation.com/Firmware/Creality/DW747/CR10Max_BIL_DW7.4.7.hex.zip)
  is the best option for you.

If you look [here](https://github.com/InsanityAutomation/Marlin/tree/CrealityDwin_2.0/Marlin/src), it looks like our Firmware is based on Marlin 2.1 bugfix branch:
<https://github.com/InsanityAutomation/Marlin/tree/CrealityDwin_2.0/Marlin/src>

Not quite sure how to tell if that includes the new resonance-compensation stuff though...

I assume you know how to flash the mobo firmware, so I'll skip that.

* My notes do say that after flashing, my COM3 port on 115200 didn't connect anymore (I think with Pronterface), and I had to change the baud rate to 250000.

## Now - for the LCD firmware

* **Keep in mind, the linked videos are tailored for CR-10 S printers, your settings might slightly deviate**
* First go and watch [this Video](https://www.youtube.com/watch?v=EvG4uqx-Oos)
* and then also [this Video](https://www.youtube.com/watch?v=SBX30GmM3Qo&t=184s)
  
I assume you know [how to flash the LCD](https://youtu.be/SBX30GmM3Qo?t=228&si=xub7F4CkfgSIix7P), i'll only skim over it.

* You need to open the printer to gain access to the SD card slot on the LCD board directly

Looking at the files I had kept, the link in the old word doc I downloaded from Tinymachines, I used [this LCD firmware set](http://insanityautomation.com/Firmware/Creality/SingleExtruderScreens_V2Rev1.1.7z).

The LCD firmware I'm running is branded Tinymachines. That specific LCD-FW no longer exists, but I still have the file if you need it...

### If you are flashing a new version of the mobo firmware, you definitely need a newer version of the LCD firmware

The version of the word doc suggests [this LCD firmware](https://insanityautomation.com/Firmware/Creality/DW746/TM3D_Combined480272_Landscape_V7.7z) to pair with mobo firmware **746**.

If you skipped ahead to **747, you need to find a "combined landscape" file**, in this folder: Very likely [this file](https://insanityautomation.com/Firmware/Creality/DW747/CombinedLandscapeDwin_TM3D_V8.7z):

* <https://insanityautomation.com/Firmware/Creality/DW747/CombinedLandscapeDwin_TM3D_V8.7z>

Give that a go!

### Worst case, you can go back to 100% vanilla...

...Creality mobo and LCD firmware (I can share the file) - though we all know its horribly broken.

OK, I hope that gives you enough to go on.

* While I'm here: **Let me share a last few points:**
  * If you are like me and dont use 7zip or similar, you can [extract container-files like .7z files online](https://extract.me)
  
  * The BL Touch impl in the stock firmware is 100% broken
    All the scanning a matrix when you start to print is for nothing and the points are thrown away before print

  * Z-offset is stuffed
    new firmware fixes that.

  * Use Octoprint
    * Get a Raspberry pi, install that and hook that up to your printer
      * Makes running custom cgode commands to the mobo super easy!
    * After flashing the firmware you need to wipe the "EEPROM" on your printer
      * Just a chunk of persistent memory which holds all the settings.
      * You need to wipe all that cause its probably not compatible with the new firmware
      * I think its _M502_
       though check out the Teaching Tech videos or [the marlin G-Code reference](https://marlinfw.org/meta/gcode/) for that
    * After you've flashed, and you use Octoprint, you don't have to run the BL touch matrix measuring at the start of every print...
    * I use a plugin in Octo to run that now and then manualy.
    * You do need to customize that plugin to _SAVE THE MEASUREMENTS TO EEPROM_ (I can help with that if you need)
    * Customize your start-g-code in your Slicer to _LOAD_ those measurements from EEPROM (I can help with that too)
  * Get some non-backlash nuts for the Z axis - that helps. You might have to grind out a chunk of the nuts to install them, but it's worth it.

Other than that, Check out [Teaching Tech's videos](https://www.youtube.com/@TeachingTech/videos) on how to tune your printer, he also has a [custom website](https://teachingtechyt.github.io/), which can help you tune each separate thing and generate test print models etc. Its really great.

* **Some other things to look out for on the Max:**
  * The 2 gears on the rear Y-motor:
    * My one gear got loose and drifted a bit, (actually the grub-screw fell out!)
      the belt started wearing on the raised edge of the gear.
    * If you notice a lot of rubber dust under that rear motor:
      * check the alignment of the belt to the gears.
      * Maybe adjust the positions of the gears so you don't mess up that belt.
    * Even with the new firmware and working BLT
      * you will need a Z-offset adjustment

If you need to compare start-g-code or something, let me know.

## Good luck!

This guide was edited by [tino_moser_999](https://github.com/tinomoser999) based on a guide from the friendly [reddit-user u/ElectroSoprk9000](https://www.reddit.com/user/ElectroSpork9000/)
