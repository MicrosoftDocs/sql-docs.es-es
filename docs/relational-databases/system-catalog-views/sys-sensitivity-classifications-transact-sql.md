---
description: sys.sensitivity_classifications (Transact-SQL)
title: sys.sensitivity_classifications (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
ms.custom: ''
ms.author: mibar
author: barmichal
f1_keywords:
- 'sys.sensitivity_classifications '
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sensitivity_classifications statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- rank
monikerRange: '>= sql-server-ver15 || = azuresqldb-current || = azure-sqldw-latest'
ms.openlocfilehash: 1945926624b6f0a9c996c0be624e32b49e07345b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343135"
---
# <a name="syssensitivity_classifications-transact-sql"></a>sys.sensitivity_classifications (Transact-SQL)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Devuelve una fila por cada elemento clasificado en la base de datos.

|Nombre de la columna|Tipo de datos|Descripción|
|-----------------|---------------|-----------------|  
|**class**|**int**|Identifica la clase del elemento en el que existe la clasificación. Siempre tendrá el valor 1 (que representa una columna).|  
|**class_desc**|**VARCHAR (16)**|Descripción de la clase del elemento en el que existe la clasificación. siempre tendrá el valor *OBJECT_OR_COLUMN*|  
|**major_id**|**int**|Representa el identificador de la tabla que contiene la columna clasificada, que corresponde a sys. All _objects. object_id|  
|**minor_id**|**int**|Representa el identificador de la columna en la que existe la clasificación, correspondiente a sys. All _columns. column_id|   
|**label**|**sysname**|La etiqueta (inteligible) asignada para la clasificación de confidencialidad|  
|**label_id**|**sysname**|Identificador asociado a la etiqueta, que se puede usar en un sistema de protección de la información, como Azure Information Protection (AIP).|  
|**information_type**|**sysname**|El tipo de información (legible) asignado para la clasificación de confidencialidad|  
|**information_type_id**|**sysname**|IDENTIFICADOR asociado con el tipo de información, que se puede usar en un sistema de protección de la información, como Azure Information Protection (AIP).|  
|**criterios**|**int**|Un valor numérico del rango: <br><br>0 para ninguno<br>10 para LOW<br>20 para medio<br>30 para alta<br>40 para crítico| 
|**rank_desc**|**sysname**|Representación textual del rango:  <br><br>NINGUNO, BAJO, MEDIO, ALTO, CRÍTICO|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Comentarios  

- Esta vista proporciona visibilidad en el estado de clasificación de la base de datos. Se puede usar para administrar las clasificaciones de la base de datos, así como para generar informes.
- Actualmente solo se admite la clasificación de columnas de base de datos.
 
## <a name="examples"></a>Ejemplos

### <a name="a-listing-all-classified-columns-and-their-corresponding-classification"></a>A. Enumerar todas las columnas clasificadas y su clasificación correspondiente

En el ejemplo siguiente se devuelve una tabla que muestra el nombre de la tabla, el nombre de la columna, la etiqueta, el identificador de etiqueta, el tipo de información, el ID. de tipo de información, el rango y la descripción de rango de cada columna clasificada de la base de datos

> [!NOTE]
> Label es una palabra clave de Azure Synapse Analytics.

```sql
SELECT
    SCHEMA_NAME(sys.all_objects.schema_id) as SchemaName,
    sys.all_objects.name AS [TableName], sys.all_columns.name As [ColumnName],
    [Label], [Label_ID], [Information_Type], [Information_Type_ID], [Rank], [Rank_Desc]
FROM
          sys.sensitivity_classifications
left join sys.all_objects on sys.sensitivity_classifications.major_id = sys.all_objects.object_id
left join sys.all_columns on sys.sensitivity_classifications.major_id = sys.all_columns.object_id
                         and sys.sensitivity_classifications.minor_id = sys.all_columns.column_id
```

## <a name="permissions"></a>Permisos  
 Requiere el permiso **View any Sensitivity Classification** . 
 
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="see-also"></a>Consulte también  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

[Clasificación y detección de datos de Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview)