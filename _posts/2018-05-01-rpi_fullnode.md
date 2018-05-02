---
title:  "Full node on Raspberry Pi 3"
date:   2018-0405-01 21:00:00 -0500
permalink: /install_fullnode_on_rpi/
author: TjadenFroyda
categories: install
post_importance: 8
toc: true
toc_label: "Full Node on RPi"
toc_icon: "x"
---

# Setting up the Stratis Full Node on a Raspberry Pi 3

This article will show you how to install the Stratis Full Node on your Raspberry Pi 3. This How-to was originally posted on [Reddit](https://www.reddit.com/r/stratisplatform/comments/8d3g2a/how_tofull_node_beta_gui_staking_on_raspberry_pi_3/) but has been updated and reformatted for the SDF website. 

The article builds upon the excellent [RPi tutorial by olcko and demon](https://olcko.gitbooks.io/staking-stratis-on-a-raspberry-pi/content/) and will take over from the previous tutorial's step 4. Steps 1-3 cover installing Raspbian and SSH on the RPi and are _excellent_.



## Prerequisites
* Raspberry Pi 3. _May work on other RPi devices_ 
* At least 1.5GB of free space for block storage.
* dotnet-sdk _(The project must be built on another machine)_
* git
* ssh
* scp
* screen (optional) `sudo apt-get install screen`
* expect (optional) `sudo apt-get install expect`


## Full Node Setup
At the time of this writing, **no stable dotnet-sdk exists for linux-arm.** The original Reddit how-to referenced the dotnet-sdk-2.2.100 nightly builds, however these caused daily node crashes during testing and I do **not** recommend. This section should not be completed on your RPi. 

### Downloading the source
* The following instructions are loosely based on this [MSDN post](https://blogs.msdn.microsoft.com/david/2017/07/20/setting_up_raspian_and_dotnet_core_2_0_on_a_raspberry_pi/)
* Starting in your home folder, make a new folder for your full node's files
```
mkdir StratisFullNode
cd StratisFullNode
```
* Follow the instructions [here](https://github.com/stratisproject/StratisBitcoinFullNode/blob/master/Documentation/getting-started.md)
```
git clone https://github.com/stratisproject/StratisBitcoinFullNode.git
cd StratisBitcoinFullNode
git submodule update --init --recursive
cd src
dotnet restore
dotnet build
cd Stratis.StratisD
```

### Building for RPi deployment
From within `StratisBitcoinFullnode/src/Stratis.StratisD` issue the following command to build your RPi app:
```
dotnet publish -r linux-arm 
```
The last command will build a deployable linux-arm application in `./bin/Release/netcoreapp2.0/linux-arm/publish`. 

Now copy the entire directory over to your RPi. For this example, I am storing the app at ~/Stratis/ on the RPi. 
```
cd bin/Release/netcoreapp2.0/linux-arm/publish
scp -r * pi@<Local-RPi-IP>:~/Stratis/
```

### Installing Linux-ARM .NET runtime on the RPi 
To install the dotnet 2.0 linux-arm runtime, I created this helper script:
```
#!/bin/bash
cd ~/Download
curl -sSL -o dotnet.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Runtime/release/2.0.0/dotnet-runtime-latest-linux-arm.tar.gz
sudo mkdir -p /opt/dotnet && sudo tar zxf dotnet.tar.gz -C /opt/dotnet
sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```

### Starting the Node
To minimize downtime, I created a script to monitor and restart the node if it freezes: (Replace \<Walletname\> with your wallet's name)

`/home/pi/bin/startStratisNode`
```
#!/bin/bash
cd /home/pi/Stratis
echo "Cleaning up running dotnet processes"
killall -9 dotnet 2> /dev/null
echo "Enter password:"
read -s PASSWORD
PROCESS_TO_MONITOR="dotnet ./Stratis.StratisD.dll '-apiuri=http://0.0.0.0:37221 -stake=1 -walletname=<Walletname> -walletpassword=$PASSWORD -v m'"
OUTPUT_FILE=/tmp/stratis-output.log
SUBSTRING="Stratis.Bitcoin"
ERRORSTRING="Error"
TIMEOUT=240
pid=""
while true
do
  if [ -z "$pid" ]; then
    echo "Node not running. Starting..."
    $PROCESS_TO_MONITOR >> $OUTPUT_FILE &
    pid=$!
  fi
  TAIL_AND_GREP="tail -n 150 -f $OUTPUT_FILE | grep -m 1 '$SUBSTRING' | grep -m 1 -v '$ERRORSTRING' > /dev/null"
  COMMAND="/bin/sh -c \"$TAIL_AND_GREP\""
  expect -c "set echo \"-noecho\"; set timeout $TIMEOUT; spawn -noecho $COMMAND; expect timeout { exit 1 } eof { exit 2 }"
  TIMEOUT_EXIT_CODE=$?
 
  if [ $TIMEOUT_EXIT_CODE = 1 ]; then
    echo "Error or $TIMEOUT second timeout reached. Restarting node."
    kill $pid > /dev/null 2>&1
    wait $pid 2> /dev/null
    killall -9 dotnet 2> /dev/null
    pid=""
    TIMESTAMP=$(date +%s)
    cp $OUTPUT_FILE /tmp/stratis-error-$(TIMESTAMP).log
    truncate -s0 $OUTPUT_FILE
  elif [ $TIMEOUT_EXIT_CODE = 2 ]; then
    echo "Node is running. Re-poll in 60 seconds."
    sleep 60
  fi
done
```
This script:
* Starts the node with staking enabled
* Enables API access from other devices on your local network
* Monitors the node and restarts if hung/frozen
* Outputs errors to a log file with timestamp

If you don't plan on using RPC, make sure you follow the instructions from the [previous tutorial step 3](https://olcko.gitbooks.io/staking-stratis-on-a-raspberry-pi/content/connect-with-ssh.html) to block RPC connections on your RPi

`sudo ufw deny 16174`

If you use SSH to access your RPi, I highly recommend that you use [Screen](https://www.gnu.org/software/screen/manual/screen.html). Screen is a wonderful utility that will leave your session running when you disconnect from SSH. This is available via `sudo apt-get install screen` on your RPi. 

To start a session, type:
```
screen
```
This brings you to a new bash prompt. To start the node using the script above:
```
startStratisNode
```
You can detach from the screen with `CTRL+a d`. This allows you to reconnect to the same session at another time with:
```
screen -x
```

That's it! Your node should be running now, but may take a while to sync. You can test the node's API by pointing your browser to:
```
http://<Local-RPi-IP>:37221/swagger/
```

### Monitoring the node
If you want to monitor the Node output, this can be done by tailing the output file set in the `startStratisNode` script. 

First, setup another screen: `CTRL+a c`. 
* You can rename the active screen with `CTRL+a A` and toggle between screens with `CTRL+a [0-9]`, where the number is the target window number. See the screen manual for more details. 

The following script can be ran in this second window:
`/home/pi/bin/monitorStratisNode`
```
#!/bin/bash
tail -f /tmp/stratis-output.log 2> /dev/null
```

 
### Killing the node
If you used the helper script `startStratisNode` above, you'll notice a nice infinite loop is used to maximize uptime. You can use the script below to kill the node. (Yes, some of the commands are redundant). 

`/home/pi/bin/killStratisNode`
```
#!/bin/bash
killall -9 startStratisNode 2> /dev/null
killall -9 dotnet 2> /dev/null
```

## Full Node GUI Setup

Now that the Core Daemon is running, we can install the GUI frontend. We will be following the instructions from [here](https://github.com/stratisproject/FullNodeUI). The GUI that interacts with the node can be installed on the RPi **or** another device on your network (with some tweaks to the underlying code, below).

```
cd ~
mkdir StratisFullNodeUI
cd StratisFullNodeUI
git clone https://www.github.com/stratisproject/FullNodeUI
cd FullNodeUI/FullNodeUI.UI
npm run mainnet
```

That's it! Go ahead and set up your new wallet with that shiny UI!

Follow the instructions on the [StratisProject GitHub](https://github.com/stratisproject/FullNodeUI) to build the UI for production for your device. 
* For example, the executable for linux device can be found at `./FullNodeUI/FullNode.UI/app-builds/linux-unpacked/stratis-core`
    
### Tweaks for headless RPi with local network GUI enabled
This section describes some slight modifications to the source code that will allow you to run the new FullNodeUI on a different machine on your local network. This is possible because the FullNodeUI interacts with the FullNode via the API (not the RPC server). 

By default the API is bound to http://localhost, which prevents connections from other LAN devices. Our modifications will change that default behavior. _Please do not enable remote port forwarding to your RPi. Disaster awaits you._

1. There is one place in the code where you need to replace "localhost" with your RPi's local private network IP address. 

`src/app/shared/services/api.service.ts#38`
```
this.stratisApiUrl = 'http://<Local-RPi-IP>:' + this.apiPort + '/api';
```
2. You will also need to comment out a few sections to prevent the GUI from starting up or shutting down the full node when you open/close the GUI. This is redundant because the node is already running in the background on your RPi and will continue to run after you close the GUI.

`main.ts#86`
```
//startStratisApi();
```
`main.ts#96`
```
//closeStratisApi();
```
3. If you have built the UI for production, you will have to run the build process again for your device after these changes are made. 

Article written by @TjadenFroyda (can be contacted on our [Discord](/discord/)). Tip @TjadenFroyda with Stratis [here](https://chainz.cryptoid.info/strat/address.dws?SNSwQVvB5FB6KPVT7325tJGWXbxVd4xceR)
{: .notice--info}
