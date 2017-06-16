# OSH Park Eagle scripts

These scripts are to help Eagle users get orders in just a tad bit faster, and help keep your Eagle files a bit more up to date.

This script will perform the following: 
- Add a menu button to enable easy PCB ordering right from inside Eagle. 
- Update your OSH Park CAM, DRU, and ULP files.
- Manage version-specific differences for CAM or DRC files. 
In the future, we'll be adding improvements to the script to add cool new feautures.


## Installation (easy mode)

The simplest way is to run our installer script. This consists of a few short steps, and we'll figure out the file paths to copy things over. 

1. Download all the files here. 
2. Open Eagle. 
3. Open or create a BRD file.
4. Select `File`, then `Run ULP`. On the file select dialog, browse to the files and select `INSTALL.ulp`.
5. Should be all done! You'll new see a new ![](bin/oshpark.png) button on your top toolbar, and any CAM, DRC, or ULP files are now updated. Click it to see cool stuff!

Restart Eagle to see the new menu button.

## Managing updates
Once you've run the installer, you can select `Update and Install`. This will automatically fetch any updates from the Github repository, and re-run the installer. 


## Installation (command line)
For Linux users with Eagle in the path, you can install this via command line with 

```
git clone https://github.com/OSHPark/OSHPark-Eagle-Tools.git
cd OSHPark-Eagle-Tools
eagle -C "RUN $PWD/INSTALL.ulp;quit;" blank.brd
```

## Installation (hard mode)

### Find your $EAGLEDIR directory

In Linux based systems, this directory is typically `~/.eagle/`

For Windows, this directory is typically `C:\Users\[username]\AppData\Roaming\CadSoft\EAGLE`. 

### Copy over the files

To get the image to display properly, copy the `oshpark.png` file to your `$EAGLEDIR/scr` directory.


Copy `oshpark.ulp` to `$EAGLEDIR/ulp/`

Copy `oshpark.png` to the `bin/` folder where Eagle is installed. This is unfortunately where Eagle looks for menu images. 

### Edit Eagle.scr

Next is some quick editing to `$EAGLEDIR/scr/eagle.scr`. This file is what Eagle uses for it's startup configuration, and consists of a few menu commands. This file consists of several sections, split up into sections for various work interfaces in Eagle.

```
BRD:
# configure the board layout editor

SCH:
# configure the schematic layout editor

LIB:
LBR:
DEV:
SYM:
PAC:
# configure various library menus
```

To add a button, we'll use the `MENU` command. This is most helpful under `BRD:`, but you can add it under `SCH:` if you want. 

The simplest method is to simply add the following line after any existing `MENU` lines. Doing so will overwrite any previous Menu commands.

```
MENU ' [oshpark.png]Oshpark{Upload PCB: Run oshpark-upload.ulp;}'
```

If you have other `MENU` items want to use, you can add it to the existing line.

```
MENU 'Text1:action1;'\
     'Text2:action2;'\
     '[oshpark.png]Oshpark{Upload PCB: Run oshpark-upload.ulp;}'\
     ;
```
Note that you must use a `\` at the end of each line, and each command or block must have a space between them (in this example, the spaces are at the start of the line). More complex examples can be found in the `src` dir in your Eagle installation.

### Test the button!

Click it! 

If everything worked properly, you should see a link to oshpark, where we've started processing the board!
