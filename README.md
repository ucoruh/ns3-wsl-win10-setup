# ns3 Installation and Test on Windows 10 WSL2 (Windows Subsystem Linux)

## Step-1: Enable WSL on Windows 10

- You should enable WSL from Turn Windows features on or off. You can check following instruction. Also your operating system should provide WSL requirements.

[How to install Windows Subsystem for Linux (WSL) on Windows 10 | Windows Central](https://www.windowscentral.com/install-windows-subsystem-linux-windows-10)

## Step-2: Install, List, Unregister and Shutdown Commands for WSL

- Then open a powershell terminal with administrator privilege (Run as Administrator)

- Following command  list downloaded distributions on your computer

```batch
wsl --list
```

- If you use with online tag you can list all wsl distrubtions. You can select by name and install on your computer.

```batch
wsl --list --online
```

You should get output similar to following list.

```batch
The following is a list of valid distributions that can be installed.
Install using 'wsl --install -d <Distro>'.

NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Debian          Debian GNU/Linux
kali-linux      Kali Linux Rolling
openSUSE-42     openSUSE Leap 42
SLES-12         SUSE Linux Enterprise Server v12
Ubuntu-16.04    Ubuntu 16.04 LTS
Ubuntu-18.04    Ubuntu 18.04 LTS
Ubuntu-20.04    Ubuntu 20.04 LTS
```

During this installation we will use Ubuntu 20.04 (you can use latest one) with --install parameter as follow

```batch
wsl --install -d Ubuntu-20.04
```

Installation will open a terminal for Ubuntu and ask for user name and password. Please enter this required information for super user.  When we enter wsl it will kept wsl environmet up we can close wsl with --shutdown parameter. Close open terminal and open a new terminal then enter following command. 

```batch
wsl --shutdown
```

After this command you can close terminal. 

If you want to uninstall wsl environment then you can use following command.

```batch
wsl --unregister Ubuntu-20.04
```

Untill this step you will have Ubuntu-20.04 installed WSL environment but it needs some updates.

## Step-3: Setup Ubuntu Environment on WSL

Open a powershell terminal with admin privilege and write

```batch
wsl   
```

On the terminal environment you can access the linux environment.  Run following sequence

Install common tools

```batch
sudo apt-get install software-properties-common
sudo apt-get update && sudo apt-get upgrade
```

Install software

```batch
sudo apt-get install git zsh curl make build-essential 
sudo apt-get install libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl imagemagick libmagickwand-dev
```

Add <mark>/mnt</mark> to <mark>PRUNEPATHS</mark> in <mark>/etc/updatedb.conf</mark> this avoid indexing windows files and mlocate installation not hanging on too long. 

Install mlocate

```batch
sudo apt-get install mlocate
sudo updatedb
```

Install Tasksel

```batch
sudo apt install tasksel
```

Install Xubuntu desktop

```batch
sudo tasksel install xubuntu-desktop
```

Install Xfce and select `lightdm`

```batch
sudo apt install xfce4
```

After xfce4 installation you should add DISPLAY driver parameter to configuration file (Open file with following command)

```batch
sudo nano /etc/bash.bashrc
```

End of this file add following line

```batch
export DISPLAY=:0
```

CTRL+O (save) + Enter and CTRL+X (close)

You can check with sudo nano command again, that you addition is inserted to file. 



During this setup I also add this DISPLAY parameter to `~/.bashrc` and `~/.zshrc` files but I think this is not required. 



Install ns3 requirements

```batch
sudo apt update
sudo apt install build-essential autoconf automake libxmu-dev python3-pygraphviz cvs mercurial bzr git cmake p7zip-full python3-matplotlib python-tk python3-dev qt5-qmake qt5-default gnuplot-x11 wireshark
```

Close terminal and on Windows 10 install VcXsrv Windows X server

[VcXsrv Windows X Server download | SourceForge.net](https://sourceforge.net/projects/vcxsrv/)

After installation open firewall and check public private network permissions if they are missing then enable. 

Run Xlaunch and Select Disable Access Control to avoid connection issues.

Open a new powershell terminal and run wsl command to enter linux environment

and run `xfce4-session` command. If not connected then reset environment with shutdown and try again during this operation close all open terminals. 

```batch
PS C:\Users\ugur.coruh> wsl
ucoruh@LAPTOP-RQNNS9IG:/mnt/c/Users/ugur.coruh$ xfce4-session
```

If command successfully operated then you will see WSL GUI. In multiple screen setups there are some problems to fit monitor screen sizes. 

You can also install RDP client via following command to use RDP for connection

```batch
sudo apt install xrdp
```

and run service with

```batch
sudo service xrdp start     
```

you can check all services with following command

```batch
service --status-all
```

Check ip address of WSL environment with 

```batch
ip addr
```

## Step-4 ns3 Installation and Hello World

Open mozilla firefox and visit [Releases | ns-3](https://www.nsnam.org/releases/) to download ns-3 package. 

You can also download this package on Windows 10 and then copy via

/mnt/<driver name> / <windows 10 folder path> to home folder on WSL.

You can check following sample

```batch
ucoruh@LAPTOP-RQNNS9IG:/$ ls
bin   dev  home  lib    lib64   media  opt   root  sbin  srv  tmp  var
boot  etc  init  lib32  libx32  mnt    proc  run   snap  sys  usr
ucoruh@LAPTOP-RQNNS9IG:/$ cd mnt
ucoruh@LAPTOP-RQNNS9IG:/mnt$ ls
c  d  e
ucoruh@LAPTOP-RQNNS9IG:/mnt$ cd c
ucoruh@LAPTOP-RQNNS9IG:/mnt/c$ cd Users
ucoruh@LAPTOP-RQNNS9IG:/mnt/c/Users$ cd ugur.coruh/
ucoruh@LAPTOP-RQNNS9IG:/mnt/c/Users/ugur.coruh$ ls


```

enter home user directory such as (home/ucoruh) and copy ns-allinone-xx.x package here and extract here with right click then enter folder via terminal and build ns3 

```batch
cd ns-allinone-x.xx/
./build.py --enable-examples --enable-tests
```

This approximately takes 20 min. 

After build enter ns-x.xx folder and run following command to test environment

```batch
./waf --run hello-simulator
```

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/ns-3.34$ ./waf --run hello-simulator
Waf: Entering directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
Waf: Leaving directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
Build commands will be stored in build/compile_commands.json
'build' finished successfully (2.584s)
Hello Simulator
```

## Step-5 Run ns3 Examples

ns3 applications run in scratch folder under ns-x.xx folder. You can copy any example here and then run as same as follow

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/ns-3.34$ cp examples/tutorial/first.cc scratch/
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/ns-3.34$ ./waf --run scratch/first
Waf: Entering directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
Waf: Leaving directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
Build commands will be stored in build/compile_commands.json
'build' finished successfully (1.974s)
At time +2s client sent 1024 bytes to 10.1.1.2 port 9
At time +2.00369s server received 1024 bytes from 10.1.1.1 port 49153
At time +2.00369s server sent 1024 bytes to 10.1.1.1 port 49153
At time +2.00737s client received 1024 bytes from 10.1.1.2 port 9


```

## Step-6 Visualize ns3 Examples

```batch
vim first.cc    
```

add header netanim-module

```c
#include "ns3/core-module.h"
#include "ns3/network-module.h"
#include "ns3/internet-module.h"
#include "ns3/point-to-point-module.h"
#include "ns3/applications-module.h"
#include "ns3/netanim-module.h" 

...

  AnimationInterface anim("first.xml");
  Simulator::Run ();
  Simulator::Destroy ();

```

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/ns-3.34$ ./waf --run scratch/first
Waf: Entering directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
[2930/2984] Compiling scratch/first.cc
[2945/2984] Linking build/scratch/first
Waf: Leaving directory `/home/ucoruh/ns-allinone-3.34/ns-3.34/build'
Build commands will be stored in build/compile_commands.json
'build' finished successfully (8.757s)
AnimationInterface WARNING:Node:0 Does not have a mobility model. Use SetConstantPosition if it is stationary
AnimationInterface WARNING:Node:1 Does not have a mobility model. Use SetConstantPosition if it is stationary
AnimationInterface WARNING:Node:0 Does not have a mobility model. Use SetConstantPosition if it is stationary
AnimationInterface WARNING:Node:1 Does not have a mobility model. Use SetConstantPosition if it is stationary
At time +2s client sent 1024 bytes to 10.1.1.2 port 9
At time +2.00369s server received 1024 bytes from 10.1.1.1 port 49153
At time +2.00369s server sent 1024 bytes to 10.1.1.1 port 49153
At time +2.00737s client received 1024 bytes from 10.1.1.2 port 9

```

Enter NetAnim folder and run application

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/ns-3.34$ cd ../netanim-3.108/
```

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/netanim-3.108$ sudo ./NetAnim

```

If you get following error

```batch
./NetAnim: error while loading shared libraries: libQt5Core.so.5: cannot open shared object file: No such file or directory
```

I tried following command and solved in my case

```batch
ucoruh@LAPTOP-RQNNS9IG:~/ns-allinone-3.34/netanim-3.108$ sudo strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5
```

After successfull running GUI will be opened and then open first.xml file (/home/ucoruh/ns-allinone-3.34/ns-3.34/first.xml) and play animation



Thats all!!

# References

[WSL Installation · GitHub](https://gist.github.com/mohsenkhanpour/6abd8e724e08140e1bfa253112180400)

[Install Xfce / Xubuntu desktop on Ubuntu 20.04 Focal Fossa Linux - Linux Tutorials - Learn Linux Configuration](https://linuxconfig.org/install-xfce-xubuntu-desktop-on-ubuntu-20-04-focal-fossa-linux)

https://techcommunity.microsoft.com/t5/windows-dev-appconsult/running-wsl-gui-apps-on-windows-10/ba-p/1493242

[ns3 Simulator - VANET using WaveNetDevice tutorial - YouTube](https://www.youtube.com/watch?v=CkTKXYN3TZI&ab_channel=AdilAlsuhaim)

[GitHub - addola/NS3-HelperScripts: Helper Scripts to run NS3](https://github.com/addola/NS3-HelperScripts)
