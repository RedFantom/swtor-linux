# SWTOR on Linux
#### How to install and run SWTOR on a Linux machine through wine
Welcome to this guide on how to install and run Star Wars: The Old
Republic on a Linux distribution. This guide is written for Ubuntu, but
it should be easy enough to adapt the instructions for your distribution
if you know the proper commands.

## Contents
- [Installation](#installation)
  - [Before you start](#before)
  - [Installing Wine](#wine)
  - [Setting up a Bottle](#bottle)
  - [SWTOR setup](#swtor)
  - [Removing BitRaider](#bitraider)
- [Running SWTOR](#running)
  - [Script](#script)
  - [Desktop Entry](#desktop)
- [Tips](#tips)
- [Definitions](#definitions)
- [Credits](#credits)

## Installation <a id="installation"></a>
The istallation of SWTOR on a Linux system is a bit more complicated 
than just opening an executable installer, but not by too much. Just
follow these steps and you should be up and running in no-time!

### Before you start <a id="before"></a>
Before you get started on installing SWTOR, please note the following:
- SWTOR runs very badly on an nVidia GPU when using the standard 
  `nouveau` drivers. To install the `nvidia` drivers, simply install
  them in the Ubuntu driver settings. There are plently of guides online
  explaining in more detail.
- Wine bottles cannot be installed on a Fuse file-system, and it is not
  possible to use the copy of SWTOR located on a Windows-partition if 
  you are using a dual-boot.
- It is not *required* to download the whole SWTOR game data again. If 
  you already have a copy (even if on a Fuse file-system), check the 
  [Tips](#tips).
- Official game discs will not work for installation anymore.

### Installing Wine <a id="wine"></a>
First you need to install `wine`, a compatibility layer between Windows
applications and Linux. The default version of `wine` is not quite 
suitable if you want a high level of performance though. For that, 
`wine-staging` is required. `wine-staging` contains more features than
the stable release of `wine`, but it might not work with all programs.
If you are already using other Wine Bottles<sup>1</sup>.

For Ubuntu, the installation of `wine-staging` is given below. For other
distributions, please consult the official [instructions](https://wine-staging.com/installation.html).
```bash
wget -nc https://repos.wine-staging.com/wine/Release.key
sudo apt-key add Release.key
sudo apt-add-repository 'https://dl.winehq.org/wine-builds/ubuntu/'
sudo apt-get update && sudo apt-get install --install-recommends wine-staging -y
```

### Setting up a Bottle <a id="bottle"></a>
It is possible to use the default Wine installation directory (`~/.wine`) to 
install SWTOR, but personally I prefer to put it in a different directory, 
because it is such a huge installation. First, choose a directory that suits
your needs. Consider if you want to put the game on your main drive, or on
a separate drive. As said [before](#before you start), Fuse file-systems will
not work!

For this guide, I will use the location `~/Bottles/SWTOR`. Remember to replace
that path with the location of your choosing if you want to use a different
directory.

First, create the directory, and then run `winecfg` to set it up as Bottle.
```bash
mkdir ~/Bottles/SWTOR
# Replace 'user' with your username. Note the WINEARCH is 32-bit, as SWTOR is fully 32-bit.
WINEPREFIX=/home/user/Bottles/SWTOR WINEARCH=win32 winecfg
```
While the Wine Configuration Utility is still open, got to the tab Staging
and make sure to select `Enable_CSMT`. This will improve the multi-threading
of the Windows-API-translation and can improve performance significantly.
In the Applications tab, make sure that the Windows version is set to
Windows 7.

### SWTOR setup <a id="swtor"></a>
Now that you have correctly setup your Bottle, you are ready to install SWTOR
into it. First, download the official game installer from [here](http://www.swtor.com/game/download).
If you changed your downloads-folder, then make sure to change the path to the
installer file appropriately in the commands below.
Now run the setup file and install SWTOR with these commands:
```bash
# Make sure to change WINEPREFIX if you use a different folder
WINEPREFIX=/home/user/Bottles/SWTOR WINEARCH=win32 wine ~/Downloads/SWTOR_setup.exe
```
Now follow the installer instructions. Do *not* change the default installation
directory to another folder. Make sure to immediately start
SWTOR after the installation is complete by *not unchecking* the checkbox.
Upon first run, SWTOR will create an important file named `launcher.settings`
in its installation directory. Do not log in and close the launcher
after is done starting.

### Removing BitRaider <a id="bitraider"></a>
Use a text editor to open the following file: `WINE_BOTTLE_PATH/drive_c/
Program Files/Electronic Arts/Bioware/Star Wars - The Old Republic/launcher.settings`.
Now change the following values, and pay attention to the quotes:
```json
, "PatchingMode": "{\"swtor\": \"SSN\"}"
, "bitraider_download_complete": {}
, "log_levels": "INFO,SSNFO,ERROR"
, "bitraider_disable": true
```

## Running SWTOR
Starting SWTOR from your Bottle is a bit more complicated than it may seem.


## Tips <a id="tips"></a>
- Remove BitRaider as described. *Seriously*, spare yourself loads of
  wasted time and system resources.
- If you would like to record the SWTOR sound separately, it is possible
  to redirect the sound to a sound sink under Ubuntu (or another distro
  using `pulseaudio`). First, create a virtual audio cable using this 
  command:
  ```bash
  pacmd load-module module-null-sink sink_name=Virtual_Cable sink_properties=device.description=Virtual_Cable
  ```
  This will create a new audio output device named `Virtual_Cable` that 
  can be selected in the Audio tab of `winecfg`. Then you can select
  the automatically created monitor of this device as audio source in
  the recording software of your choosing. To make the setup permament
  use:
  ```bash
  echo "load-module module-null-sink sink_name=Virtual_Cable sink_properties=device.description=Virtual_Cable" >> /etc/pulse/default.pa
  ```

## Definitions <a id="definitions"></a>
In this section, terms you might not recognize are explained. If you do not
understand any terms that are not named here, then I recommend you get to
know the Linux terminal a little better first.
1. A Wine Bottle is a folder that contains a separate Wine-configuration
  for (usually) a single program.
  
## Credits <a id="credits"></a>
Special thanks to Verain, a prominent member of the Galactic StarFighter 
community, for helping me setup my Wine Bottle for SWTOR and giving me
tips on how to improve performance.
- [Virtual Audio Cable](https://onetransistor.blogspot.nl/2017/10/virtual-audio-cable-in-linux-ubuntu.html)
- [Removing BitRaider](https://www.reddit.com/r/swtor/comments/3ksypm/guide_to_permanently_removing_bitraider_and/)

