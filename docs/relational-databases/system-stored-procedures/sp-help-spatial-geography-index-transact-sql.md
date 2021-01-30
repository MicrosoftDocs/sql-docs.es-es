---
description: sp_help_spatial_geography_index (Transact-SQL)
title: sp_help_spatial_geography_index (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_index
- sp_help_spatial_geography_index_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_index procedure
ms.assetid: c9bf5675-eafc-4d71-bfdb-da963384fa0c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: adccb4ff123d5ea91691d57c03556c51441abed3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212051"
---
# <a name="sp_help_spatial_geography_index-transact-sql"></a>sp_help_spatial_geography_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los nombres y valores para un conjunto especificado de propiedades sobre un índice espacial de **geografía** . El resultado se devuelve en un formato de tabla. Puede decidir devolver un conjunto básico de propiedades o todas las propiedades del índice.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_spatial_geography_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ] 'verboseoutput' ]   
     [ , [ @query_sample = ] 'query_sample' ]   
```  
  
## <a name="arguments"></a>Argumentos  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="properties"></a>Propiedades  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener asignado un rol PUBLIC para tener acceso al procedimiento. Requiere el permiso READ ACCESS en el servidor y el objeto.  
  
## <a name="remarks"></a>Comentarios  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente `sp_help_spatial_geography_index` se utiliza para investigar el índice espacial **geography** **SIndx_SpatialTable_geography_col2** definido en la tabla **geography_col** para **\@** el ejemplo de consulta determinado en. En este ejemplo se devuelven solo las propiedades básicas del índice especificado.  
  
```  
declare @qs geography  
        ='POLYGON((-90.0 -180, -90 180.0, 90 180.0, 90 -180, -90 -180.0))';  
exec sp_help_spatial_geography_index 'geography_col', 'SIndx_SpatialTable_geography_col2', 0, @qs;  
```  
  
 El rectángulo de selección de una instancia de **Geography** es toda la tierra.  
  
## <a name="requirements"></a>Requisitos  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de índice espacial](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geography_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)   
 [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
