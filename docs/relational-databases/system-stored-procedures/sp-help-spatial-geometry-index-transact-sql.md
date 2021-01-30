---
description: sp_help_spatial_geometry_index (Transact-SQL)
title: sp_help_spatial_geometry_index (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_index
- sp_help_spatial_geometry_index_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index procedure
ms.assetid: f1bcefb1-09c8-4b49-8c51-5d471065849f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: af6ed2f873a79be59af9adcd40311ef02fc715dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207342"
---
# <a name="sp_help_spatial_geometry_index-transact-sql"></a>sp_help_spatial_geometry_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los nombres y valores para un conjunto especificado de propiedades sobre un índice espacial de **geometría** . El resultado se devuelve en un formato de tabla. Puede decidir devolver un conjunto básico de propiedades o todas las propiedades del índice.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ] 'verboseoutput'   
     [ , [ @query_sample = ] 'query_sample']   
```  
  
## <a name="arguments"></a>Argumentos  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="property-valuereturn-value"></a>Valor de propiedad y valor devuelto  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener asignado un rol PUBLIC para tener acceso al procedimiento. Requiere el permiso READ ACCESS en el servidor y el objeto.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades que contienen valores NULL están incluidas en el conjunto que se devuelve.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente `sp_help_spatial_geometry_index` se utiliza para investigar el índice espacial **SIndx_SpatialTable_geometry_col2** definido en el **geometry_col** de tabla para el ejemplo de consulta determinado en **\@ q**. En este ejemplo se devuelven solo las propiedades básicas del índice especificado.  
  
```  
declare @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
exec sp_help_spatial_geometry_index 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de índice espacial](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index_xml](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)   
 [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
