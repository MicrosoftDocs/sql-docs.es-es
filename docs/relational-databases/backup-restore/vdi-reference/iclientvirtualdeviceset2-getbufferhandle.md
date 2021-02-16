---
title: IClientVirtualDeviceSet2::GetBufferHandle
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IClientVirtualDeviceSet2::GetBufferHandle.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3751af7f6d784d58fce46dfe24605260f530a99e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347280"
---
# <a name="iclientvirtualdeviceset2getbufferhandle-vdi"></a>IClientVirtualDeviceSet2::GetBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Es posible que algunas aplicaciones requieran más de un proceso para operar en los búferes devueltos por **IClientVirtualDevice2::GetCommand**. En esos casos, el proceso que recibe el comando puede usar **GetBufferHandle** para obtener un identificador independiente del proceso que identifica el búfer. Este identificador se puede comunicar a cualquier otro proceso que también tenga abierto el mismo conjunto de dispositivos virtuales. Después, ese proceso usaría IClientVirtualDeviceSet2::MapBufferHandle para obtener la dirección del búfer. Es probable que la dirección que obtenga sea distinta a la de su asociado, ya que cada proceso puede asignar búferes en direcciones diferentes.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IClientVirtualDeviceSet2::GetBufferHandle (
   BYTE*         pBuffer,
   DWORD*      pBufferHandle
);
```

## <a name="parameters"></a>Parámetros

*pBuffer* Es la dirección de un búfer que se obtiene de un comando de lectura o escritura.

*pBufferHandle* Se devuelve un identificador único para el búfer.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | La función se ha realizado correctamente. |
| VD_E_PROTOCOL | El conjunto de dispositivos virtuales no está abierto actualmente. |
| VD_E_INVALID | pBuffer no es una dirección válida. |

## <a name="remarks"></a>Observaciones

El proceso que invoca la función GetBufferHandle es responsable de invocar IClientVirtualDevice2::CompleteCommand cuando se complete la transferencia de datos.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).