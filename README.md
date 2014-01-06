# Syphon Virtual Screen

Based on bangnoise/vade's [Syphon](http://syphon.v002.info) framework and Enno Welber's [EWProxyFrameBuffer](https://github.com/mkernel/EWProxyFramebuffer).

Syphon Virtual screen is a tool to share video output in applications that can go full-screen but are not yet Syphon-enabled (I use it with [QLab](http://figure53.com/qlab/), waiting for Qlab 3!)

The framebuffer it draws on is developed by Enno Welbers (thanks Enno!). It resides in CPU memory, so maybe it could not be the best solution for heavy video processing: every frame has to be uploaded to the GPU RAM to be published on a syphon server, and this is a (relatively) slow operation. The best would be having native Syphon support in the application you target of course, but if you're here it means you can't go that way.. But still it can be helpful to hijack an application video output that is not Syphon-enabled. As for now, this is the only solution with QLab, while we wait for a real support in the application. SVS has been reported to work flawlessly on a 2048x768 texture.


## Installing

### From source

Download the master branch, then, update the submodule:

      git submodule update --init

The project will compile in two products: 

- EWProxyFramebuffer.kext - this is the virtual frame buffer driver, and must be correctly installed to work. To install it: copy to /System/Library/Extensions, fix permissions (`sudo chmod -R 755 /System/Library/Extensions/EWProxyFramebuffer.kext`) and ownership (`sudo chown -R root:wheel /System/Library/Extensions/EWProxyFramebuffer.kext`), then clear cache (in Lion, `rm -R /System/Library/Caches/com.apple.kext.caches/Startup`). Now reboot!

- Syphon Virtual Screen.app - this is the client application. Just copy it in /Applications and launch (after installing the kext and reboot)


### Binary

As an alternative to compiling from source, you can just download the latest version from [this link](https://dl.dropboxusercontent.com/u/2764054/SVS.zip).

This includes an installer and an alternate version of the client application.
Pleas install the package first, then replace the "Syphon Virtual Screen" application in /Applications folder with the one provided in the .zip file.

## Using custom resolutions

To use different resolutions you can just modify the EWSyphonProxyFramebuffer/info.plist. 
You can do it:
- before compiling the kext
- downloading the binary and editing the kext configuration. Here is how:


- open the Terminal
- type `sudo su`, enter, type your password
- type `cd /System/Library/Extensions/EWProxyFramebuffer.kext/Contents/`
- type `nano Info.plist`
- search this section:

    MaxResolution

    height	1024
    width	1280


and replace max height and width as you need to

- a little above this section you’ll find a list of available resolutions. Add yours:

    height	2048
    name	2048×768
    width	768


- type “Ctrl+X” to save your editing

- now, back in terminal, you have to repair permissions for the driver you’ve just modified. Type:
`sudo chmod -R 755 /System/Library/Extensions/EWProxyFramebuffer.kext`
`sudo chown -R root:wheel /System/Library/Extensions/EWProxyFramebuffer.kext`

- delete the kext cache (“rm -R /System/Library/Caches/com.apple.kext.caches/Startup”);

- reboot