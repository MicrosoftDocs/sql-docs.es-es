---
title: IClientVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IClientVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: fee9c16399ba091df1878774c38751a2d92880fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347270"
---
# <a name="iclientvirtualdeviceset2close-vdi"></a>IClientVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **Close** cierra el conjunto de dispositivos virtuales creado por IClientVirtualDeviceSet2::Create. Como resultado, se liberan todos los recursos asociados al conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IClientVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | Se devuelve cuando el conjunto de dispositivos virtuales se ha cerrado correctamente. |
| VD_E_PROTOCOL | No se realizó ninguna acción porque el conjunto de dispositivos virtuales no estaba abierto. |
| VD_E_OPEN | Los dispositivos aún estaban abiertos. |

## <a name="remarks"></a>Observaciones

La invocación de Close es una declaración por parte del cliente de que se deben liberar todos los recursos usados por el conjunto de dispositivos virtuales. El cliente debe asegurarse de que toda actividad que implique búferes de datos y dispositivos virtuales haya finalizado antes de invocar Close. Todas las interfaces de dispositivo virtual devueltas por OpenDevice se invalidan con Close.

El cliente puede emitir una llamada a Create en la interfaz del conjunto de dispositivos virtuales después de que se devuelva la llamada a Close. Esta llamada crearía un nuevo conjunto de dispositivos virtuales para una operación BACKUP o RESTORE posterior.

Si se llama a Close cuando uno o más dispositivos virtuales están todavía abiertos, se devuelve VD_E_OPEN. En este caso, SignalAbort se desencadena internamente para garantizar un apagado adecuado si es posible. Se publican los recursos de VDI. El cliente debe esperar una indicación VD_E_CLOSE en cada dispositivo antes de invocar IClientVirtualDeviceSet2::Close. Si el cliente sabe que el conjunto de dispositivos virtuales ya está en un estado de finalización anómala, no debería esperar una indicación VD_E_CLOSE por parte de GetCommand y puede invocar IClientVirtualDeviceSet2::Close tan pronto como finalice la actividad en los búferes compartidos.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).