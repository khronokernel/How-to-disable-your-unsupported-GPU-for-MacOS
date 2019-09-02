# Introduction

## How to disable your unsupported GPU for macOS

So you got your shiny new RTX 2080ti BLOWYMATRON edition but you've probably noticed there's currently no support for your GPU in MacOS Mojave. Well for those who are running Maxwell, Pascal or Turing GPUs there is still some hope for you with options to spare!

This is an information thread, for discussion please visit the r/hackintosh thread: [How to disable your unsupported GPU for macOS](https://www.reddit.com/r/hackintosh/comments/bu1wf8/how_to_disable_your_unsupported_gpu_for_macos/)

## Prerequisite

* A Hackintosh built off the [Vanilla guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)\(This is a must as you need to have a clean system for this to work properly\)
* A GPU that is support in Mojave/Catalina\(generally this will be an iGPU for most users here, check the [GPU Buyers Guide](https://khronokernel-3.gitbook.io/catalina-gpu-buyers-guide/) to see if your GPU is supported. Keep in mind that using a Kepler GPU with Maxwell, Pascal or Turing GPUs will require more work\)
* Up to date [Lilu.kext](https://github.com/vit9696/Lilu/releases) and [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
* A Plist editor: Clover configurator works fine

## BIOS Settings

So for your BIOS, make sure to have the following enabled:

* iGPU Multi-Monitor: Enabled
* Primary Display: Enabled

And make sure to have your displays connected to the motherboards display outs

## Can I use the iGPU for rendering but the video outs of my dGPU?

Unfortunately not, and the reason being is actually quite similar to how Nvidia's Optimus technology functions. You would first need a way to grab/encode the iGPU's signal, send it towards the discrete GPU, then have said GPU decode the signal and display it. One small problem, decoding the signal would require proper GPU acceleration which your unsupported GPU doesn't have. So you will need to use your motherboard's video out ports no matter what

## Which Options should I choose?

Option 1:

* Disables all GPUs that aren't the iGPU
* Quick and easy with little work, avoid when possible as it doesn't always work

Option 2:

* Disables all GPUs from one brand\(ie: all Nvidia GPUs\)

Option 3:

* Disables a GPU on per slot basis
* Recommended for those who run Kepler GPUs with Maxwell, Pascal or Turing

Option 4:

* Disables a GPU on per slot basis
* Recommended for those who run Kepler GPUs with Maxwell, Pascal or Turing

