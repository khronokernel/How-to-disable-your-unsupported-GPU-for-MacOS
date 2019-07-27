Options 3 and 4 are quite similar, they both disable a very specifc GPU allowing you to use other GPUs togehter with is like a GT 710 with a GTX 2080Ti. How the DevicePropertes patch differ from an SSDT is that it's easier to find the GPU's device path for a GPU from macOS.

To start, you'll need the following:

* [gfxutil](https://github.com/acidanthera/gfxutil/releases)
* [Lilu.kext]()
* [WhateverGreen.kext]()


Now you'll want to open up terminal, drag the gfxutil and add -f and GFX0(or whatever your GPU is called, you can check with IORegistryExplorer):

```
/Users/(YourUsername)/Downloads/(gfxdownload folder)/gfxutil -f GFX0
```
And the output will result in something similar:

```
DevicePath = PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)
```

With this, we can start the real work.

Under `Devices -> Properties -> Devices`, we can add our PCI route with the following properties:

|Properties Key\*|Properties Value|Value Type|
|:-|:-|:-|
|name|23646973706C6179|data|
|IONAME|\#display|string|
|class-code|FFFFFFFF|data|


and that XML goodness:
```
<key>Properties</key>
		<dict>
			<key>PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)</key>
			<dict>
				<key>IONAME</key>
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
