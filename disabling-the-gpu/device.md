# Option 4: DeviceProperties

Options 3 and 4 are quite similar, they both disable a very specific GPU allowing you to use other GPUs of the same brand together like a GT 710 with a GTX 2080Ti. How the DevicePropertes patch differs from an SSDT is that it's easier to find the GPU's device path for a GPU from macOS.

To start, you'll need the following:

* [gfxutil](https://github.com/acidanthera/gfxutil/releases)
* [Lilu.kext](device.md)
* [WhateverGreen.kext](device.md)

Now you'll want to open up terminal, drag the gfxutil and add -f and GFX0\(or whatever your GPU is called, you can check with IORegistryExplorer\):

```text
/Users/(YourUsername)/Downloads/(gfxdownload folder)/gfxutil -f GFX0
```

And the output will result in something similar:

```text
DevicePath = PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)
```

With this, we can start the real work.

Under `Devices -> Properties -> Devices`, we can add our PCI route with the following properties:

Nvidia users:

| Properties Key\* | Properties Value | Value Type |
| :--- | :--- | :--- |
| name | 23646973706C6179 | data |
| IOName | \#display | string |
| class-code | FFFFFFFF | data |

AMD users:

| Properties Key\* | Properties Value | Value Type |
| :--- | :--- | :--- |
| name | 23646973706C6179 | data |
| IOName | \#display | string |
| class-code | FFFFFFFF | data |
| vendor-id | FFFF0000 | data |
| device-id | FFFF0000 | data |

and that XML goodness:

```text
<key>Properties</key>
        <dict>
            <key>PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)</key>
            <dict>
                <key>IOName</key>
                <string>#display</string>
                <key>class-code</key>
                <data>
                /////w==
                </data>
                <key>name</key>
                <data>
                I2Rpc3BsYXk=
                </data>
            </dict>
        </dict>
```

![](https://i.imgur.com/kErJi0g.png)

And voila! Your unsupported GPU is now hidden, do keep in mind other devices that go into that PCIe slot will also gain these properties disabling them\(_I may or may not have disabled my PCIe drives this way_\)

