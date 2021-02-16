---
title: 'Asistente para configuración de seguridad: Elegir los servidores'
description: Describe las propiedades que se encuentran en la página Elegir servidores del Asistente para configuración de seguridad de la creación de reflejo de base de datos.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.choosesrvrs.f1
ms.assetid: 59e23ff3-d7ee-4e32-9629-0b54d3a258f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c2561f81c55ca387f8038ca8ab10f82ea6fce704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350709"
---
# <a name="configure-database-mirroring-wizard-choose-servers-to-configure"></a>Asistente para configuración de la creación de reflejo de la base de datos: Elegir los servidores para configurar 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilice esta página para especificar las instancias de servidor que desea configurar ahora. Debe seleccionar al menos una instancia de servidor antes de continuar con el asistente.  
  
 Si desactiva la casilla para una instancia de servidor, el asistente no realizará ningún cambio en ella. Sin embargo, el asistente le solicitará información acerca de esa instancia y guardará esta información como parte de la configuración de las otras instancias de servidor. Por ejemplo, si desactiva la casilla de la instancia de servidor testigo, el asistente le solicitará la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del testigo, ya que debe crearse un inicio de sesión para esa cuenta como parte de la configuración de seguridad que se guarda en las instancias del servidor principal y reflejado.  
  
 **Para configurar la creación de reflejo de la base de datos mediante SQL Server Management Studio**  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opciones  
 **Instancia de servidor principal**  
 Seleccione esta opción para configurar la seguridad del servidor principal.  
  
 **Instancia del servidor reflejado**  
 Seleccione esta opción para configurar la seguridad del servidor reflejado.  
  
 **Instancia de servidor testigo**  
 Seleccione para configurar la seguridad del servidor testigo (si está presente).  
  
## <a name="see-also"></a>Consulte también  
 [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
