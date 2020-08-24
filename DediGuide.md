## Dead Matter Dedicated Server Setup Guide - By TheKraken_ZA#0001

Dead Matter allows anyone to host a game server on their own hardware. This can be done on the same hardware you intend playing the game on, but the server is resource-intensive, and thus a separate, dedicated system is recommended. 


### 1. Port Forwarding.

The first step is to ensure the necessary ports are open on your router. The default ports you need to open are as follows:

UDP 27016 - Steam server query

UDP/TCP 7777 - Dead Matter 1

UDP/TCP 7778 - Dead Matter 2

The exact details on how to set open these ports depend on your router make and model, and may even require the assistance of your ISP. The below two links may prove useful to you in figuring out how to do this:

Steam - Configuring a Router for use with a Dedicated Server: https://support.steampowered.com/kb_article.php?ref=5121-RPXB-7955

PortForward.com: https://portforward.com/router.htm

These default ports can be modified in the game.ini and engine.ini files. Instructions to do so can be found further down in this guide.


### 2. Install SteamCMD

Create a new folder, i.e. C:\SteamCMD\

Download SteamCMD for from this link: https://developer.valvesoftware.com/wiki/SteamCMD#Windows

Extract the Zip file into the folder you created for it.


### 3. Install Dead Matter Dedicate Server app

Create a new folder for the Dead Matter Dedicated Software app to be installed to, i.e. C:\DMDS\

Right-click your windows button and click on "Run". Type in CMD and hit enter to open up your Command Prompt.

In your command prompt, type in the following command, replacing details with your own where necessary:


>steamcmd +login &lt;username> &lt;password> +force_install_dir &lt;your_directory i.e C:\DMDS\> +app_update 1110990 +quit

If prompted for your Steam Authenticator key, enter it and press enter. 

This will now install the necessary software into the folder you specified.


### 4. BAT file

Once done installing, navigate to the folder to which you installed the Dead Matter Dedicated Server app, i.e. C:\DMDS\ . Create a new text file therein, and paste the below code:


>@echo off  
>cls  
>:start  
>echo Starting server...  
>start /wait deadmatterServer.exe -log  
>echo Restarting server...  
>timeout /t 3  
>echo.  
>goto start  


Save the file as "start.bat". Make sure the extension is .bat and not .txt.


### 5. First Run

Run the new bat file. This will launch a console with the Dedicated Software. You will see many errors as it starts up (Closed Alpha...). After a minute or two, the console should slow down, and you should see lines similar to the following:


>[2020.08.23-12.41.51:904][  3]LogDMDebug: [OnSessionCreated: 143] Created session "GameSession"  
>[2020.08.23-12.41.51:905][  3]LogDMDebug: [OnSessionStarted: 160] Started session "GameSession"

At this point, close both the Server App and the .bat file.


### 6. Game.ini basics

Now navigate to the following folder inside your Dead Matter Dedicated Server app install directory, I.e C:\DMDS\deadmatter\Saved\Config\WindowsServer

Open up the game.ini file. It should be blank. Enter the following lines:

>[/Script/DeadMatter.SurvivalBaseGameState]  
>ServerName=<your server name>  
>+SuperAdmins=<your steam id>  
>ServerTags="Key:Pair"  
>ServerTags="PvP:Yes"  
>ServerTags="RP:Roleplaying"  
>MOTD=Welcome to the Dead Matter South Africa server! Warning - Due to the current state of the game, servers crash every 1-4 hours. You will keep your character when rejoining, but will respawn at a random location.  

>[/Script/Engine.GameSession]  
>MaxPlayers=36


*   _+SuperAdmins=&lt;your steam id>_ allows you to set the server super admins. You can add this line multiple times for additional Super Admins. It is suggested to be used for server owners and advanced moderators who will need access to operation server functionality such as server restarts.
*   Your steam ID can be obtained by pasting your Steam name in here: https://steamidfinder.com

    The steamID64 that is returned is your Steam ID.

*   The _ServerTags="Key:Pair"_ items listed above are examples. Replace it with your own. It needs to be in the "Key:Pair" format. The official documentation from QI Software indicates a leading “+” for the tag declaration (i.e. “+ServerTags="Abc:1"”). This seems to be incorrect and will result in tags not working.
*   _MaxPlayers=36_ is the recommended number of players recommended during the closed alpha.
*   _MOTD=_ is for the “Message of the day” displayed in the character creation screen, although this doesn’t seem to be functioning currently.

Save the file, and re-run start.bat.

Once you see the “Created session "GameSession"” and “Started session "GameSession"” lines, your server should be up and running, and visible in the in-game server browser.


### 7. Additional game.ini edits

The game.ini as it is in the previous section is all that is needed to run a dedicated server. The following settings only need to be added if needed. If you don’t need any, leave them out.

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.SurvivalBaseGameState]</span>


>+Admins=Abc  
>+Admins=Def  
>+Admins=Ghi

Adding the above lines allows you to specify the Steam IDs of normal server moderators who do not need access to operational server functionality.


>Seed=0

Server random seed. Not used much.


>bFirstPersonOnly=false 

If the server forces first person only. For gameplay reasons, this has no effect.


>bVACSecure=false

Whether or not to turn on VAC and EAC.


>bIsHardcore=false

Whether or not to turn on hardcore mode.


>MaxZombieCount=2048

The absolute hard-cap for zombie NPCs. If these many zombies are on the server, no more will be allowed to spawn.

>MaxAnimalCount=100

Above, for animal NPCs.


>MaxBanditCount=256

Above, for non-safezone human NPCs.


>PVP=true

Toggles whether or not PVP is enabled. If false, no damage is inflicted by one player on another.


>FallDamageMultiplier=1.0

Change how much damage falling does here. 1.0 is full damage, 0 is no damage at all.

If you need any of the below remaining flags below, and your game.ini doesn’t have the heading in [   ] added yet, go ahead and add it in followed by the necessary flags from the lists below:

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.SurvivalBaseGamemode]</span>


>WhitelistActive=false

If the server whitelist is enabled. If enabled, also add the below list of Whitelisted users (again using their SteamIDs:


>+Whitelist=Abc  
>+Whitelist=Def  
>+Whitelist=Ghi

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.FlockSpawner]</span>

>AnimalSpawnMultiplier=1.0 

How many more animals to spawn than usual.

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.GlobalAISpawner]</span>


>ZombieSpawnMultiplier=1.0

How many more zombies to spawn than usual.

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.Agenda]</span>


>Timescale=5.5

The timescale, relative to real time. The default value of 5.5 indicates that one real-life second is 5.5 seconds ingame.

<span style="text-decoration:underline;">Added under: [/Script/DeadMatter.ZombiePawn]</span>


>AttackMultiplier=1.0

How strongly the zombies do damage. Set to zero to disable. 


>DefenseMultiplier=1.0

How much the zombies soak up hits. Set to zero to make them made of paper.


### 8. Using non-default ports

If for any reason you cannot use the default ports, you can specify custom ports as needed. To do so, you need to add the below to your game.ini file, as well as to your engine.ini file:

<span style="text-decoration:underline;">Change Steam server query:</span>

Add the following to the **game.ini** file, using the port needed:


>[/Script/deadmatter.ServerInfoProxy]  
>SteamQueryPort=27029

You also need to add the following to the engine.ini file:


>[OnlineSubsystemSteam]  
>GameServerQueryPort=27029

<span style="text-decoration:underline;">Change the Dead Matter server port:</span>

Add the following to the **engine.ini** file, using the port needed:


>[URL]  
>Port=7790

**N.B. Make sure you update your port-forwarding rules according to your changes above!**

_Disclaimer: The above guide is correct as of 24/08/2020. Changes to the software by the developers may result in some of the above information being out of date._

_With thanks to @Cas#0789 for pointing me in the right direction._
