---
title: IServerVirtualDeviceSet2::Open
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDeviceSet2::Open.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: bb5341a03ee64b3b9d1d23508e1e59048f843b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347082"
---
# <a name="iservervirtualdeviceset2open-vdi"></a>IServerVirtualDeviceSet2::Open (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **Open** abre el dispositivo virtual establecido en el servidor y debe ser la primera llamada realizada mediante el identificador de interfaz proporcionado por COM.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDeviceSet2::Open (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpName
);
```

## <a name="parameters"></a>Parámetros

*lpInstanceName* Esta cadena identifica la instancia de SQL Server a la que se enviará el comando SQL. Se puede pasar NULL para identificar la instancia predeterminada en el equipo actual.

*lpName* Se proporciona desde la primera cláusula VIRTUAL_DEVICE = del comando BACKUP o RESTORE. Este nombre se usa como la clave para obtener acceso al conjunto de dispositivos virtuales creado por el cliente.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | La función se ha realizado correctamente. |
| VD_E_INVALID | El nombre proporcionado no ha identificado un conjunto de dispositivos virtuales accesible para el servidor. |

## <a name="remarks"></a>Observaciones

Después de invocar correctamente esta función, el servidor puede continuar con la configuración del conjunto de dispositivos virtuales mediante GetConfiguration y SetConfiguration.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).