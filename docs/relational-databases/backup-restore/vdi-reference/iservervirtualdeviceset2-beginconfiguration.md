---
title: IServerVirtualDeviceSet2::BeginConfiguration
titlesuffix: SQL Server VDI reference
description: En este artículo se proporciona una referencia para el comando IServerVirtualDeviceSet2::BeginConfiguration.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0d543ee25e92dbaccf44719f98bc838030fa3005
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347152"
---
# <a name="iservervirtualdeviceset2beginconfiguration-vdi"></a>IServerVirtualDeviceSet2::BeginConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

El servidor invoca la función **BeginConfiguration** para empezar la configuración del conjunto de dispositivos virtuales.

## <a name="syntax"></a>Sintaxis

```c
HRESULT IServerVirtualDeviceSet2::BeginConfiguration (
   DWORD   dwFeatures,
   DWORD   dwAlignment,
   DWORD   dwBlockSize,
   DWORD   dwMaxTransferSize,
   DWORD   dwTimeout
);
```

## <a name="parameters"></a>Parámetros

*dwFeatures* La máscara de características modificadas. VDF_WriteMedia o VDF_ReadMedia.

*dwAlignment* La alineación final. Si es 0, el valor predeterminado es dwBlockSize. Debe ser una potencia de 2, >= dwBlockSize y <= 64 KB.

*dwBlockSize* La unidad de transferencia mínima, en bytes. Debe ser una potencia de 2, >=512 y <= 64 KB.

*dwMaxTransferSize* La transferencia de mayor tamaño que se intentará. Debe ser un múltiplo de 64 KB.

*dwTimeout* Milisegundos para esperar a que el cliente principal termine de declarar las áreas de búfer que va a proporcionar.

## <a name="return-value"></a>Valor devuelto

|Valor devuelto | Explicación |
|---|---|
| NOERROR | El conjunto de dispositivos virtuales se encuentra en el estado Configurable. |
| VD_E_ABORT | Se ha invocado SignalAbort. |
| VD_E_PROTOCOL | El conjunto de dispositivos virtuales no se encuentra en el estado Conectado. |

## <a name="remarks"></a>Observaciones

Después de invocar esta función, el conjunto de dispositivos virtuales pasa al estado Configurable, en el que se decide el diseño del búfer.
Una vez establecida la configuración básica (según los parámetros), estos valores permanecen fijos durante la vida del conjunto de dispositivos virtuales. La propiedad de alineación del conjunto de dispositivos virtuales se usa para controlar la alineación de los búferes de datos. Este valor establece un valor de alineación mínimo que se puede invalidar de forma individual en cada búfer.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea la [información general de la referencia de interfaces de dispositivo virtual de SQL Server](reference-virtual-device-interface.md).