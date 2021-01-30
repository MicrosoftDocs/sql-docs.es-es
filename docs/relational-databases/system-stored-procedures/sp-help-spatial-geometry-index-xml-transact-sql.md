---
description: sp_help_spatial_geometry_index_xml (Transact-SQL)
title: sp_help_spatial_geometry_index_xml (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_index_xml_TSQL
- sp_help_spatial_geometry_index_xml
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index_xml procedure
ms.assetid: 9668ae6d-9ed5-418e-bb9a-9e7b66f7dd16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0630c37d00e6f9d8496d1fd0b0786987c46e96f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185393"
---
# <a name="sp_help_spatial_geometry_index_xml-transact-sql"></a>sp_help_spatial_geometry_index_xml (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los nombres y valores para un conjunto especificado de propiedades sobre un índice espacial de **geometría** . Puede decidir devolver un conjunto básico de propiedades o todas las propiedades del índice.  
  
 Los resultados se devuelven en un fragmento XML que muestra el nombre y valor de las propiedades seleccionadas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ]'{ 0 | 1 }]   
     [ , [ @query_sample = ] 'query_sample' ]   
     [ ,.[ @xml_output = ] 'xml_output' ]   
```  
  
## <a name="arguments"></a>Argumentos  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="properties"></a>Propiedades  
 Vea [argumentos y propiedades de procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Permisos  
 El usuario debe ser miembro del rol **Public** . Requiere el permiso READ ACCESS en el servidor y el objeto.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades que contienen valores NULL están incluidas en el conjunto XML que se devuelve.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente `sp_help_spatial_geometry_index_xml` se utiliza para investigar el índice espacial **SIndx_SpatialTable_geometry_col2** definido en el **geometry_col** de tabla para el ejemplo de consulta determinado en **\@ q**. En este ejemplo se devuelven las propiedades básicas del índice especificado en un fragmento XML que muestra el nombre y valor de las propiedades seleccionadas.  
  
 Después, se ejecuta una consulta [XQuery](../../xquery/xquery-basics.md) en el conjunto de resultados, que devuelve una propiedad concreta.  
  
```  
DECLARE @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
DECLARE @x xml;  
EXEC sp_help_spatial_geometry_index_xml 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs, @x output;  
SELECT @x.value('(/Primary_Filter_Efficiency/text())[1]', 'float');  
```  
  
 De forma similar a [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md), este procedimiento almacenado proporciona acceso mediante programación más sencillo a las propiedades de un índice espacial y notifica el conjunto de resultados en XML.  
  
## <a name="requirements"></a>Requisitos  
  
## <a name="see-also"></a>Consulte también  
 [Argumentos y propiedades de los procedimientos almacenados de índice espacial](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)   
 [Procedimientos almacenados de índice espacial](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Conceptos básicos de XQuery](../../xquery/xquery-basics.md)   
 [Referencia del lenguaje de XQuery](../../xquery/xquery-language-reference-sql-server.md)  
  
