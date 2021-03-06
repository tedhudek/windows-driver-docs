---
title: Monitor Profiles
description: Monitor Profiles
ms.assetid: aede7ada-3826-40a4-8b37-18ae242eea77
keywords:
- display drivers WDK Windows 2000 , color management
- color management WDK Windows 2000 display
- display drivers WDK Windows 2000 , monitor profiles
- monitor profiles WDK Windows 2000 display
- device profiles WDK Windows 2000 display
- color space WDK Windows 2000 display
- color gamut WDK Windows 2000 display
- device-independent color space WDK Windows 2000 display
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Monitor Profiles


## <span id="ddk_monitor_profiles_gg"></span><span id="DDK_MONITOR_PROFILES_GG"></span>


A [*monitor profile*](https://msdn.microsoft.com/library/windows/hardware/ff556308#wdkgloss-monitor-profile) is a type of device profile used for color management. This profile contains information about how to convert colors in a monitor's *color space* and *color gamut* into colors in a device-independent color space. Any user-mode application, such as a setup program or a word processor with graphics capabilities, can use a monitor profile, provided that [*ICM*](https://msdn.microsoft.com/library/windows/hardware/ff556290#wdkgloss-icm) has been enabled, and that the application has knowledge of the profile's format.

Although you can create custom monitor profiles using third-party tools, you may be able to use one of the monitor profiles shipped with Windows 2000 and later operating system versions. These profiles are described in the following table.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Profile</th>
<th align="left">Monitor Characteristics</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>mnB22G15.icm</p></td>
<td align="left"><p>B22 phosphor, gamma 1.5</p></td>
</tr>
<tr class="even">
<td align="left"><p>mnB22G18.icm</p></td>
<td align="left"><p>B22 phosphor, gamma 1.8</p></td>
</tr>
<tr class="odd">
<td align="left"><p>mnB22G21.icm</p></td>
<td align="left"><p>B22 phosphor, gamma 2.1.</p></td>
</tr>
<tr class="even">
<td align="left"><p>mnEBUG15.icm</p></td>
<td align="left"><p>EBU phosphor, gamma 1.5</p></td>
</tr>
<tr class="odd">
<td align="left"><p>mnEBUG18.icm</p></td>
<td align="left"><p>EBU phosphor, gamma 1.8</p></td>
</tr>
<tr class="even">
<td align="left"><p>mnEBUB21.icm</p></td>
<td align="left"><p>EBU phosphor, gamma 2.1</p></td>
</tr>
<tr class="odd">
<td align="left"><p>mnP22G15.icm</p></td>
<td align="left"><p>P22 phosphor, gamma 1.5</p></td>
</tr>
<tr class="even">
<td align="left"><p>mnP22G18.icm</p></td>
<td align="left"><p>P22 phosphor, gamma 1.8</p></td>
</tr>
<tr class="odd">
<td align="left"><p>mnP22G21.icm</p></td>
<td align="left"><p>P22 phosphor, gamma 2.1</p></td>
</tr>
<tr class="even">
<td align="left"><p>Diamond Compatible 9300K G2.2.icm</p></td>
<td align="left"><p>9300Â° Kelvin white point, gamma 2.2</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Hitachi Compatible 9300K G2.2.icm</p></td>
<td align="left"><p>9300Â° Kelvin white point, gamma 2.2</p></td>
</tr>
<tr class="even">
<td align="left"><p>NEC Compatible 9300K G2.2.icm</p></td>
<td align="left"><p>9300Â° Kelvin white point, gamma 2.2</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Trinitron Compatible 9300K G2.2.icm</p></td>
<td align="left"><p>9300Â° Kelvin white point, gamma 2.2</p></td>
</tr>
</tbody>
</table>

 

### <span id="Installing_a_Monitor_Profile"></span><span id="installing_a_monitor_profile"></span><span id="INSTALLING_A_MONITOR_PROFILE"></span>Installing a Monitor Profile

A user can install a monitor profile in three different ways:

1.  In the Windows Explorer, select the profile, right-click the name, and then click **Install Profile**.

2.  Refer to the profile in a [monitor INF file](monitor-inf-file-sections.md).

3.  Hard-code the profile's path and file name in an application.

Because the default directory for monitor profiles is subject to change, hard-coding the profile's path and file name is not recommended.

### <span id="Using_a_Monitor_Profile"></span><span id="using_a_monitor_profile"></span><span id="USING_A_MONITOR_PROFILE"></span>Using a Monitor Profile

A monitor profile, unlike a printer profile, supports very little communication between the output device and an application. For example, if a user changes the gamma ramp in the video buffer, the monitor profile is not notified that such a change has occurred. In this case, with ICM enabled, two color corrections are applied to the image before it is displayed, as shown in the following sequence of steps.

1.  The application opens and then manipulates the image.

2.  The application enables ICM by a call to a Win32 GDI ICM function, such as **SetICMMode**. (See the Microsoft Windows SDK for more information.)

3.  The application sends the image to Win32 GDI.

4.  If ICM is enabled, Win32 GDI uses the monitor profile to translate the colors in the image.

5.  Win32 GDI sends the image to kernel-mode GDI.

6.  Kernel-mode GDI formats the image for the display driver, based on such device characteristics of the device context (DC) as bit depth, resolution, and halftoning.

7.  The display driver (or video hardware) performs gamma correction to the image.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Monitor%20Profiles%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




