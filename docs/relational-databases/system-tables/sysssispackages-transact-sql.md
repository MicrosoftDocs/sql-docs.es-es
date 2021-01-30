---
description: sysssispackages (Transact-SQL)
title: sysssispackages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdtspackages90_TSQL
- sysdtspackages90
dev_langs:
- TSQL
helpviewer_keywords:
- sysssispackages system table
ms.assetid: 66155dcd-dcdb-4e33-a242-1625828ad8d2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 31f1833808260508f29a858381d258f21ebd744a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186664"
---
# <a name="sysssispackages-transact-sql"></a>sysssispackages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada paquete que se guarda en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esta tabla se almacena en la base de datos **msdb** .  
  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Identificador único del paquete.|  
|**id**|**uniqueidentifier**|Identificador único global (GUID) del paquete.|  
|**description**|**nvarchar**|Descripción opcional del paquete.|  
|**CreateDate**|**datetime**|Fecha de creación del paquete.|  
|**folderId**|**uniqueidentifier**|GUID de la carpeta lógica en la que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] incluye el paquete.|  
|**ownersid**|**varbinary**|Identificador de seguridad único del usuario que creó el paquete.|  
|**packagedata**|**image**|El paquete.|  
|**packageformat**|**int**|Formato con el que se guarda el paquete:<br /><br /> Un valor de 2 indica que el paquete se guarda en el [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] formato.<br /><br /> Un valor de 3 indica que el paquete se guarda en formato de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o posterior.|  
|**packagetype**|**int**|Cliente que creó el paquete. Los valores posibles son los siguientes:<br /><br /> 0 (valor predeterminado)<br /><br /> 1 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Asistente para importación y exportación)<br /><br /> 3 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] replicación)<br /><br /> 5 ( [!INCLUDE[ssIS](../../includes/ssis-md.md)] Diseñador)<br /><br /> 6 (Asistente o Diseñador de planes de mantenimiento).<br /><br /> <br /><br /> Tenga en cuenta que los valores de esta columna se corresponden con la <xref:Microsoft.SqlServer.Dts.Runtime.DTSPackageType> enumeración.|  
|**vermajor**|**int**|Última versión importante del paquete.|  
|**verminor**|**int**|Última versión de menor importancia del paquete.|  
|**verbuild**|**int**|Última versión de compilación del paquete.|  
|**vercomments**|**nvarchar**|Comentarios acerca de la versión del paquete.|  
|**verid**|**uniqueidentifier**|GUID de la versión del paquete.|  
|**isencrypted**|**bit**|Valor booleano que indica si el paquete está cifrado.|  
|**readrolesid**|**varbinary**|Rol de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que puede cargar paquetes.|  
|**writerolesid**|**varbinary**|Rol de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que puede guardar paquetes.|  
  
## <a name="see-also"></a>Consulte también  
 [Paquetes de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)  
  
  
