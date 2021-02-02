---
description: Servicio de captura de datos modificados para Oracle de Attunity
title: Change Data Capture Service para Oracle de Attunity | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 22ec8a5c-9550-4d38-8a4a-485ec3e53ea8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65f610b26444f86de750b3f21b175df4fedc3792
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813154"
---
# <a name="change-data-capture-service-for-oracle-by-attunity"></a>Servicio de captura de datos modificados para Oracle de Attunity

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El servicio CDC para Oracle es un servicio de Windows que examina los registros de transacciones de Oracle y captura los cambios a las tablas de Oracle deseadas en tablas de cambios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las tablas de cambios de SQL donde se almacenan los cambios capturados de Oracle son el mismo tipo de tablas de cambios empleadas en la característica de captura de datos modificados nativa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esto hace que el uso de estos cambios sea tan sencillo como el de los cambios realizados a las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="installation"></a>Instalación  

Descargue Microsoft Change Data Capture Designer y Service para Oracle de Attunity para la versión de SQL Server correspondiente de los vínculos siguientes:

- [Feature Pack de CDC Designer/Service para Oracle de Attunity de Microsoft SQL Server 2012 Integration Services](https://www.microsoft.com/download/details.aspx?id=51606)
- [Feature pack de CDC Designer/Service para Oracle de Attunity de Microsoft SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=55802)
- [Microsoft SQL Server 2017 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=56610)
- [Microsoft SQL Server 2019 Integration Services Feature Pack](https://www.microsoft.com/download/details.aspx?id=100303)
  
 El servicio CDC para Oracle se puede instalar en cualquier equipo compatible con Windows que tenga acceso a las bases de datos de origen de Oracle que se van a capturar y a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino donde reside la base de datos CDC de destino. El servicio CDC no necesita ninguna instalación local de la base de datos de Oracle ni de la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , solo sus clientes compatibles. Para obtener información acerca de dónde instalar los componentes de base de datos necesarios, vea **Requisitos previos de la base de datos** en este tema.  
  
 La instalación del servicio CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para Oracle coloca la interfaz de usuario de configuración del servicio y el programa de servicio en la ubicación seleccionada. El servicio CDC para Oracle se configura de forma independiente mediante la Consola de configuración del servicio CDC de Oracle. Para obtener más información acerca de cómo configurar el servicio CDC de Oracle, vea [Servicio de captura de datos modificados para Oracle de Attunity (Ayuda de F1)](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md).  
  
 El servicio CDC para Oracle se puede instalar en cualquier equipo compatible con Windows donde esté instalado [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Native Client; no es necesario instalarlo en el mismo equipo donde está instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino.  
  
## <a name="supported-windows-environments"></a>Entornos de Windows admitidos  
 El Servicio de captura de datos modificados para Oracle de Attunity se puede ejecutar en los entornos de Windows siguientes:  
  
-   Windows 8 y 8.1  
-   Windows 10  
-   Windows Server 2012 y 2012 R2
-   Windows Server 2016
  
## <a name="database-prerequisites"></a>Requisitos previos de la base de datos  
 Para trabajar con el servicio CDC de Oracle, debe instalar un cliente de Oracle que sea compatible con la versión de la base de datos de Oracle. Se trata de un requisito previo que se debe obtener de Oracle e instalar antes de instalar el servicio CDC de Oracle. Además, debe instalar el cliente ODBC de SQL Server mediante el programa de instalación de SQL Server.  
  
 El servicio CDC para Oracle admite las versiones siguientes:  
  
### <a name="source-oracle-database"></a>Base de datos de Oracle de origen  
  
-   Oracle Database 10g versión 2
-   Oracle Database 11g versión 1 y versión 2
-   Oracle Database 12C en instalación clásica. (La instalación de tipo multiinquilino no es compatible).  
-   Oracle Database 18c en instalación clásica. (La instalación de tipo multiinquilino no es compatible). 
-   Oracle Database 19c en instalación clásica. (La instalación de tipo multiinquilino no es compatible). 
  
### <a name="target-sql-server-database"></a>Base de datos de SQL Server de destino  
 Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
## <a name="running-the-installation-program"></a>Ejecutar el programa de instalación  
 Para instalar el servicio CDC para Oracle, abra el asistente para la instalación para la plataforma Windows que esté usando (32 o 64 bits) y siga las instrucciones de la pantalla.  
  
## <a name="uninstalling-change-data-capture-service-for-oracle-by-attunity"></a>Desinstalar el servicio de captura de datos modificados para Oracle de Attunity  
 La desinstalación del servicio CDC para Oracle se realiza mediante Panel de control, Programas y características.  
  
 Al desinstalar el servicio CDC no se eliminan las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creadas. Para quitar completamente la herramienta, debe quitar la base de datos MSXDBCDC y las bases de datos CDC específicas que se crearon en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino con la que trabajó.  
  
 Si desinstala el software del servicio CDC de un equipo y lo instala en otro equipo, solo tiene que proporcionar las definiciones siguientes:  
  
-   Cuenta de servicio  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Cadena de conexión y credenciales  
  
-   La contraseña maestra  
  
 Todas las demás definiciones están almacenadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y están disponibles de la instalación anterior en el otro equipo.  
  
## <a name="in-this-documentation"></a>En esta documentación  
  
-   [Servicio de captura de datos modificados para Oracle de Attunity (Arquitectura del sistema)](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-system-architecture.md)  
  
-   [El servicio CDC de Oracle](../../integration-services/change-data-capture/the-oracle-cdc-service.md)  
  
-   [Servicio de captura de datos modificados para Oracle de Attunity (Ayuda de F1)](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md)  
  
-   [Servicio de captura de datos modificados para Oracle de Attunity (Guía de procedimientos)](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-how-to-guide.md)  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con el servicio CDC de Oracle](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md)  
  
  
