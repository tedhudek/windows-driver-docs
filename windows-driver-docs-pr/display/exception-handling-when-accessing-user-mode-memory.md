---
title: Exception Handling When Accessing User-Mode Memory
description: Exception Handling When Accessing User-Mode Memory
ms.assetid: 44ed69a0-da0e-4335-9128-a78a83ea80dd
keywords:
- user-mode memory WDK Windows 2000 display
- user-mode memory WDK Windows 2000 display , exception handling
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Exception Handling When Accessing User-Mode Memory


## <span id="ddk_exception_handling_when_accessing_user_mode_memory_gg"></span><span id="DDK_EXCEPTION_HANDLING_WHEN_ACCESSING_USER_MODE_MEMORY_GG"></span>


A display or video miniport driver must use exception handling around code that accesses data structures allocated in user mode. The Microsoft Direct3D runtime secures ownership of such data structures before passing them to the driver. To secure ownership of user-mode memory, the runtime calls the [**MmSecureVirtualMemory**](https://msdn.microsoft.com/library/windows/hardware/ff556374) function. When the runtime secures ownership of user-mode memory, it prevents any other thread from modifying the type of access to the memory. For example, if the runtime secures ownership of a data structure that a user-mode thread has allocated with read and write access, other threads cannot restrict the data structure's access type to read-only. Also, securing ownership of user-mode memory does not guarantee that the memory remains valid.

Therefore, unless exception handling is implemented around code that accesses such memory, the operating system crashes if the driver attempts to access invalid user-mode memory. For invalid kernel-memory accesses, the only available option for the operating system is to crash. However, for invalid user-memory accesses, the driver can terminate the application that invalidated the memory and leave the operating system and the driver's device in a stable state.

The driver must implement exception handling rather than rely on the runtime to handle exceptions. If the runtime handled exceptions and the driver accessed invalid user-mode memory, the stack would return to the exception-handling code in the runtime, leaving the driver or the device in an unknown state. The driver must implement exception handling so that it can perform the following operations if an exception occurs:

-   Restore its state and the state of its device.

-   Release any spin locks that it acquired.

In the following scenarios, the runtime secures ownership of memory allocated in user mode before passing the memory to the driver.

-   The driver processes vertex data that is specified by a pointer to user-mode memory. The driver receives this memory pointer in a call to its [**D3dDrawPrimitives2**](https://msdn.microsoft.com/library/windows/hardware/ff544704) function. In this *D3dDrawPrimitives2* call, the D3DHALDP2\_USERMEMVERTICES flag of the **dwFlags** member of the [**D3DHAL\_DRAWPRIMITIVES2DATA**](https://msdn.microsoft.com/library/windows/hardware/ff545957) structure is set.

-   The driver updates the render state array to which the **lpdwRStates** member of D3DHAL\_DRAWPRIMITIVES2DATA points. The driver updates this array during a call to its *D3dDrawPrimitives2* function.

-   The driver updates its state at the **lpdwStates** member of the [**DD\_GETDRIVERSTATEDATA**](https://msdn.microsoft.com/library/windows/hardware/ff551551) structure during a call to its [**D3dGetDriverState**](https://msdn.microsoft.com/library/windows/hardware/ff544708) function.

-   The driver bit-block transfers or accesses a system texture that was allocated in user memory.

A display driver can use the **try/except** mechanism to implement exception handling. For more information about **try/except**, see the Microsoft Visual C++ documentation.

The following code example shows how the driver can use the **try/except** mechanism to throw an exception if an error occurs due to accessing invalid memory.

```
__try
{
    // Access user-mode memory.
}
__except(EXCEPTION_EXECUTE_HANDLER)
{
    // Recover and leave driver and hardware in a stable state.
}
```

**Note**   Aside from accessing and copying the user-mode value into a local variable, the driver should not perform any other operations inside the **\_\_try** block. Other operations can cause their own exceptions to occur. The operating system handles these exceptions differently.

 

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Exception%20Handling%20When%20Accessing%20User-Mode%20Memory%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




