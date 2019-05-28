# How-to-disable-your-unsupported-GPU-for-MacOS
So you got your shiny new RTX  2080ti BLOWYMATRON edition but you've probably noticed there's currently no support for your GPU in MacOS Mojave. Well for those who are running Maxwell, Pascal or Truing GPUs there is still some hope for you with options to spare!

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

# Option 2: Blocking All Nvidia GPUs from MacOS

This is going to be the most popular for users on here as you can disable your Nvidia card but still get to use supported AMD GPU for heavy lifting in MacOS. Main downsides of this method is that it will disable your Kepler GPU as well so you can't run an RTX 2080ti with your GT 710.

So to start, you'll need to open up your config.plist and navigate towards Devices -> Add Properties


|Devices|Key|Value|Disabled|Value Type |
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

# Option 3: Patching with an SSDT

For most this is considered the hardest as this requires the most amount of work, we'll be using [Rehabman's SSDT patching](https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/) to accomplish our Spoofing. The benefit of this method is that you can use a Kepler GPU with your system without any issues as we'll be blocking a device on the PCIe level

To start, we'll need to get a DSDT/SSDT dump with Clover. To do this, press F4 at the Clover boot screen(unfortunately there's no feedback if this was successful). 

Next, open your EFI and go within EFI/CLOVER/ACPI/origin where you'll find a bunch of .aml files. 
