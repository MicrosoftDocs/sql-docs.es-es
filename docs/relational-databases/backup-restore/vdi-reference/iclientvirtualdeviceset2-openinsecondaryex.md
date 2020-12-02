---
title: IClientVirtualDeviceSet2::OpenInSecondaryEx
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IClientVirtualDeviceSet2::OpenInSecondaryEx.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: a5b2a680e34d6aa6c174cc356a2cb99bee75131f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128952"
---
# <a name="iclientvirtualdeviceset2openinsecondaryex-vdi"></a>IClientVirtualDeviceSet2::OpenInSecondaryEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **OpenInSecondaryEx** abre el conjunto de dispositivos virtuales en un cliente secundario. El cliente principal ya debe haber usado CreateEx y GetConfiguration para configurar el conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IClientVirtualDeviceSet2::OpenInSecondaryEx (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpSetName
);
```

## <a name="parameters"></a>Parámetros

*lpInstanceName* Esta cadena identifica la instancia de SQL Server a la que se enviará el comando SQL.

*lpSetName* Identifica el conjunto. Este nombre distingue entre mayúsculas y minúsculas, y debe coincidir con el nombre usado por el cliente principal al invocar IClientVirtualDeviceSet2::Create.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | La función se ha realizado correctamente. |
| VD_E_PROTOCOL | No se ha abierto el conjunto de dispositivos virtuales o no está listo para aceptar solicitudes abiertas de clientes secundarios. |
| VD_E_ABORT | La operación se va a anular. |

## <a name="remarks"></a>Observaciones

Cuando se usa un modelo de varios procesos, el cliente principal es responsable de detectar la finalización normal y anómala de los clientes secundarios.

El nombre de instancia debe identificar la instancia en la que se emite T-SQL. NULL identifica la instancia predeterminada. No se acepta el prefijo "machineName\".

OpenInSecondaryEx reemplaza al comando IClientVirtualDeviceSet::OpenInSecondary original definido en la interfaz original de SQL Server, versión 7.0. En el desarrollo nuevo se debe usar OpenInSecondaryEx.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).