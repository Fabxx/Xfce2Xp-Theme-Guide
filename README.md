# Initial setup

git clone --recursive https://github.com/rozniak/xfce-winxp-tc

cd xfce-winxp-tc/packaging

./buildall.sh, if errors happen install the missing depencies given by the log.

a `xptc` folder will be created, go into it

install all the packages created by the builder, based on your distro they will have different format 

(for example, debian will have deb packages)

# Theme Setup

Go into Aspect and select these:

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/4a762d25-ab6d-41ec-9140-2cc184d03171)

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/68b55055-025f-4254-9f56-74fa2b4becd1)

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/ec0095f3-cdfd-4307-9c9c-566cce79dc96)

Go to Window manager and select between these new options

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/389df61d-15f2-4695-8eb8-6e3efb8a3ce1)

For the mouse cursor go into mouse and touchpad and select this:

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/9394494a-3ba6-4a3b-b7b5-c035540e94e6)


# Sounds - Part 1

install mpv media player: `sudo your-package-manager-command mpv`

go into `sessions and startup` and then into `Automatic startup` tab

add these XP events:

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/3611a3f7-7f66-4ff9-93a5-2aeb701a532f)


```
XP Logoff Sound - on exit
XP Logoff (user change) sound - on exit
XP Logon/Boot Sound - on access
XP TaskBar - on access
```

# Sounds - Part 2

give to the following events this mpv command:

`XP Logoff Sound: mpv --no-video "/path/to/audiofile"`

`XP Logoff (user change) sound: mpv --no-video "/path/to/audiofile"`

# Sounds - Part 3 (Specific behaviour)

we can't distinguish logon and boot, because both happen when we login with credentials, resulting in a mixed up
sound. But we can create a script that creates a file in /tmp as a flag, if that file is available, then on next login
it will play the logon sound, if not it will play the boot sound. edit the `xp-startup_logon-sound.sh` script
by simply modifying the path to the audio file to play. After this, edit the event like so:

`XP Logon/Boot Sound: bash "/path/to/xp-startup_logon-sound.sh"`

For the taskbar, it's simply a terminal command that automatically runs on boot:

`XP TaskBar: wintc-taskband`

the shutdown/reboot/halt sound doesn't play in time because systemd gets the termination signal too fast
and we can't hear the sound because it kills every process. To avoid this, we create a systemd service, and 
we use another sh script.

`sudo touch /etc/systemd/system/XPShutdown.service`

`sudo nano /etc/systemd/system/XPShutdown.service`

copy-paste this content, and edit the `ExecStart path:

```
[Unit]
Description=Play XP sound on shutdown-reboot-poweroff-halt
DefaultDependencies=no
Before=poweroff.target reboot.target halt.target shutdown.target

[Service]
Type=oneshot
ExecStart=/path/to/xp-play-shutdown.sh
TimeoutStartSec=0

[Install]
WantedBy=poweroff.target reboot.target halt.target shutdown.target
```

This calls our sh script that plays the XP shutdown audio file, only in reboot/shutdown/halt events.

setup the `xp-play-shutdown.sh` script, again same as before, change the mpv path file and
place it where you want, then in `ExecStart` give the path to the sh file.

now do:

`sudo systemctl enable  /etc/systemd/system/XPShutdown.service`

`sudo systemctl start  /etc/systemd/system/XPShutdown.service`

Now when you do one of the actions, the system will wait 2 second, play the audio, and then shutdown.

NOTE: If you do again the systemctl start command, you will hear the windows XP shutdown. To avoid so, delete
the file created by the script `/tmp/shutdown.log`

# Sounds - Part 4 (Sound theme pack)

note, not all apps use these notification sounds, so not all app will produce a sound.

install all canberra GTK packages: sudo your-package-manager-command libcanberra*

then `export GTK_MODULES=canberra-gtk-module`

go into `xfce4-settings-editor` and find `xsettings`

find `Net` section and change the parameters like these:

![immagine](https://github.com/Fabxx/Xfce2Xp-Theme-Guide/assets/30447649/fa9b5984-33aa-457a-b8d3-997b4cb7a680)

now logoff, logon and to test the sound execute the "run" command from terminal and type a wrong command, you should hear a 
critical error sound. Also try to delete a file on your desktop














