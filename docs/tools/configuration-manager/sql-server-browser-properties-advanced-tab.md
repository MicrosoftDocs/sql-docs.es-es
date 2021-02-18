---
title: Propiedades de SQL Server Browser (pestaña Avanzadas)
description: Obtenga información sobre las opciones de la pestaña Opciones avanzadas del cuadro de diálogo Propiedades de SQL Server Browser, como el directorio de volcado y el id. de la instancia.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ba79137a-cb72-4bf3-a650-e11d02cfce10
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: fc2075fe1875a8d74b101f79a691f5d88c26e04f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345222"
---
# <a name="sql-server-browser-properties-advanced-tab"></a>Propiedades de SQL Server Browser (pestaña Avanzadas)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  El programa Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta como un servicio en el servidor. El Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha las solicitudes entrantes de recursos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y proporciona información sobre las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instaladas en el equipo.  
  
## <a name="options"></a>Opciones  
 **Agrupado**  
 Indica si este servicio está instalado como un recurso de un servidor en clúster.  
  
 **Informes de comentarios de clientes**  
 Indica si se ha habilitado la supervisión de la calidad del servicio en este servicio. Para obtener más información acerca de los informes de comentarios de clientes, busque el tema sobre la configuración de informes de errores y uso en los Libros en pantalla.  
  
 **Volcar directorio**  
 Ubicación de los volcados de memoria si se produce un error.  
  
 **Informes de errores**  
 Cuando se establece en **Yes**, el programa Dr. Watson envía información a [!INCLUDE[msCoName](../../includes/msconame-md.md)] o a su servidor de errores si se produce un error grave. Para obtener más información acerca de Informes de errores, busque el tema sobre la configuración de informes de errores y uso en los Libros en pantalla.  
  
 **Id. de instancia**  
 Indica la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ha usado esta instancia del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La instancia predeterminada es **MSSQL10_50.MSSQLSERVER**.  
  
## <a name="see-also"></a>Consulte también  
 [Servicio SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md)  
  
  
