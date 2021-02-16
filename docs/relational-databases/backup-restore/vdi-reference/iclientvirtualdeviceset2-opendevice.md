---
title: IClientVirtualDeviceSet2::OpenDevice
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IClientVirtualDeviceSet2::OpenDevice.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6abf203001af56c11bb8d1cee3f233877fb8d51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347266"
---
# <a name="iclientvirtualdeviceset2opendevice-vdi"></a>IClientVirtualDeviceSet2::OpenDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **OpenDevice** abre uno de los dispositivos del conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IClientVirtualDeviceSet2::OpenDevice (
   LPCWSTR                  lpName,
   IClientVirtualDevice**   ppVirtualDevice

);
```

## <a name="parameters"></a>Parámetros

*lpName* Identifica el dispositivo virtual.

*ppVirtualDevice* Cuando la función se ejecuta correctamente, se devuelve un puntero de interfaz al dispositivo virtual. Esta interfaz se usa con GetCommand y CompleteCommand.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | La función se ha realizado correctamente. |
| VD_E_ABORT | Se solicitó Abort. |
| VD_E_OPEN |Todos los dispositivos están abiertos. |
| VD_E_PROTOCOL | El conjunto no está en el estado de inicialización o este dispositivo en concreto ya está abierto. |
| VD_E_INVALID | El nombre del dispositivo no es válido. No es uno de los nombres que se sabe que forman el conjunto. |

## <a name="remarks"></a>Observaciones

Es posible que VD_E_OPEN se devuelva sin problemas. El cliente puede llamar a OpenDevice por medio de un bucle hasta que se devuelva este código.
Si se configura más de un dispositivo (por ejemplo n dispositivos) el conjunto de dispositivos virtuales devolverá n interfaces de dispositivo únicas. El primer dispositivo tiene el mismo nombre que el conjunto de dispositivos virtuales. Los nombres de los demás dispositivos se especifican con las cláusulas VIRTUAL_DEVICE de la instrucción BACKUP/RESTORE.

Se puede usar la función GetConfiguration para esperar hasta que se puedan abrir los dispositivos.

Si esta función no se realiza correctamente, se devuelve un valor NULL a través de ppVirtualDevice.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).