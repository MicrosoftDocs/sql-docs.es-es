---
description: MSSQLSERVER_-1
title: MSSQLSERVER_-1 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "-1"
helpviewer_keywords:
- -1 (Database Engine error)
ms.assetid: bad25b91-eaed-46c0-a5b7-71117a32304c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6b750fa65b41bd4e5125d48306caf9178623b57f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197557"
---
# <a name="mssqlserver_-1"></a>MSSQLSERVER_-1
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|-1|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Error al establecer una conexión al servidor.  Cuando se conecta con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o con versiones posteriores, la configuración predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no permite conexiones remotas. (proveedor: Interfaces de red de SQL; error: 28 - el servidor no admite el protocolo requerido) (Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], error: -1)|  
  
## <a name="explanation"></a>Explicación  
El cliente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede conectar al servidor. Este error podría deberse a uno de los siguientes motivos:  
  
-   El nombre de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificado no es válido.  
  
-   Los protocolos de canalizaciones con nombre o TCP no están habilitados.  
  
-   El firewall en el servidor ha rechazado la conexión.  
  
-   El servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser (sqlbrowser) no se inició.  
  
## <a name="user-action"></a>Acción del usuario  
Para resolver este error, intente una de las siguientes acciones:  
  
-   Compruebe la ortografía del nombre de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se especifica en la cadena de conexión.  
  
-   Use la herramienta Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y aceptar las conexiones remotas a través de los protocolos de canalizaciones con nombre o TCP. Para obtener más información sobre cómo aceptar las conexiones remotas, vea [Habilitar o deshabilitar un protocolo de red de servidor](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).  
  
-   Asegúrese de que ha configurado el firewall en la instancia de servidor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para abrir los puertos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el puerto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser (UDP 1434).  
  
-   Asegúrese de que el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser se inició en el servidor.  
  
## <a name="see-also"></a>Consulte también  
[Configuración de Firewall de Windows para el acceso al motor de base de datos](~/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Protocolos de red y bibliotecas de red](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configuración de red de cliente](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurar protocolos de cliente](~/database-engine/configure-windows/configure-client-protocols.md)  
[Habilitar o deshabilitar un protocolo de red de servidor](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
