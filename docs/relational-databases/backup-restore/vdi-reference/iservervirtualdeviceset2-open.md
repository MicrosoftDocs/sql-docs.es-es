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
ms.openlocfilehash: 0e4d91e75ee2c030112a1e826366d5caee537ee3
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128859"
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