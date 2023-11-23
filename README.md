Personal cheatsheet, workaround notes, open source homebrew apps testing, issues & image quality improvements

### Format Structure:

> [!IMPORTANT]
> Wii U: This console has a very low USB amp, so you will need a SATA-USB Y cable for a HDD

> [!NOTE]
> Wii U: To connect two devices (HDD + Pendrive / HDD + HDD) use an external USB charger

> [!NOTE]
> Wii U: Use **U-Stealth** on the WiiVC device in order to **skip** the **Wii U format popup** every time you turn on the console

> [!NOTE]
> Wii: Prefer to connect the USB to the bottom or near the end, as this is the default location on most launchers.

 ### Format Structure
   1. Wii (USB Loader GX)
      - 128GB Pendrive (Connected on bottom)
      - Partition:
        - FAT32 32K (30GB), Primary, Active <sub><- For GC, HB apps, configs...</sub>
        - NTFS (90GB), Primary <sub><- For Wii</sub>
  2. Wii U (WiiVC/USB Loader GX)
     - 64GB Micro SD (Connected on SD Port)
     - 500GB HDD + SATA-USB Y cable + Charger. (Connected on bottom)
     - 128GB Pendrive (U-Stealth enabled) (Connected on middle)
     - Partition:
       - 64GB Micro SD
         - FAT32 with the highest possible cluster size <sub><- to transfer your Wii U.</sub>
       - 500GB HDD
         - Wii U special format (Just connect the drive to the console and format it) <sub><- For Wii U</sub>
       - 128GB Pendrive
         - FAT32 32K (30GB), Primary, Active <sub><- For GC, HB apps, etc...</sub>
         - NTFS (90GB), Primary <sub><- For Wii</sub>
   3. Wii U (WiiVC Injects)
      - 64GB Micro SD (Connected on SD Port)
      - 500GB HDD + SATA-USB Y cable. (Connected on both usb)
      - Partition:
        - 64GB Micro SD
          - FAT32 with the highest possible cluster size <sub><- to transfer your Wii U and Wii/GC Injects.</sub>
        - 500GB HDD
          - Wii U special format (Just connect the drive to the console and format it) <sub><- For Wii U, Wii/GC Injects</sub>

## Possible Encounters
1. Usb Loader GX. White Screen / Empty UI
   - Cause: NTFS partition without 'wbfs' folder.
   - Solution: I had to create an empty 'wbfs' folder in.

2. USB Loader GX -> Nintendont. Nintendont "No FAT device found!"
   - Apparently, the issue seems to be related to the cluster size or fragmentation. Reformatting it with a 32K cluster size on FAT32 solved the problem.
   - Prefer using 1:1 (Full size) over trimmed ones.

3. Pendrive/HDD got corrupted (A lot of folder with random characters + wrong partition size)
   - Cause: Disconnecting the device with the Wii powered on.
   - Solution: Shutdown the Wii before disconnecting the device.
  
4. Capture Card issues on 480i/480p/PAL/NTSC
   - Cause: Your Capture Card is PAL/NTSC or i/p format only.
   - Solution 1: Use **AnyRegion Changer** to change the video mode for your entire Wii system.
   - Solution 2: Use **USB Loader GX** to change the video mode for specific games or globally.

## Deflickering, Image Quality, Widescreen, Framebuffer...
1. Wii
   > For deflickering, frame buffering and resolution, use USB Loader GX.
2. Wii U (WiiVC Mode)
   > For deflickering, frame buffering and resolution, use USB Loader GX.
3. Wii U (WiiVC Injects)
   >  Since WIIVC Injects can't utilize USB Loader GX, alternative methods are needed to apply the Deflicker, Framebuffer and Resolution patches
   - Deflickering (ISO Patching)
      - Parse WBFS into ISO
      - Open ISO with Wii Scrubber
      - Open main.dol with a HEX editor
      - To deflicker NTSC 480i?? 
        - Find all **`"08080A0C0A0808"`** and **`"04080c100c0804"`**, then replace it with **`"00001516150000"`**
      - To deflicker progressive
        - Find **`"9109800041820040"`** and replace it with **`"9109800048000040"`**
   - Framebuffer
     > Currently, there is no way to implement a Framebuffer; however, if you use Aroma, you can apply this width enhancement for WiiVC.
     - https://github.com/GaryOderNichts/evwii
     - Put the .wps on wiiu\environments\aroma\plugins\
   - Resolution
     > 480p may be better than 1080p in certain TV models when using WiiVC, as it reduces aliasing and provides smoother graphics. If you are using Aroma, consider applying the AutoLaunch resolution plugin to launch WiiVC Injects at 480p.
     - https://github.com/Lynx64/WiiVCLaunch
     - Put the .wps on wiiu\environments\aroma\plugins\
4. Gamecube (Widescreen, ISO Patching)
   - Search for a **Gecko Widescreen Code** for your game
   - Search for a **GCT Maker/Generator** to create **.gct** file with that code
   - Use **`gc-tool`** to open your **.iso** file, then **`Extract whole ISO...`**
   - Use **`GeckoLoader`** to load both the **.dol** and **.gct** files in order to generate a **patched .dol**
   - Replace the **original .dol** file with the **patched .dol**
   - Use **`GCRebuilder`** to create a new **.iso** from the extracted folder
5. Gamecube (Widescreen, Nintendont)
> [!WARNING]
> Don't enable the **`Widescreen option`** in the Nintendont Config File as it uses a **generic widescreen code** that causes **visual glitches**.
   - Search for a **Gecko Widescreen Code** for your game
   - Search for a **GCT Maker/Generator** to create **.gct** file with that code
   - Enable **`Cheats`** in the Nintendont Config file
   - Place the generated **.gct** file in the following format: **`/codes/GAMEID.gct`**
