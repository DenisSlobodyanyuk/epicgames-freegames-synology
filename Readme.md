# Epic Games Store Weekly Free Games

Automatically login and redeem promotional free games from the Epic Games Store.
Handles multiple accounts, 2FA, captcha bypass, captcha notifications, and scheduled runs.

Fork from https://github.com/claabs/epicgames-freegames-node/issues because I had trouble figuring out how to set this up on my Synology. 
https://git.sr.ht/~nitanmarcel/epicgames-freegames-node-telegram#json-configuration - this helped a lot

# My steps

### Install Docker
  Just go to Package Center
  Search for Docker
  Install Docker

### Enable SSH(optional) - I figured it would be easier this way than figuring out how to set environmental variables throught the app
  Go to Control Panel
  Go to Terminal & SNMP
  Check box for "Enable SSH service"
  Set port for connecting
  PS - Make sure that in Users & Groups, the user you're going to use had SSH enabled in Applications tab, and that the group that said user is assigned to, is not blocking SSH

### Folder structure
  Go to Control Panel
  Go to Shared Folder
  Create a folder for your containers, I called mine Docker
  Exit Control Panel
  Open File Station
  Go to the folder you created
  Create a subfolder for this container, name it whatever you want, you will place the config.json file in here 
  
### config.json - This is what mine looks like, anything in the line starting with // is a comment. You can remove these lines when creating your config.json file. 

---------------------------------------------------------------------------------------------------------------------------------------------------------
  {
  "accounts": [
  // Multiple accounts can be configured here. If you have 2fa on your account, either disable it, or look into totp on https://git.sr.ht/~nitanmarcel/epicgames-freegames-node-telegram#json-configuration 
    {
      "email": "account1@website.com",
      "password": "password",
    },
    {
      "email": "account2@website.com",
      "password": "password",
    },
  ],
  //(Optional) promotion - the script will redeem all free games in the Epic Games catalog. To only redeem the weekly promotions, set to weekly
  "searchStrategy": "promotion",
  //(Optional) If true, the process will run on container startup in addition to the scheduled time
  "runOnStartup": false,
  //(Optional) intervalTime controls the script execution interval of multiple accounts in seconds. (Only effective when multiple accounts are configured using config.json)
  "intervalTime": 60,
  //I read what it's supposed to mean, but I don't actually know right now. If anyone knows how to make changes to this, to run every thursday and tuesday, let me know. 
  "cronSchedule": "0 12 * * *",
  //(Optional) If true, don't schedule runs. Use with RUN_ON_STARTUP to run once and shutdown.
  "runOnce": false,
  //(Optional) Log level in lower case. Can be [silent, error, warn, info, debug, trace]
  "logLevel": "info",
  "webPortalConfig": {
    //set your synology's ip:3000, or whatever port you set
    "baseUrl": "http://10.0.0.13:3000",
  },
  "notifiers": [
  // You may configure as many of any notifier as needed, I set up discord webook because email wasn't working. You can see how to set up webook
  {
  "type": "discord",
    "webhookUrl": "https://discord.com/api/webhooks/197349123471239087/v32h1e9S-asdfasdfasdfasdfq23r23rtgfre-sd90ucuhnfqop9f08",
  },
  ],
}
------------------------------------------------------------------------------------------------------------

Place this file in the folder you just created

### Deploy container

SSH into your synology by using either, putty or powershell(will need install) on windows/terminal on macOS and Linux. ex "ssh user@10.0.0.13 -p 22"
run "docker run -d -e TZ=America/Chicago -v /volume1/<docker folder name>/<container folder name>:/usr/app/config:rw -p 3000:3000 charlocharlie/epicgames-freegames:latest"


### How to use

The program will start signing into your epic accounts and will claim the free games. When your account requires a capcha to be solved, either on sign in or when claiming a game, you will receive a notification with a link to click and solve the capha manually.
