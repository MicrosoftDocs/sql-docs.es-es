---
title: Requisitos de base de datos
description: Utilice Administrador de configuración de Master Data Services para crear y configurar la base de datos de Master Data Services, que almacena todos los datos maestros.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: fe731839-c5c4-4884-bb6a-644eca28bb30
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: a365a3fc39d31dd57a11d068eff5d0f4706ae2e6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236463"
---
# <a name="database-requirements-master-data-services"></a>Requisitos de base de datos (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Todos los datos maestros están almacenados en una base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . El equipo que hospeda esta base de datos debe ejecutar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Use [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] para crear y configurar la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] en un equipo local o remoto. Si mueve la base de datos de un entorno a otro, puede mantener la información en un entorno nuevo si asocia el servicio web de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] y [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] a la base de datos en la nueva ubicación.  
  
> [!NOTE]  
>  Cualquier equipo en el que instale los componentes de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] debe tener las licencias pertinentes. Para obtener más información, consulte el Contrato de licencia para el usuario final (CLUF).  
  
## <a name="requirements"></a>Requisitos  
 Para poder crear una base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , asegúrese de que se cumplen los siguientes requisitos.  
  
### <a name="sql-server-edition"></a>Edición de SQL Server  
 La base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] se puede hospedar en las siguientes ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Enterprise (64 bits) x64  
  
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Developer (64 bits) x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence (64 bits) x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise (64 bits) x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer (64 bits) x64  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Business Intelligence (64 bits) x64  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise (64 bits) x64: actualizar solo desde [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Enterprise  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Developer (64 bits) x64  
  
-   Microsoft SQL Server 2008 R2 Enterprise (64 bits) x64  
  
-   Microsoft SQL Server 2008 R2 Developer (64 bits) x64  
  
 Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). 
  
### <a name="operating-system"></a>Sistema operativo  
 Para obtener más información sobre los sistemas operativos Windows admitidos y otros requisitos del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], see [Hardware and Software Requirements for Installing SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
### <a name="accounts-and-permissions"></a>Cuentas y permisos  
  
|Tipo|Descripción|  
|----------|-----------------|  
|Cuenta de usuario|En [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)], puede usar una cuenta de Windows o una cuenta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el fin de conectar a la instancia [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para hospedar la base de datos [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . La cuenta de usuario debe pertenecer al rol de servidor **sysadmin** de la instancia del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para obtener más información sobre el rol **sysadmin** , vea [Roles de nivel de servidor](../../relational-databases/security/authentication-access/server-level-roles.md).|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Cuenta de administrador de|Cuando cree una base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , debe especificar una cuenta de usuario de dominio para que sea el administrador del sistema de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Para todas las aplicaciones web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] asociadas a esta base de datos, este usuario puede actualizar todos los modelos y todos los datos de todas las áreas funcionales. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md).|  
  
### <a name="database-backup"></a>Copia de seguridad de bases de datos  
 Se recomienda hacer una copia de seguridad de toda la base de datos diariamente en el momento de menor actividad y hacer una copia de seguridad de los registros de transacciones con más frecuencia, en función de las necesidades del entorno. Para obtener más información sobre las copias de seguridad de bases de datos, vea [Información general de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)   
 [Crear una base de datos de Master Data Services](../../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Master Data Services base de datos](../../master-data-services/master-data-services-database.md)   
 [Cuadro de diálogo conectar con una base de datos de Master Data Services](../../master-data-services/connect-to-a-master-data-services-database-dialog-box.md)   
 [Asistente Crear base de datos &#40;Administrador de configuración de Master Data Services&#41;](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)  
  
