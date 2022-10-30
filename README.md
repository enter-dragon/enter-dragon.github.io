# Building Depthboot
Due to licensing restraints, Depthboot cannot be distributed as an iso. Instead, it has to be build locally on the device.

## Instructions:

**If running under crostini(aka "Linux" on ChromeOS)**, follow [these instructions](/extra-pages/crostini.md?id=crostini-specific-instructions) first.

1. Open the terminal, clone the repo and start the script with this command:
    ```
    git clone --depth=1 https://github.com/eupnea-linux/depthboot-builder; cd depthboot-builder; ./main.py
    ```
2. Follow the instructions inside the Terminal.

3. If you chose the image option, flash the image to a USB-stick/SD-card using Etcher, Rufus, DD, or any other tool.
    - If you're running this within Crostini, copy it to a folder you can access from ChromeOS's Files App and then change the `.img` file's extension to `.bin`.
    - You can then [flash](https://www.virtuallypotato.com/burn-an-iso-to-usb-with-the-chromebook-recovery-utility/) it by using the Chrome Recovery Tool.
    
4. [Enable Devloper mode](https://www.androidauthority.com/how-to-enable-developer-mode-on-a-chromebook-906688/) now, if you havent done so yet.

5. Boot into ChromeOS, open the shell by pressing <kbd>CTRL</kbd><kbd>ALT</kbd><kbd>T</kbd>, enter `shell` and press <kbd>Enter</kbd>.  

6. Inside the chromeos shell enable USB and Custom Kernel Booting by running:
    ```
    sudo crossystem dev_boot_usb=1; sudo crossystem dev_boot_signed_only=0; sync
    ```

7. Reboot with the USB plugged in and press <kbd>CTRL</kbd><kbd>U</kbd> or select "External". 

If you selected Fedora, the system will need some time to relabel all files. DO NOT TURN IT OFF 

After a short black screen Depthboot should boot.

## Enable audio
To enable audio on Eunea, follow the instructions below:

1. Boot into Depthboot

2. Make sure you are connected to the internet.

- Skylake, Kaby Lake, or Coffee Lake (7th/8th Gen Intel CPU):
  1. Switch to alt kernel by running: ``update-kernel --alt`` in the Terminal.
  2. Reboot Depthboot
  3. Run ``setup-audio`` in the Terminal.
  4. Reboot again
- Everything else: 
  1. Run: `setup-audio` in the Terminal.
  2. Reboot Depthboot

If audio still doesn't work, please open an issue with your device codename and generation.

> #### Skylake (SKL) / Kabylake (KBL) disclaimer
>
> If you have a Skylake or Kabylake device, do not change the UCM files (`/usr/share/alsa/ucm2/`) in an attempt to use PulseAudio. If you have no idea what any of these are, you can safely ignore this.
>
> PulseAudio, without UCM modifications, errors out. If you modify the UCM to remove the `Front Mic`, `Rear Mic`, and `Mic` (all of these are related to PCM3 on `da7219max`), PulseAudio and general audio will work, but your speakers **will be fried** or their membranes **will burst**.

## Optional:
#### ZRAM(aka swap in ram):
The commands below will create 6GB of memory compressed to 2GB.  
To enable "extra" memory run the following commands in the terminal:  
1. ``sudo modprobe zram``
2. ``SIZE=6144 # change the size here if you want more/less swap memory``
3. ``sudo echo $(($SIZE*1024*1024)) > /sys/block/zram0/disksize``
4. ``sudo mkswap /dev/zram0``
5. ``sudo swapon /dev/zram0 -p 10``  

#### Fan control/ectool:
To install the chromeos ectool utility run: ``install-ectool``

#### Installing to internal storage

This will wipe ChromeOS(or any other currently installed OS). It is always possible to [restore ChromeOS using a recovery USB](https://support.google.com/chromebook/answer/1080595?hl=en). It's recommended to [setup audio](?id=enable-audio) and confirm all hardware is working correctly(touchpad, touchscreen, speakers) before proceeding. If you are sure you want to install to internal storage, run ``install-to-internal`` in the terminal.
