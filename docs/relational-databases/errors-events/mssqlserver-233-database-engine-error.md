---
title: MSSQLSERVER_233 | Microsoft Docs
description: El cliente de SQL Server no se puede conectar al servidor. Vea una explicación del error 233 y las posibles resoluciones.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
f1_keywords:
- "233"
helpviewer_keywords:
- 233 (Database Engine error)
ms.assetid: 201665dc-7ac8-4c19-90d3-33354c5caa72
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bf1a3e2f94c221caa4b70105aaccc331c6082b97
ms.sourcegitcommit: 36fe62a3ccf34979bfde3e192cfa778505add465
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94521079"
---
# <a name="mssqlserver_233"></a>MSSQLSERVER_233
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|233|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Se estableció correctamente una conexión con el servidor, pero luego se produjo un error durante el proceso de inicio de sesión. (proveedor: Proveedor de memoria compartida; error: 0 - No hay ningún proceso en el otro extremo de la canalización). (Microsoft SQL Server, Error: 233)|  
  
## <a name="explanation"></a>Explicación  
El cliente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede conectar al servidor. Este error se puede producir porque el servidor no está configurado para aceptar conexiones remotas.  
  
## <a name="user-action"></a>Acción del usuario  
Use la herramienta Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acepte conexiones remotas.  
  
## <a name="see-also"></a>Consulte también  
[Protocolos de red y bibliotecas de red](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configuración de red de cliente](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Habilitar o deshabilitar un protocolo de red de servidor](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
