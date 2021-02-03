---
description: MSSQLSERVER_-2
title: MSSQLSERVER_-2 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "-2"
helpviewer_keywords:
- -2 (Database Engine error)
ms.assetid: f37a7b7d-26e1-4b9e-bcb4-57f7805393d2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 73149dd75fd2b6829deb6d2f56fd7ddeb2f96770
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187507"
---
# <a name="mssqlserver_-2"></a>MSSQLSERVER_-2
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|-2|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Tiempo de espera agotado.  El tiempo de espera transcurrió antes de que se completase la operación, o bien el servidor no responde. (Microsoft SQL Server, Error: -2)|  
  
## <a name="explanation"></a>Explicación  
El cliente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede conectar al servidor. Este error puede producirse porque el firewall en el servidor ha rechazado la conexión.  
  
## <a name="user-action"></a>Acción del usuario  
Asegúrese de haber configurado el firewall en la instancia del servidor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para aceptar las conexiones.  
  
## <a name="see-also"></a>Consulte también  
[Configure the Windows Firewall to Allow SQL Server Access (Configurar el Firewall de Windows para permitir el acceso a SQL Server)](~/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)  
[Configuración de Firewall de Windows para el acceso al motor de base de datos](~/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Protocolos de red y bibliotecas de red](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configuración de red de cliente](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Habilitar o deshabilitar un protocolo de red de servidor](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
