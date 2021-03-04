---
description: Uso de resolución de IP de red transparente
title: Uso de resolución de IP de red transparente | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
author: chris-rossi
ms.author: v-chross
ms.openlocfilehash: dbbafac3c6b895d76ec4c931602194d767f9aabd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837502"
---
# <a name="using-transparent-network-ip-resolution"></a>Uso de resolución de IP de red transparente
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Propósito
La resolución de IP de red transparente (TNIR) es una revisión de la característica MultiSubnetFailover existente. Afecta la secuencia de conexión del controlador cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. TNIR interactúa con MultiSubnetFailover para proporcionar las tres secuencias de conexión siguientes:<br />
* 0: se intenta una IP, seguida de todas las direcciones IP en paralelo.
* 1: todas las direcciones IP se intentan en paralelo.
* 2: todas las direcciones IP se intentan una tras otra.

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportamiento|
|--------|--------|--------|
|True|True|1|
|True|False|0|
|False|True|1|
|False|False|2|

## <a name="setting-transparent-network-ip-resolution"></a>Configuración de la resolución de IP de red transparente
TransparentNetworkIPResolution está habilitado de forma predeterminada. MultiSubnetFailover está deshabilitado de forma predeterminada. Las páginas siguientes proporcionan más información sobre la configuración de estas propiedades: 
- [Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server](..\applications\using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Propiedades de inicialización y autorización](..\ole-db-data-source-objects\initialization-and-authorization-properties.md)

## <a name="see-also"></a>Consulte también 
[Controlador OLE DB para la compatibilidad de SQL Server con la alta disponibilidad y la recuperación ante desastres](./oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)
