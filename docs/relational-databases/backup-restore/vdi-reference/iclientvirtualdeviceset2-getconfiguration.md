---
title: IClientVirtualDeviceSet2::GetConfiguration
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IClientVirtualDeviceSet2::GetConfiguration.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4ee3d601a463e5ee3d9c766f41180c867955ed5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347233"
---
# <a name="iclientvirtualdeviceset2getconfiguration-vdi"></a>IClientVirtualDeviceSet2::GetConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

La función **GetConfiguration** se usa para esperar a que el servidor configure el conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IClientVirtualDeviceSet2::GetConfiguration (
   DWORD         dwTimeOut,
   VDConfig*      pCfg
);
```

## <a name="parameters"></a>Parámetros

*DwTimeOut* El tiempo de espera, en milisegundos. Use INFINITE para evitar el tiempo de espera.

*pCfg* Tras su correcta ejecución, contiene la configuración seleccionada por el servidor. Para más información, vea Configuración.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | Se devolvió la configuración. |
| VD_E_ABORT | Se invocó SignalAbort. |
| VD_E_TIMEOUT | Se agotó el tiempo de espera de la función. |

## <a name="remarks"></a>Observaciones

Esta función se bloquea en un estado de alerta. Después de la invocación correcta, se pueden abrir los dispositivos del conjunto de dispositivos virtuales.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).