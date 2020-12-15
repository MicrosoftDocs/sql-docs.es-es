---
description: sp_datatype_info_90 (Azure Synapse Analytics)
title: sp_datatype_info_90 (Azure Synapse Analytics)
ms.custom: ''
ms.date: 03/13/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1d043964-dc6e-4c3e-ab61-bc444d5e25ae
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 8a859571ef9f4682c4c8556038247dc21d47eb50
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474616"
---
# <a name="sp_datatype_info_90-azure-synapse-analytics"></a>sp_datatype_info_90 (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Devuelve información acerca de los tipos de datos que admite el entorno actual.  
  
 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_datatype_info_90 [ [ @data_type = ] data_type ]   
     [ , [ @ODBCVer = ] odbc_version ]   
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumentos  
`[ @data_type = ] data_type` Es el número de código del tipo de datos especificado. Para obtener una lista de todos los tipos de datos, omita este parámetro. *data_type* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @ODBCVer = ] odbc_version` Es la versión de ODBC que se utiliza. *odbc_version* es de **tinyint** y su valor predeterminado es 2.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|TYPE_NAME|**sysname**|Tipo de datos dependiente del DBMS.|  
|DATA_TYPE|**smallint**|Código para el tipo de ODBC al que se asignan todas las columnas de este tipo.|  
|PRECISION|**int**|Precisión máxima del tipo de datos en el origen de datos. Se devuelve NULL para los tipos de datos a los que no se puede aplicar un parámetro de precisión. El valor devuelto para la columna PRECISION está expresado en base 10.|  
|LITERAL_PREFIX|**VARCHAR (** 32 **)**|Carácter o caracteres utilizados antes de una constante. Por ejemplo, una comilla simple (**'**) para tipos de carácter y 0x para binario.|  
|LITERAL_SUFFIX|**VARCHAR (** 32 **)**|Carácter o caracteres utilizados para terminar una constante. Por ejemplo, una comilla simple (**'**) para tipos de caracteres y sin comillas para binario.|  
|CREATE_PARAMS|**VARCHAR (** 32 **)**|Descripción de los parámetros de creación para este tipo de datos. Por ejemplo, **decimal** es "Precision, Scale", **float** es NULL y **VARCHAR** es "max_length".|  
|NULLABLE|**smallint**|Especifica la nulabilidad.<br /><br /> 1 = Permite valores NULL.<br /><br /> 0 = No permite valores NULL.|  
|CASE_SENSITIVE|**smallint**|Especifica si deben distinguirse mayúsculas y minúsculas.<br /><br /> 1 = Todas las columnas de este tipo distinguen mayúsculas y minúsculas (para intercalaciones).<br /><br /> 0 = Las columnas de este tipo no distinguen mayúsculas y minúsculas.|  
|BUSCABLE|**smallint**|Especifica la capacidad de búsqueda del tipo de columna:<br /><br /> 1 = No se puede buscar.<br /><br /> 2 = Se puede buscar con LIKE.<br /><br /> 3 = Se puede buscar con WHERE.<br /><br /> 4 = Se puede buscar con WHERE o LIKE.|  
|UNSIGNED_ATTRIBUTE|**smallint**|Especifica el signo del tipo de datos.<br /><br /> 1 = Tipo de datos sin signo.<br /><br /> 0 = Tipo de datos con signo.|  
|MONEY|**smallint**|Especifica el tipo de datos **Money** .<br /><br /> 1 = tipo de datos **Money** .<br /><br /> 0 = no es un tipo de datos **Money** .|  
|AUTO_INCREMENT|**smallint**|Especifica el incremento automático.<br /><br /> 1 = Incremento automático.<br /><br /> 0 = Sin incremento automático.<br /><br /> NULL = Atributo no aplicable.<br /><br /> Una aplicación puede insertar valores en una columna que tenga este atributo, pero no puede actualizar los valores de la columna. A excepción del tipo de datos **bit** , AUTO_INCREMENT solo es válido para los tipos de datos que pertenecen a las categorías de tipos de datos numéricos exacto y numéricos aproximados.|  
|LOCAL_TYPE_NAME|**sysname**|Versión traducida del nombre del tipo de datos, dependiente del origen de datos. Por ejemplo, DECIMAL es DECIMALE en francés. Se devuelve NULL si el origen de datos no admite un nombre traducido.|  
|MINIMUM_SCALE|**smallint**|Escala mínima del tipo de datos en el origen de datos. Si un tipo de datos tiene una escala fija, las columnas MINIMUM_SCALE y MAXIMUM_SCALE contienen este valor. Se devuelve NULL cuando no se puede aplicar la escala.|  
|MAXIMUM_SCALE|**smallint**|Escala máxima del tipo de datos en el origen de datos. Si la escala máxima no está definida por separado en el origen de datos, sino que se define igual que la precisión máxima, esta columna contendrá el mismo valor que la columna PRECISION.|  
|SQL_DATA_TYPE|**smallint**|Valor del tipo de datos SQL tal como aparece en el campo TYPE del descriptor. Esta columna es igual que la columna DATA_TYPE, excepto los tipos de datos **DateTime** y **Interval** ANSI. Este campo siempre devuelve un valor.|  
|SQL_DATETIME_SUB|**smallint**|el subcódigo de **intervalo** **DateTime** o ANSI si el valor de SQL_DATA_TYPE es SQL_DATETIME o SQL_INTERVAL. En el caso de los tipos de datos distintos de **DateTime** e **Interval** ANSI, este campo es NULL.|  
|NUM_PREC_RADIX|**int**|Número de bits o dígitos para calcular el número máximo que puede tener una columna. Si el tipo de datos es numérico aproximado, esta columna contendrá el valor 2 para indicar varios bits. Para tipos numéricos exactos, esta columna contiene el valor 10 que indica varias cifras decimales. De lo contrario, esta columna es NULL. Mediante la combinación de la precisión con la base, la aplicación puede calcular el número máximo que puede tener la columna.|  
|INTERVAL_PRECISION|**smallint**|Valor de precisión inicial del intervalo si *data_type* es **Interval**; de lo contrario, NULL.|  
|USERTYPE|**smallint**|valor **usertype** de la tabla systypes.|  
  
## <a name="remarks"></a>Comentarios  
 sp_datatype_info es equivalente a SQLGetTypeInfo en ODBC. Los resultados devueltos se ordenan por DATA_TYPE y, a continuación, por la proximidad de la asignación de los tipos de datos a los tipos de datos de ODBC SQL correspondientes.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se recupera información de los tipos de datos **sysname** y **nvarchar** especificando el valor de *data_type* de `-9` .  
  
```sql  
USE master;  
GO  
EXEC sp_datatype_info_90 -9;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de Azure Synapse Analytics](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  

