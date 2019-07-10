# Introduction

## How to disable your unsupported GPU for macOS

So you got your shiny new RTX 2080ti BLOWYMATRON edition but you've probably noticed there's currently no support for your GPU in MacOS Mojave. Well for those who are running Maxwell, Pascal or Turing GPUs there is still some hope for you with options to spare!

This is an information thread, for discussion please visit the r/hackintosh thread: [How to disable your unsupported GPU for macOS](https://www.reddit.com/r/hackintosh/comments/bu1wf8/how_to_disable_your_unsupported_gpu_for_macos/)

## Prerequisite

* A Hackintosh built off the [Vanilla guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)\(This is a must as you need to have a clean system for this to work properly\)
* A GPU that is support in Mojave/Catalina\(generally this will be an iGPU for most users here, check the [Catalina GPU buyers Guide](https://khronokernel-3.gitbook.io/catalina-gpu-buyers-guide/) to see if your GPU is supported. Keep in mind that using a Kepler GPU with Maxwell, Pascal or Turing GPUs can cause issues\)
* Up to date [Lilu.kext](https://github.com/vit9696/Lilu/releases) and [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
* A Plist editor: Clover configurator works fine

## BIOS Settings

So for your BIOS, make sure to have the following enabled:

* iGPU Multi-Monitor: Enabled
* Primary Display: Enabled

And make sure to have your displays connected to the motherboards display outs

