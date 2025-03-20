* Installation
This installation is written for Matlab R2022b.

** Download the zip archive
Download the zip archive from to official website of [[https://matlab.mathworks.com/][Matlab]].
You are going to need an account which includes having a license.
The following instructions assume that you downloaded the .zip into your =Downloads= folder.

** Create two new folders
```bash
  cd ~/Downloads/
  mkdir matlab
  mkdir TMP
```

** Prepare for extraction
```bash
  mv matlab_R2022b_glnxa64.zip matlab/
  cd matlab/
```

The reason I do this is so I don't unzip matlab into my Downloads folder which would be a huge mess.

** Unzip the archive
```bash
  unzip matlab_R2022b_glnxa64.zip
```
Nothing out of the ordinary here.

** Start the installer
```bash
  sudo ./install -downloadFolder /home/<user>/Downloads/TMP/
```
For some reason I could only get this to run successfully with the =-downloadFolder= flag. I have no idea why but this seams to fix the issue.
You need to run the install script with elevated privileges so that the installer can create the directory =/usr/local/MATLAB/= und create symlinks to =/usr/local/bin=. I would recommend not changing anything because the installer has a mind of its own and anything I change let to failure.

** Run through the installer
*IMPORTANT!*: Tick the option to create symlinks to your =$PATH=. For some reason this is not ticked on by default. If you wish to launch the matlab application without typing the full path or creating symlinks manually this is recommended.
*ALSO IMPORTANT!*: Your login/user name should be the same as your =whoami= information.
During the installer your only option is to pray to god that it doesn't freeze or crash another way because there is nothing you can do.

This should successfully download all the files and install Matlab if you are lucky.


* Activate Matlab
```bash
  cd /usr/local/MATLAB/R2022b/bin/
  sudo ./activate_matlab.sh
```
Although you already entered your credentials twice by downloading the zip archive and during the installer you still need to prove that you have a license one more time.
For what ever reason the developers of this piece of trash have decided that they were not able to do that during the installer just to make your life a little bit harder.
Assuming you have installed to the standard path and you have ticked the option to create symlinks to your =$PATH= the activate_matlab.sh script should launch just fine if you run it with elevated privileges.
Enter your credentials. Hopefully you remember the user name from the install wizard. Otherwise you are in trouble.


* Unable to open or create live scripts
```bash
  cd /usr/local/MATLAB/R2022b/bin/glnxa64/
  sudo rm libfreetype.so*
```
The same library incompatibility that the installer had still exists. You are propably not gonna be able to open or create new live scripts. To fix this issue remove the according files in the installation path.
This is only a problem if you intend to use the shitty IDE that Matlab comes with. That's fine if you want to walk your dog for two ours to skip the input delay. If you intend to use a real text editor follow the following step.

Desktop entry

Optionally create a desktop entry. The MIME type of MATLAB files is text/x-matlab.

Start matlab with:

    -desktop to run Matlab without a terminal.
    -nosplash to prevent the splash screen from showing up.
    -useStartupFolderPref to use the folder specified by the Initial working folder preference [1]

In order for icons to appear correctly StartupWMClass needs to be set in the desktop entry. To find it out start MATLAB, run xprop | grep WM_CLASS and select the MATLAB window.

Example desktop entry:

/usr/share/applications/matlab.desktop

[Desktop Entry]
Type=Application
Terminal=false
MimeType=text/x-matlab
Exec=/usr/local/MATLAB/R20xyz/bin/matlab -desktop -useStartupFolderPref
Name=MATLAB
Icon=matlab
Categories=Development;Math;Science
Comment=Scientific computing environment
StartupNotify=true

If one need to set environment variable, one could prepend env in Exec, for example, to system's libfreetype:

Exec=env LD_PRELOAD=/usr/lib/libfreetype.so.6 matlab

One might want to use the system's libstdc++. 