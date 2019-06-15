# How to disable your unsupported GPU for macOS
So you got your shiny new RTX  2080ti BLOWYMATRON edition but you've probably noticed there's currently no support for your GPU in MacOS Mojave. Well for those who are running Maxwell, Pascal or Turing GPUs there is still some hope for you with options to spare!

# Prerequisite

* A Hackintosh built off the [Vanilla guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)(This is a must as you need to have a clean system for this to work properly)
* A GPU that is support in Mojave(generally this will be an iGPU for most users here, check the [Mojave GPU buyers Guide](https://www.reddit.com/r/hackintosh/comments/b91vf5/mojave_gpu_buyers_guide/) to see if your GPU is supported. Keep in mind that using a Kepler GPU with Maxwell, Pascal or Turing GPUs can cause issues)
* Up to date [Lilu.kext](https://github.com/vit9696/Lilu/releases) and [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
* A Plist editor: Clover configurator works fine

# BIOS Settings

So for your BIOS, make sure to have the following enabled:

* iGPU Multi-Monitor: Enabled
* Primary Display: Enabled

And make sure to have your displays connected to the motherboards display outs

# Option 1: Using WhateverGreen's Boot Flags

So this is probably the easiest of them all for users on here, all you need to do is add the following flag:

    -wegnoegpu

Now all GPUs besides the iGPU will be disabled, this isn't guaranteed to always work and has the consequence of not allowing other discrete GPUs to be used instead

# Option 2: Blocking all discrete GPUs from MacOS

This is going to be the most popular for users on here as you can disable your Nvidia card but still get to use supported AMD GPU for heavy lifting in MacOS. Main downsides of this method is that it will disable your Kepler GPU as well so you can't run an RTX 2080ti with your GT 710.

So to start, you'll need to open up your config.plist and navigate towards Devices -> Add Properties where you'll add the following:

|Devices|Key|Value|Disabled|Value Type|
|:-|:-|:-|:-|:-|
|NVidia|name|23646973706C6179||DATA|
|NVidia|IOName|\#display||STRING|
|NVidia|class-code|FFFFFFFF||DATA|

And here's the XML for those who prefer Copy-paste:

        <key>AddProperties</key>
        <array>
            <dict>
                <key>Device</key>
                <string>NVidia</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>name</string>
                <key>Value</key>
                <data>
                I2Rpc3BsYXk=
                </data>
            </dict>
            <dict>
                <key>Device</key>
                <string>NVidia</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>IOName</string>
                <key>Value</key>
                <string>#display</string>
            </dict>
            <dict>
                <key>Device</key>
                <string>NVidia</string>
                <key>Disabled</key>
                <false/>
                <key>Key</key>
                <string>class-code</string>
                <key>Value</key>
                <data>
                /////w==
                </data>
            </dict>
        </array>

And now all Nvidia GPUs will be blocked from your system

&#x200B;

And for those with Navi or other unsupported AMD GPUs, there's also some luck for you as well

&#x200B;

|Devices|Key|Value|Disabled|Value Type|
|:-|:-|:-|:-|:-|
|ATI|name|23646973706C6179||DATA|
|ATI|IOName|\#display||STRING|
|ATI|class-code|FFFFFFFF||DATA|
|ATI|vendor-id|FFFF0000||DATA|
|ATI|device-id|FFFF0000||DATA|

And that XML Goodness:

                        <key>AddProperties</key>
                        <array>
    			<dict>
    				<key>Device</key>
    				<string>ATI</string>
    				<key>Disabled</key>
    				<false/>
    				<key>Key</key>
    				<string>name</string>
    				<key>Value</key>
    				<data>
    				I2Rpc3BsYXkA
    				</data>
    			</dict>
    			<dict>
    				<key>Device</key>
    				<string>ATI</string>
    				<key>Disabled</key>
    				<false/>
    				<key>Key</key>
    				<string>IOName</string>
    				<key>Value</key>
    				<string>#display</string>
    			</dict>
    			<dict>
    				<key>Device</key>
    				<string>ATI</string>
    				<key>Disabled</key>
    				<false/>
    				<key>Key</key>
    				<string>class-code</string>
    				<key>Value</key>
    				<data>
    				/////w==
    				</data>
    			</dict>
    			<dict>
    				<key>Device</key>
    				<string>ATI</string>
    				<key>Disabled</key>
    				<false/>
    				<key>Key</key>
    				<string>vendor-id</string>
    				<key>Value</key>
    				<data>
    				//8AAA==
    				</data>
    			</dict>
    			<dict>
    				<key>Device</key>
    				<string>ATI</string>
    				<key>Disabled</key>
    				<false/>
    				<key>Key</key>
    				<string>device-id</string>
    				<key>Value</key>
    				<data>
    				//8AAA==
    				</data>
    			</dict>
                        </array>

# Option 3: Patching with an SSDT

For most this is considered the hardest as this requires the most amount of work, we'll be using [Rehabman's SSDT patching](https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/) to accomplish our Spoofing. The benefit of this method is that you can use a Kepler GPU with your system without any issues as we'll be blocking a device on the PCIe level

To start, you'll need the following:

* An SSDT/DSDT dump(done by pressing F4 at Clover boot screen)
* [AISL Compiler](https://bitbucket.org/RehabMan/acpica/downloads/)
* [MaciASL](https://sourceforge.net/projects/maciasl/)

If you open your EFI and go within EFI/CLOVER/ACPI/origin, you'll find a bunch of .aml files. These are the files we'll be playing with so grab them and put them in a folder somewhere on your hack

Now you'll want to grab an AISL Complier to analyze these files, you can grab Rehabman's Compiler [here](https://bitbucket.org/RehabMan/acpica/downloads/).

Within finder, press Command+Shift+G, enter /usr/bin and paste the IASL file here(you will need to authenticate)

Now in terminal, running the following command will disassemble our .aml files:

    cd "to directory where you placed all SSDT/DSDT"
    iasl -da -dl DSDT.aml SSDT*.aml

Now you'll find a bunch of .dsl files in that folder as well

Next lets try and find \_OFF, this is what is needed for disabling your GPU

    cd "to directory where you placed all SSDT/DSDT"
    grep -l Method.*_OFF *.dsl

Terminal should return a list of SSDT's with \_OFF within them

Example:

    SSDT-2-PegSsdt.dsl
    SSDT-3-Ther_Rvp.dsl

We can also check where the \_INI files are, these files are likely going to have some that match with \_OFF which are likely the files we want

    cd "to directory where you placed all SSDT/DSDT"
    grep -l Method.*_INI *.dsl

Terminal should return a list of SSDT's with \_INI within them

Example:

    SSDT-2-PegSsdt.dsl

You'll need to open both of these files and examine, we want the file that corresponds to your GPU. My GPU was found under SSDT-2 but this isn't the same for everyone, you'll need to check whether the \_OFF method is within a PowerShell macro or by itself(We want it by itself)

My GPU was found here:

    \_SB.PCI0.PEG0.PEGP

And for those having issues finding the device path, you can also find it in windows by following the ACPI path in device Manager.

>Properties->Details of the Nvidia device, scroll through the properties until you find "BIOS name"  
>  
>\- Rehabman

Now we can create our SSDT!

Let's open MaciASL, create a file, paste the text below and replace the device path with the one you have:

    DefinitionBlock ("", "SSDT", 2, "hack", "spoof", 0)
    {
        Method(_SB.PCI0.RP05.PEGP._DSM, 4)
        {
            If (!Arg2) { Return (Buffer() { 0x03 } ) }
            Return (Package()
            {
                "name", Buffer() { "#display" },
                "IOName", "#display",
                "class-code", Buffer() { 0xFF, 0xFF, 0xFF, 0xFF },
                "vendor-id", Buffer() { 0xFF, 0xFF, 0,  0 },
                "device-id", Buffer() { 0xFF, 0xFF, 0, 0 },
            })
        }
    }

Now save your file as a ACPI Machine Language Binary and place it in EFI/Clover/ACPI/patched/SSDT-DiscreteSpoof.aml

(Don't forget to specify it in your Config.plist)

&#x200B;

Had to have gotten my info from somewhere;)

* ["Window Server Service only ran for 0 seconds" with dual-GPU](https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/)
* [Disabling discrete graphics in dual-GPU laptops](https://www.tonymacx86.com/threads/guide-disabling-discrete-graphics-in-dual-gpu-laptops.163772/)
* [Patching LAPTOP DSDT/SSDTs](https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/)
* [Device location in Windows](https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/page-2#post-1592455)
