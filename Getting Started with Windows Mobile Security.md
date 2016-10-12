# Getting Started with Windows Mobile Security

## Universal Windows Platform and Universal Apps
<https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide>

Windows 10 introduces the Universal Windows Platform (UWP), which further evolves the Windows Runtime model and brings it into the Windows 10 unified core. As part of the core, the UWP now provides a common app platform available on every device that runs Windows 10. With this evolution, apps that target the UWP can call not only the WinRT APIs that are common to all devices, but also APIs (including Win32 and .NET APIs) that are specific to the device family the app is running on. The UWP provides a guaranteed core API layer across devices. This means you can create a single app package that can be installed onto a wide range of devices. And, with that single app package, the Windows Store provides a unified distribution channel to reach all the device types your app can run on.

![alt text](https://i-msdn.sec.s-msft.com/en-us/windows/uwp/get-started/images/device-family-tree.png "Device families")

A device family is a set of APIs collected together and given a name and a version number. A device family is the foundation of an OS. PCs run the desktop OS, which is based on the desktop device family. Phones and tablets, etc., run the mobile OS, which is based on the mobile device family. And so on.

The universal device family is special. It is not, directly, the foundation of any OS. Instead, the set of APIs in the universal device family is inherited by child device families. The universal device family APIs are thus guaranteed to be present in every OS and on every device.

Each child device family adds its own APIs to the ones it inherits. The resulting union of APIs in a child device family is guaranteed to be present in the OS based on that device family, and on every device running that OS.

## Interception using Fiddler or Charles
<https://blogs.msdn.microsoft.com/fiddler/2011/12/10/revisiting-fiddler-and-win8-immersive-applications/>

UWP applications run inside isolated processes known as “AppContainers.” By default, AppContainers are forbidden from sending network traffic to the local computer (loopback).

**Use the EnableLoopback utility**

![alt text](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/37/50/metablogapi/2425.EnableLoopbackUtility_41276413.png "EnableLoopback utility")

## Download an app package from Store
<http://woshub.com/how-to-download-appx-installation-file-for-any-windows-store-app/>

- Use Fiddler or Charles
- In the EnableLoopback utility, exempt the "Store" package and save changes
- Open the Store app and install the application you want
- In Fiddler, search for the string "appx" in the captured URLs
- Copy the URL and open in IE or Chrome
- Note: This URL is a temporary, short-lived link

## Local Storage and App Sandbox

Installation Directory:
>C:\Program Files\WindowsApps\(package_name)

Local Storage:
>C:\Users\(username)\AppData\Local\Packages\(package_name)

## Setting up the UWP test environment
<http://www.c-sharpcorner.com/article/how-to-install-windows-10-sdk-and-windows-10-emulators/>

### Install VS2015 and Emulators
- Download Visual Studio 2015 from here:
<https://www.visualstudio.com/downloads/>
- Ensure you check the "Universal Windows App Development Tools" to install the Win10Mobile emulators  and the required libraries

![alt text](http://csharpcorner.mindcrackerinc.netdna-cdn.com/article/how-to-install-windows-10-sdk-and-windows-10-emulators/Images/image003.png "Universal Windows App Development Tools")

### Firing up the  Emulators
<http://stackoverflow.com/questions/31493408/how-to-run-the-windows-phone-10-emulator>

The emulators are installed here:
>C:\Program Files (x86)\Microsoft XDE\(build_version)

Run the following command to get info:
`xde /?`

Run the following command to fire up an emulator:
`"C:\Program Files (x86)\Microsoft XDE\10.0.1.0\XDE.exe" /name "Mobile Emulator 10.0.14393.0 WVGA 4 inch 1GB.kaushalb" /displayName "Mobile Emulator 10.0.14393.0 WVGA 4 inch 1GB" /vhd "C:\Program Files (x86)\Windows Kits\10\Emulation\Mobile\10.0.14393.0\flash.vhd" /video "480x854" /memsize 1024 /diagonalSize 4 /language 409 /creatediffdisk "C:\Users\kaushalb\AppData\Local\Microsoft\XDE\10.0.14393.0\dd.480x854.1024.vhd" /snapshot /fastShutdown`

Note: If  the following folder does not exist, create it.
>C:\Users\(username)\AppData\Local\Microsoft\XDE\10.0.14393.0\

Emulators can also be launched from VS2015 by running some test code on an emulator

### Developer Mode and Device Portal
<https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development>
<https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-mobile>

- Enable Developer Mode on your computer

![alt text](https://i-msdn.sec.s-msft.com/en-us/windows/uwp/get-started/images/devmode-pc-options.png "Developer Mode on your computer")

- Enable Developer Mode on your mobile/emulator

![alt text](https://i-msdn.sec.s-msft.com/en-us/windows/uwp/get-started/images/devmode-mob.png "Developer Mode on your mobile/emulator")

- Enable Device Discovery and Device Portal

![alt text](https://i-msdn.sec.s-msft.com/en-us/windows/uwp/get-started/images/devmode-mob-options.png "Device Discovery and Device Portal")

## Installing applications and Accessing File-system

### Via WinAppDeployCmd.exe
<https://blogs.windows.com/buildingapps/2015/07/09/just-released-windows-10-application-deployment-tool/>
<https://code.msdn.microsoft.com/windowsapps/UWP-Apps-How-to-instal-d20eb116>

WinAppDeployCmd.exe is available here:
>C:\Program Files (x86)\Windows Kits\10\bin\x86\

Install application using the following command:
`WinAppDeployCmd install -file <Appx file path> -ip <address> -pin <p>`

### Via Device Portal
<https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal>
1. Click browse and find your app package (.appx).
2. Click browse and find the certificate file (.cer). (Not required on all devices.)
3. Add dependencies. If you have more than one, add each one individually.
Under Deploy, click Go.
4. To install another app, click the Reset button to clear the fields.
