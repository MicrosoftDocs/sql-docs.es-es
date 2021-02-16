---
title: IServerVirtualDeviceSet2::SignalAbort
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDeviceSet2::SignalAbort.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7dd21dfb56d093a6d35d977577971545c3fb1263
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347050"
---
# <a name="iservervirtualdeviceset2signalabort-vdi"></a>IServerVirtualDeviceSet2::SignalAbort (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **SignalAbort** indica que se debe producir una finalización anómala.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDeviceSet2::SignalAbort ();
```

## <a name="return-value"></a>Valor devuelto

Devuelve *HRESULT* que indica si la llamada al método se realizó correctamente o no. Un valor de NOERROR indica que la llamada al método se ha realizado correctamente. Un valor distinto de cero indica que se ha producido un error.

## <a name="remarks"></a>Observaciones

En cualquier momento, el servidor puede optar por anular la operación BACKUP o RESTORE.

Esta rutina indica que todas las operaciones deben cesar. La interfaz general entra en un estado de anulación. No se aceptan más comandos en ningún dispositivo. El agente de finalización devuelve ERROR_OPERATION_ABORTED para cada solicitud pendiente y vuelve al autor de la llamada. Se omiten todas las finalizaciones registradas en el cliente.

El servidor se asegura de que no haya ningún uso más necesario de los búferes o dispositivos devueltos por la interfaz del dispositivo virtual. Después, el servidor realiza una limpieza de finalización anómala, que debe incluir la llamada a la función IServerVirtualDeviceSet2::Close.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).