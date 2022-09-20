# Epic Games Store Weekly Free Games
  Automatically login and redeem promotional free games from the Epic Games Store.
  Handles multiple accounts, 2FA, captcha bypass, captcha notifications, and scheduled runs.

  Fork from https://github.com/claabs/epicgames-freegames-node/issues because I had trouble figuring out how to set this up on my Synology. 

  https://git.sr.ht/~nitanmarcel/epicgames-freegames-node-telegram#json-configuration - this helped a lot

# My steps

## Install Docker
######   Just go to Package Center
######   Search for Docker
######   Install Docker

## Enable SSH(optional) - I figured it would be easier this way than figuring out how to set environmental variables throught the app
######   Go to Control Panel
######   Go to Terminal & SNMP
######   Check box for "Enable SSH service"
######   Set port for connecting
######   PS - Make sure that in Users & Groups, the user you're going to use had SSH enabled in Applications tab, and that the group that said user is assigned to, is not blocking SSH

## Folder structure
######   Go to Control Panel
######   Go to Shared Folder
######   Create a folder for your containers, I called mine Docker
######   Exit Control Panel
######   Open File Station
######   Go to the folder you created
######   Create a subfolder for this container, name it whatever you want, you will place the config.json file in here 
  
## config.json
######   Download config.json file
######   Make changes you need
######   Place this file in the folder you just created

## Deploy container
###### SSH into your synology by using either, putty or powershell(will need install) on windows/terminal on macOS and Linux. ex "ssh user@10.0.0.13 -p 22"
run "docker run -d -e TZ=America/Chicago -v /volume1/<docker folder name>/<container folder name>:/usr/app/config:rw -p 3000:3000 charlocharlie/epicgames-freegames:latest"

# How to use
##### The program will start signing into your epic accounts and will claim the free games. When your account requires a capcha to be solved, either on sign in or when claiming a game, you will receive a notification with a link to click and solve the capha manually.
