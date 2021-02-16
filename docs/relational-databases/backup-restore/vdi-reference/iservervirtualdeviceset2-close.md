---
title: IServerVirtualDeviceSet2::Close
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDeviceSet2::Close.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bc520d4cb4ded130c6d44a2fef70ddd4b5ff5d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347146"
---
# <a name="iservervirtualdeviceset2close-vdi"></a>IServerVirtualDeviceSet2::Close (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **Close** cierra un conjunto de dispositivos virtuales abierto por IServerVirtualDeviceSet2::Open. Como resultado, se liberan todos los recursos asociados al dispositivo virtual. El identificador IServerVirtualDeviceSet2 no es útil después de que esta función devuelva un resultado y se debe devolver a COM.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDeviceSet2::Close ();
```

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| VD_E_PROTOCOL | Los dispositivos todavía estaban abiertos. |

## <a name="remarks"></a>Observaciones

No se debe realizar el cierre del conjunto de dispositivos virtuales antes de cerrar los dispositivos. Si se produce esta situación, se devuelve VD_E_PROTOCOL. Esta acción hace que Close libere inmediatamente su asignación de memoria compartida. El servidor está sujeto a infracciones de acceso si sigue esperando la propiedad de los recursos devueltos por la interfaz del dispositivo virtual. La interfaz realiza el procesamiento de SignalAbort.

El agente de finalización, si está en ejecución, completa todos los comandos pendientes antes de volver al autor de la llamada. Los comandos pendientes se completan con ERROR_OPERATION_ABORTED. Es decir, la función de devolución de llamada se invoca para cada comando de este tipo.

En todos los casos, incluso cuando se devuelven errores, Close libera todos los recursos de la interfaz del dispositivo virtual. Los búferes y otros punteros de interfaz devueltos por VDI dejan de ser válidos.

Es importante asegurarse de que el agente de finalización ha finalizado antes de descargar la biblioteca COM. Si la biblioteca se descarga antes de que el agente de finalización vuelva al autor de la llamada, el proceso podría producir una infracción de la instrucción.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).