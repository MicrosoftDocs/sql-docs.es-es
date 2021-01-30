---
description: sys.dm_db_objects_impacted_on_version_change (Azure SQL Database)
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 6e04a46841fe9e1df60133c9f3ff9495e4375abc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206266"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Esta vista del sistema con ámbito de base de datos está diseñada para proporcionar un sistema de alerta rápida que permita determinar los objetos que se verán afectados por una actualización de versión importante en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Puede utilizar la vista antes o después de la actualización para obtener una enumeración completa de los objetos afectados. Tendrá que consultar esta vista en cada base de datos para obtener una perspectiva completa de todo el servidor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|class|valor **int** NO NULL|La clase del objeto que se verá afectado:<br /><br /> **1** = restricción<br /><br /> **7** = Índices y montones|  
|class_desc|**nvarchar (60)** NO NULL|Descripción de la clase:<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|valor **int** NO NULL|Identificador de objeto de la restricción, o identificador de objeto de la tabla que contiene el índice o el montón.|  
|minor_id|valor **int** ACEPTA|**NULL** para restricciones<br /><br /> Index_id para índices y montones|  
|dependency|**nvarchar (60)** NO NULL|Descripción de la dependencia que está afectando a una restricción o a un índice. El mismo valor también se utiliza para las advertencias generadas durante la actualización.<br /><br /> Ejemplos:<br /><br /> **space** (para valores intrínsecos)<br /><br /> **geometry** (para UDT de sistema)<br /><br /> **geography::Parse** (para el método UDT de sistema)|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se muestra una consulta en **sys.dm_db_objects_impacted_on_version_change** para buscar los objetos a los que afecta una actualización a la siguiente versión principal del servidor  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>Observaciones  
  
### <a name="how-to-update-impacted-objects"></a>Cómo actualizar los objetos afectados  
 Los pasos ordenados siguientes describen la acción correctiva que se deberá realizar después de la próxima actualización de versión de servicio del mes de junio.  
  
|Pedido|Objeto afectado|Acción correctora|  
|-----------|---------------------|-----------------------|  
|1|**Índices**|Vuelva a generar todos los índices identificados por **Sys.dm_db_objects_impacted_on_version_change** por ejemplo:  `ALTER INDEX ALL ON <table> REBUILD`<br />o<br />`ALTER TABLE <table> REBUILD`|  
|2|**Object**|Todas las restricciones identificadas por **sys.dm_db_objects_impacted_on_version_change** deben volver a validarse tras volver a calcular los datos geometry y geography de la tabla subyacente. Para las restricciones, vuelva a realizar la validación mediante ALTER TABLE. <br />Por ejemplo: <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />o<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  
