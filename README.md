# SwitchSound

**SwitchSound** is a plugin that offers the possibility to create buttons that activate the playback of audio files present locally.
Each button is linked (and will play) a single audio file or a folder of audio files in shuffle.
In the latter case, pressing the button will randomly select and play one of the audio files present in the specified path.
For each button you can adjust the volume of the audio playback. The volume cannot be adjusted during playback but only before pressing the button.


## Install Omxplayer
Log in ssh to the raspberry and install omxplayer with this command. Omxplayer is the music player that will play your sounds.

```code
sudo apt-get install omxplayer
```

## Upload your sounds on Pi
Log in ssh to the raspberry and create a folder "sounds" in this path:

```code
/home/pi/
```

To create folders and transfer your audio files, you can use an SFTP application. I use Cyberduck.
https://cyberduck.io/

## How use SwitchSound
After installing the player and uploaded our favorite sounds, let's create the Homekit buttons corresponding to our audio files.
Let's open the Homebridge configuration and create the new SwitchSound platform as follows:

```code
{
    "platform": "SwitchSound",
    "name": "SwitchSound",
    "debug": false,
    "defaultSoundPlayer": "/usr/bin/omxplayer.bin",
    "enableAlsaOutput": false,
    "accessories": [
        {
            "accessory": "SwitchSound",
            "id": "000000000001",
            "name": "Server Pronto",
            "soundFile": "/home/pi/sounds/restart/",
            "soundOptions": [],
            "shuffle": 1,
            "loop": false,
            "volume": 66,
            "debug": false,
            "sequence": false
        },
        {
            "accessory": "SwitchSound",
            "id": "000000000002",
            "name": "Allarme",
            "soundFile": "/home/pi/sounds/varie/AlarmSound2.mp3",
            "soundOptions": [],
            "loop": false,
            "volume": 66,
            "debug": false,
            "sequence": false
        }
    ]
}
```

## Parameters
* `accessory` \<string\> **required**: identifies the accessory. It must always be SwitchSound
* `id` \<string\> **required**: identifies the serial code of the accessory. You can write whatever you want
* `name` \<string\> **required**: identifies the name of the button that will appear in HomeKit
* `shuffle` \<string\> **required**: identifies the path of the audio file to be connected and playing. In case of multiple files in shuffle, you can stop at the folder
* `soundOptions` \<string\> **optional**: For the more experienced, it is possible to pass additional parameters to omxplayer
* `shuffle` \<string\> **optional**: activates shuffle mode. In this case it is necessary to indicate the path of a folder within the soundFile parameter
* `loop` \<string\> **optional**: play the audio file endlessly until the user turns off the button from HomeKit
* `volume` \<string\> **required**: It is necessary to indicate a starting volume
* `debug` \<string\> **optional**: enable debug mode with more output instructions
* `sequence` \<string\> **optional**: offers the possibility to play an audio file several times consecutively. Useful for short audio files
