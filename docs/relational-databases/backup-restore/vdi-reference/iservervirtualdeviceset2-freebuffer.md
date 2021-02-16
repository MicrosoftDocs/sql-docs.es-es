---
title: IServerVirtualDeviceSet2::FreeBuffer
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDeviceSet2::FreeBuffer.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d5cdd5fade247458526795279302a20ffd7919da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347131"
---
# <a name="iservervirtualdeviceset2freebuffer-vdi"></a>IServerVirtualDeviceSet2::FreeBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **FreeBuffer** obtiene un búfer de memoria compartida del conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDeviceSet2::FreeBuffer (
   LPVOID   pBuffer,
   UINT32   dwSize
);
```

## <a name="parameters"></a>Parámetros

*pBuffer* Devuelve un búfer devuelto por IServerVirtualDeviceSet2::AllocateBuffer.

*dwSize* Es el tamaño del búfer en bytes. Esto no incluye ninguna zona de prefijo solicitada por el cliente. Ese tipo de zona se oculta al servidor y habrá espacio disponible antes de que se devuelva la dirección del búfer.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | Se devuelve el búfer. |
| VD_E_INVALID | Un parámetro no era válido. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).