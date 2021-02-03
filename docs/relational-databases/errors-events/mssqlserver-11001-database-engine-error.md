---
description: MSSQLSERVER_11001
title: MSSQLSERVER_11001 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "12001"
helpviewer_keywords:
- 11001 (Database Engine error)
ms.assetid: 53d4d63a-61e3-441f-bfe9-9d44f7a05fd4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1805acf488eb9559db479a6915450576296f33f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197230"
---
# <a name="mssqlserver_11001"></a>MSSQLSERVER_11001
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|11001|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Error al establecer una conexión al servidor.  La causa del problema en la conexión a SQL Server puede deberse a que SQL Server no permite conexiones remotas en su configuración predeterminada. (proveedor: Proveedor TCP, error: 0: Este host es desconocido). (Proveedor de datos SqlClient de .NET)|  
  
## <a name="explanation"></a>Explicación  
El cliente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede conectar al servidor. Este error se puede producir porque el cliente no puede resolver el nombre del servidor o porque el nombre del servidor no es correcto.  
  
## <a name="user-action"></a>Acción del usuario  
Asegúrese de haber escrito correctamente el nombre del servidor en el cliente y que puede resolver el nombre del servidor desde el cliente. Para comprobar la resolución de nombres de TCP/IP, puede usar el comando **ping** en el sistema operativo Windows.  
  
## <a name="see-also"></a>Consulte también  
[Protocolos de red y bibliotecas de red](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configuración de red de cliente](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Habilitar o deshabilitar un protocolo de red de servidor](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
