---
id: vserver-windows-palworld
title: Palworld Dedicated Server Setup
description: Information about setting up an Palworld Dedicated Server on a VPS/Rootserver - ZAP-Hosting.com documentation
sidebar_label: Palworld Dedicated Server Setup
---

## Introduction

Do you have a Windows VPS or rootserver and you want to install a Palworld Dedicated server on it? You are in the right place. In this guide we will explain the step by step process of installing this service on your server.

## Preparation

To begin with, connect to your VPS or rootserver through Remote Desktop (RDP). Use our [RDP Initial Access](vserver-windows-userdp.md) guide if you need help doing this.

Once you have accessed your server, you will need to setup **SteamCMD** in order to be able to download the necessary dedicated server files. SteamCMD is the **command-line (CLI)** version of the Steam client and is the tool which allows you to easily download a range of Steam workshop and dedicated server files. Download [SteamCMD from the official Valve website](https://developer.valvesoftware.com/wiki/SteamCMD) or directly [here](https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip).

Create a new folder somewhere on your server, in this scenario we will name it `steamcmd`. Head over to your Downloads folder, find the **steamcmd.zip** file which you just downloaded and place it into your `steamcmd` folder. Now you will have unzip the file by right-clicking and using Window's unzip functionality directly, or any range of applications such as .7zip or Winrar. This should result with a **steamcmd.exe** file being unzipped.

Simply run **steamcmd.exe** and await until the installation process is fully completed.

![image](https://github.com/zaphosting/docs/assets/42719082/ffb8e8a1-26e3-4d16-9baf-938e17ec1613)

As soon as the message **Loading Steam API.... OK** is displayed, the process has been completed successfully and you can being with the installation of the Palworld dedicated server in the following section.

## Installation

After the installation, you should be able to execute commands within the **steamcmd.exe** command prompt that you ran before. You need to login prior to being able to do anything, through the **anonymous** user, by using the command: `login anonymous`

Once logged in, you can now begin downloading the files. You can set your preferred install directory by using the command `force_install_dir [path]`, replacing path with the path you wish. This is optional, for example: `force_install_dir C:\Palworld-Server`.

Now run the command `app_update 2394010` which will begin the download. The App ID **2394010** is the **Palworld** application.

![image](https://github.com/zaphosting/docs/assets/42719082/b265a784-cf9a-43dc-b100-376f080e18f3)

:::info
Please do not interrupt the process before it is completed to avoid mistakes. It may take a moment, but it's worth being patient! :)
:::

Once successful, head over to the download directory where all of the server files have been downloaded. Here, you can use the **PalServer.exe** to start the server. However, we recommend port forwarding and configuring your server first.

### Port Forwarding your server

In order to ensure that your server is accessible to the public, you must alter port forwarding rules for the ports that the dedicated server process is using. You can do this either through Powershell commands directly which is easier, or regularly through the Windows Defender Firewall page.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="powershell" label="Via Powershell" default>

Open your Windows search box, and search for **Powershell**. Make sure to right-click and **Run as Administrator** so that permissions are accessible and everything works correctly.

:::info
Ensure you run your Powershell in Administrator mode, otherwise the settings may not apply correctly.
:::

Next, copy and paste the following commands into your Powershell Prompt:
```
New-NetFirewallRule -DisplayName "Palworld Server" -Direction Inbound -LocalPort 8221 -Protocol TCP -Action Allow
New-NetFirewallRule -DisplayName "Palworld Server" -Direction Inbound -LocalPort 8221 -Protocol UDP -Action Allow
New-NetFirewallRule -DisplayName "Palworld Server" -Direction Outbound -LocalPort 8221 -Protocol TCP -Action Allow
New-NetFirewallRule -DisplayName "Palworld Server" -Direction Outbound -LocalPort 8221 -Protocol UDP -Action Allow
```

These commands will automatically create firewall rules which are necessary for your Palworld server to be accessible to the public.

</TabItem>

<TabItem value="windefender" label="Via Windows Defender">

Use the search function on Windows to open up **Windows Firewall Settings with Advanced Security**. You may have to press **Advanced Settings** to open up the necessary window if you open up the base Windows Firewall page.

![image](https://github.com/zaphosting/docs/assets/42719082/5fb9f943-7e51-4d8f-9df4-2f5ff60857d3)

You must create new rules for your Palworld server. To do this, click on the incoming and outgoing rules as described below and add them for the following protocols and ports:
- TCP incoming and outgoing: 8221
- UDP incoming and outgoing: 8221

Please use our [Windows port forwarding guide](vserver-windows-port.md) if you need further assistance doing this.

</TabItem>
</Tabs>

Once you have added these rules, your server will now be accessible which means you will be able to connect to it through your server's IP address. You can do this by going to the server list browser and inputting your IP address and port into the search at the bottom. You should search for: `[your_ip_address]:8221`

We recommend that you configure your server settings first through the following section before accessing your server.

## Configuration

By this stage, you have finished the setup for your Palworld server. You can perform further server configuration through a configuration file found within the directory of your server.

Firstly, navigate to the following directory:
```
../steamapps/common/PalServer/Pal/Saved/Config/WindowsServer
```

You will be able to find the **PalWorldSettings.ini** configuration file. Through this file, you can configure a wide selection of parameters for your server. See [Palworld's parameter guide](https://tech.palworldgame.com/optimize-game-balance) to view all of the available options and what they each do.

:::note
If you do not see this file, please start your server at least **once** for this to be generated automatically. Alternatively, you can copy and use the default settings from the `steamapps/common/PalServer/DefaultPalWorldSettings.ini` path.
:::

## Starting & Connecting to your server

Now it is time to start your server. Head over to the base directory of your Palworld server and run **PalServer.exe** to begin the start up process. This will open the server's console in a command prompt and begin the startup process. If everthing occurs as expected, your server will be visible in the server list. Alternatively, you will be able to connect directly by opening the console in-game and running `open [your_ip_address]:8221`.

You have successfully installed Palworld on your Windows VPS.