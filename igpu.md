So this little section is for those who are wanting to run their iGPU as their primary/sole display output. 


## Setting up your config.plist

So to setup your iGPU for dsplay tasks, we need to set a framebuffer appropriate for this task. To setup a framebuffer, we'll need specify the PCIRoot path. Luckily for us, intel has stayed consistent on this path for all of their iGPUs:

```
PciRoot(0x0)/Pci(0x2,0x0)
```


Now we need to to find a framebuffer approrpiate for our iGPU, you can find a list of these values in the [GPU Buyers Guide](https://khronokernel-3.gitbook.io/catalina-gpu-buyers-guide/) or in the [Intel Framebuffer Patching Guide](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/?tab=comments#comment-2626271). For the majority you can find your iGPU here:

**Ivy Bridge**:

* HD 4000
   * `0A006601`
  
**Haswell**:

* HD 4600 
* HD 4400
   * `0300220D`
   
* **NOTE**: HD 4400 isn't natievly supported, you will need to spoof it into an HD 4600 which is supported:

|Properties Key\*|Properties Value|Value Type|
|:-|:-|:-|
|device-id|12040000|Data|

   
**Skylake**:

* HD 530
   * `00001219`
   
**Kabylake**:

* HD 630
   * `00001659`
   
**Coffeelake**:

* UHD 630
   * `07009B3E`
   
Now with all the nessary values, we can put this in our config.plist:

Under `Devices` -> `Properties` you'll want to add your PCIRoot on the left and our framebuffer properties on the right:

|Properties Key\*|Properties Value|Value Type|
|:-|:-|:-|
|AAPL,ig-platform-id|00001219|Data|
* NOTE: Swap `00001219` for the framebuffer appropriate for your GPU

And for those who cannot set their iGPU's memory to 64MB in the BIOS will also need to add this patch to decrease the requirement to 19MB:

|Properties Key\*|Properties Value|Value Type|
|:-|:-|:-|
|framebuffer-patch-enable|01000000|Data|
|framebuffer-stolenmem|00003001|Data|

![Clover Configurator](https://i.imgur.com/zoa9YR6.png)

## Having either color or display out issues?

Sometimes macOS won't be able to correctly tell which display out is which so it may apply displayPort properties onto an HDMI port or vise versa. To fix this, you can refer to the [Pink/ Purple Tint section in the r/Hackintosh Vanilla Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/coffee-lake#pink-purple-tint)

   
