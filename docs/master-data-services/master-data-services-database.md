---
title: Base de datos de Master Data Services
description: La base de datos de Master Data Services contiene toda la información del sistema de Master Data Services y es fundamental para una implementación de Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], about the database
- database [Master Data Services]
ms.assetid: 5f590cc1-6ec2-4b8c-a598-03de0f6051a0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 59d430ea44a83d3795d7d3e3473917ed1c14345f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338211"
---
# <a name="master-data-services-database"></a>Base de datos de Master Data Services

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  La base de datos contiene toda la información para el sistema [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Es un punto central en una implementación de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Las base de datos [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] :  
  
-   Almacena los valores, objetos de base de datos y los datos que requiere el sistema [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
-   Contiene las tablas de ensayo que se van a utilizar para procesar los datos de los sistemas de origen.  
  
-   Proporciona objetos de esquema y de base de datos para almacenar los datos maestros de los sistemas de origen.  
  
-   Admite la funcionalidad de control de versiones, incluso la validación de reglas de negocios y notificaciones de correo electrónico.  
  
-   Proporciona vistas para sistemas de suscripción que necesitan recuperar datos de la base de datos.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Tabla de almacenamiento provisional de miembros hoja &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Tabla de almacenamiento provisional de miembros consolidados &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Tabla de almacenamiento provisional de relaciones &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md)  
  
-   [Errores del proceso de almacenamiento provisional &#40;Master Data Services&#41;](../master-data-services/staging-process-errors-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Crear una base de datos de Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [&#40;de seguridad de objetos de base de datos Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)   
 [Inicios de sesión, usuarios y roles en bases de datos &#40;Master Data Services&#41;](../master-data-services/database-logins-users-and-roles-master-data-services.md)  
  
  
