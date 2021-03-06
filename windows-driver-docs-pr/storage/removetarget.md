---
title: RemoveTarget function
description: The RemoveTarget WMI method configures the WMI provider so that it stops passing events associated with the indicated target to the WMI client.
ms.assetid: 413cee3c-5e3a-4012-925b-b4699fbd2e1b
keywords: ["RemoveTarget function Storage Devices"]
topic_type:
- apiref
api_name:
- RemoveTarget
api_location:
- Hbapiwmi.h
api_type:
- HeaderDef
---

# RemoveTarget function


The **RemoveTarget** WMI method configures the WMI provider so that it stops passing events associated with the indicated target to the WMI client.

Syntax
------

```ManagedCPlusPlus
void RemoveTarget(
   [in, HBAType("HBA_WWN")] uint8          HbaPortWWN[8],
   [in, HBAType("HBA_WWN")] uint8          DiscoveredPortWWN[8],
   [in] uint32                             AllTargets,
   [out, HBA_STATUS_QUALIFIERS] HBA_STATUS HBAStatus
);
```

Parameters
----------

*HbaPortWWN*   
A 64 bit worldwide name (WWN) that uniquely identifies the local port that should be removed from the list of ports whose events are reported to the WMI client. For a discussion of worldwide names, see the T11 committee's *Fibre Channel HBA API* specification.

*DiscoveredPortWWN*   
A WWN that indicates the remote discovered port that should be removed from the list of ports whose events are reported to the WMI client.

*AllTargets*   
The events to stop reporting. If this member is zero, the WMI provider client will stop reporting events associated with the port that is indicated by *DiscoveredPortWWN*. If this member is nonzero, the WMI provider will cease reporting all events associated any target.

*HBAStatus*   
On return, contains the status of the operation. For a list of allowed values and their descriptions, see [HBA\_STATUS](hba-status.md). The miniport driver returns this information in the **HBAStatus** member of a [**RemoveTarget\_OUT**](https://msdn.microsoft.com/library/windows/hardware/ff564039) structure.

Return value
------------

Not applicable to WMI methods.

Remarks
-------

This WMI method belongs to the [MSFC\_EventControl WMI Class](msfc-eventcontrol-wmi-class.md).

Requirements
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Target platform</p></td>
<td align="left">Desktop</td>
</tr>
<tr class="even">
<td align="left"><p>Header</p></td>
<td align="left">Hbapiwmi.h (include Hbapiwmi.h, Hbaapi.h, or Hbaapi.h)</td>
</tr>
</tbody>
</table>

## <span id="see_also"></span>See also


[**RemoveTarget\_IN**](https://msdn.microsoft.com/library/windows/hardware/ff564033)

[**RemoveTarget\_OUT**](https://msdn.microsoft.com/library/windows/hardware/ff564039)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[storage\storage]:%20RemoveTarget%20function%20%20RELEASE:%20%281/11/2018%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")





