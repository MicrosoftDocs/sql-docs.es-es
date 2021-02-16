---
title: IServerVirtualDevice::CloseDevice
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDevice::CloseDevice.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7034c517483a114d9b1a0be4fbf15f80fd8801dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347172"
---
# <a name="iservervirtualdeviceclosedevice-vdi"></a>IServerVirtualDevice::CloseDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **CloseDevice** cierra un dispositivo virtual que se ha abierto con IServerVirtualDeviceSet2::OpenDevice

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDevice::CloseDevice ();
```

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| VD_E_CLOSE | El dispositivo ya está cerrado. |
| VD_E_ABORT | La interfaz está en el estado Anular. |

## <a name="remarks"></a>Observaciones

CloseDevice no es necesario después de usar SignalAbort para forzar una terminación anómala. Si se invoca CloseDevice después de usar SignalAbort, no se realiza ninguna acción.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).