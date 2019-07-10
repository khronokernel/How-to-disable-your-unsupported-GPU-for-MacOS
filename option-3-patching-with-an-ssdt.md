# Option 3: SSDT

For most this is considered the hardest as this requires the most amount of work, we'll be using [Rehabman's SSDT patching](https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/) to accomplish our Spoofing. The benefit of this method is that you can use a Kepler GPU with your system without any issues as we'll be blocking a device on the PCIe level

To start, you'll need the following:

* An SSDT/DSDT dump\(done by pressing F4 at Clover boot screen\)
* [AISL Compiler](https://bitbucket.org/RehabMan/acpica/downloads/)
* [MaciASL](https://sourceforge.net/projects/maciasl/)

If you open your EFI and go within EFI/CLOVER/ACPI/origin, you'll find a bunch of .aml files. These are the files we'll be playing with so grab them and put them in a folder somewhere on your hack

Now you'll want to grab an AISL Complier to analyze these files, you can grab Rehabman's Compiler [here](https://bitbucket.org/RehabMan/acpica/downloads/).

Within finder, press Command+Shift+G, enter /usr/bin and paste the IASL file here\(you will need to authenticate\)

Now in terminal, running the following command will disassemble our .aml files:

```text
cd "to directory where you placed all SSDT/DSDT"
iasl -da -dl DSDT.aml SSDT*.aml
```

Now you'll find a bunch of .dsl files in that folder as well

Next lets try and find \_OFF, this is what is needed for disabling your GPU

```text
cd "to directory where you placed all SSDT/DSDT"
grep -l Method.*_OFF *.dsl
```

Terminal should return a list of SSDT's with \_OFF within them

Example:

```text
SSDT-2-PegSsdt.dsl
SSDT-3-Ther_Rvp.dsl
```

We can also check where the \_INI files are, these files are likely going to have some that match with \_OFF which are likely the files we want

```text
cd "to directory where you placed all SSDT/DSDT"
grep -l Method.*_INI *.dsl
```

Terminal should return a list of SSDT's with \_INI within them

Example:

```text
SSDT-2-PegSsdt.dsl
```

You'll need to open both of these files and examine, we want the file that corresponds to your GPU. My GPU was found under SSDT-2 but this isn't the same for everyone, you'll need to check whether the \_OFF method is within a PowerShell macro or by itself\(We want it by itself\)

My GPU was found here:

```text
\_SB.PCI0.PEG0.PEGP
```

And for those having issues finding the device path, you can also find it in windows by following the ACPI path in device Manager.

> Properties-&gt;Details of the Nvidia device, scroll through the properties until you find "BIOS name"  
> - Rehabman

Now we can create our SSDT!

Let's open MaciASL, create a file, paste the text below and replace the device path with the one you have:

```text
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
```

Now save your file as a ACPI Machine Language Binary and place it in EFI/Clover/ACPI/patched/SSDT-DiscreteSpoof.aml

\(Don't forget to specify it in your Config.plist\)

