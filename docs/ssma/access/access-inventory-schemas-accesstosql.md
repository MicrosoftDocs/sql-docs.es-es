---
description: Esquemas de inventario de acceso (AccessToSQL)
title: Esquemas de inventario de acceso (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- columns table
- databases table
- exported metadata
- exporting, schemas of exported metadata
- foreign keys table
- forms table
- indexes, table
- inventory schemas
- macros table
- modules table
- queries table
- reference, inventory schemas
- reports table
- schemas of exported metadata
- schemas, exported metadata
- SSMA_Access_InventoryColumns
- SSMA_Access_InventoryDatabases
- SSMA_Access_InventoryForeignKeys
- SSMA_Access_InventoryForms
- SSMA_Access_InventoryIndexes
- SSMA_Access_InventoryMacros
- SSMA_Access_InventoryModules
- SSMA_Access_InventoryQueries
- SSMA_Access_InventoryReports
- SSMA_Access_InventoryTables
- tables, inventory
ms.assetid: fdd3cff2-4d62-4395-8acf-71ea8f17f524
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bddb9d2329c20203aca602e7aec37f235c4b6850
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077696"
---
# <a name="access-inventory-schemas-accesstosql"></a>Esquemas de inventario de acceso (AccessToSQL)
En las secciones siguientes se describen las tablas que se crean al exportar esquemas de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="databases"></a>Bases de datos  
Los metadatos de la base de datos se exportan a la tabla **SSMA_Access_InventoryDatabases** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|GUID que identifica de forma única cada base de datos. Esta columna es también la clave principal de la tabla.|  
|**DatabaseName**|**nvarchar(4000)**|Nombre de la base de datos de Access.|  
|**ExportTime**|**datetime**|Fecha y hora de creación de estos metadatos mediante SSMA.|  
|**FilePath**|**nvarchar(4000)**|Ruta de acceso completa y nombre de archivo de la base de datos de Access.|  
|**FileSize**|**bigint**|Tamaño de la base de datos de Access en KB.|  
|**FileOwner**|**nvarchar(4000)**|Cuenta de Windows que se especifica como propietario de la base de datos de Access.|  
|**DateCreated**|**datetime**|Fecha y hora en que se creó la base de datos de Access.|  
|**DateModified**|**datetime**|Fecha y hora en que se modificó por última vez la base de datos de Access.|  
|**TablesCount**|**int**|El número de tablas en la base de datos de Access.|  
|**QueriesCount**|**int**|El número de consultas en la base de datos de Access.|  
|**FormsCount**|**int**|El número de formularios en la base de datos de Access.|  
|**ModulesCount**|**int**|El número de módulos en la base de datos de Access.|  
|**ReportsCount**|**int**|El número de informes en la base de datos de Access.|  
|**MacrosCount**|**int**|El número de macros en la base de datos de Access.|  
|**AccessVersion**|**nvarchar(4000)**|La versión de Access de la base de datos.|  
|**Intercalación**|**nvarchar(4000)**|La intercalación de la base de datos de Access. Las intercalaciones determinan cómo una base de datos ordena y compara las cadenas.|  
|**JetVersion**|**nvarchar(4000)**|Versión del motor de base de datos Jet. Las bases de datos de Access usan el motor de base de datos Jet subyacente.|  
|**IsUpdatable**|**bit**|Indica si la base de datos se puede actualizar. Si el valor es 1, la base de datos es actualizable. Si el valor es 0, la base de datos es de solo lectura.|  
|**QueryTimeout**|**int**|Valor de tiempo de espera de la consulta ODBC configurado para la base de datos, en segundos. El valor predeterminado es 60 segundos.|  
  
## <a name="tables"></a>Tablas  
Los metadatos de la tabla se exportan a la tabla **SSMA_Access_InventoryTables** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene esta tabla.|  
|**TableId**|**uniqueidentifier**|GUID que identifica de forma única la tabla. Esta columna es también la clave principal de la tabla.|  
|**TableName**|**nvarchar(4000)**|Nombre de la tabla.|  
|**RowsCount**|**int**|Número de filas de la tabla.|  
|**(**|**nvarchar(4000)**|La regla que define la entrada válida para la tabla. Si no existe ninguna regla de validación, el campo contendrá una cadena vacía.|  
|**LinkedTable**|**nvarchar(4000)**|Otra tabla, si existe, que está vinculada a la tabla. La vinculación de tablas permite adiciones, eliminaciones y actualizaciones en la otra tabla mediante el uso de esta tabla.|  
|**ExternalSource**|**nvarchar(4000)**|Origen de datos, si existe, que está asociado a la tabla. Si una tabla está vinculada, tiene un origen de datos externo especificado en este campo.|  
  
## <a name="columns"></a>Columnas  
Los metadatos de columna se exportan a la tabla **SSMA_Access_InventoryColumns** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene esta columna.|  
|**TableId**|**uniqueidentifier**|Identifica la tabla que contiene esta columna.|  
|**ColumnId**|**int**|Entero que incrementa el valor de la columna. **ColumnId** es la clave principal de la tabla.|  
|**ColumnName**|**nvarchar(4000)**|El nombre de la columna.|  
|**IsNullable**|**bit**|Especifica si la columna puede contener valores NULL. Si el valor es 1, la columna puede contener valores NULL. Si el valor es 0, la columna no puede contener valores NULL. Tenga en cuenta que la regla de validación también se puede usar para evitar valores NULL.|  
|**DataType**|**nvarchar(4000)**|El tipo de datos de acceso de la columna, como **texto** o **largo**.|  
|**IsAutoIncrement**|**bit**|Especifica si la columna incrementa automáticamente los valores enteros. Si el valor es 1, los enteros se incrementan automáticamente.|  
|**Ordinal**|**smallint**|El orden de la columna de la tabla, empezando por cero.|  
|**DefaultValue**|**nvarchar(4000)**|El valor predeterminado de la columna.|  
|**(**|**nvarchar(4000)**|La regla que se usa para validar los datos agregados o actualizados en la columna.|  
  
## <a name="indexes"></a>Índices  
Los metadatos de índice se exportan a la tabla **SSMA_Access_InventoryIndexes** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene este índice.|  
|**TableId**|**uniqueidentifier**|Identifica la tabla que contiene este índice.|  
|**IndexId**|**int**|Entero de incremento que identifica el índice. Esta columna es la clave principal de la tabla.|  
|**IndexName**|**nvarchar(4000)**|El nombre del índice.|  
|**ColumnsIncluded**|**nvarchar(4000)**|Muestra las columnas incluidas en el índice. Los nombres de columna se separan mediante un punto y coma.|  
|**IsUnique**|**bit**|Especifica si cada elemento del índice debe ser único. En un índice de varias columnas, la combinación de valores debe ser única. Si el valor es 1, el índice aplica valores únicos.|  
|**IsPK**|**bit**|Especifica si el índice se creó automáticamente como parte de la definición de la clave principal.|  
|**IsClustered**|**bit**|Especifica si el índice está agrupado. Un índice clúster reordena el almacenamiento físico de los datos. Una tabla solo puede tener un índice clúster.|  
  
## <a name="foreign-keys"></a>Claves externas  
Los metadatos de clave externa se exportan a la tabla **SSMA_Access_InventoryForeignKeys** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene esta clave externa.|  
|**TableId**|**uniqueidentifier**|Identifica la tabla que contiene esta clave externa.|  
|**ForeignKeyId**|**int**|Entero que incrementa el valor de tipo que identifica la clave externa. Esta columna es la clave principal de la tabla.|  
|**ForeignKeyName**|**nvarchar(4000)**|El nombre del índice.|  
|**ReferencedTableId**|**uniqueidentifier**|Identifica la tabla que contiene las columnas de origen.|  
|**SourceColumns**|**nvarchar(4000)**|Muestra la columna o columnas de clave externa.|  
|**ReferencedColumns**|**nvarchar(4000)**|Muestra la columna o columnas de clave principal a las que hace referencia la clave externa.|  
|**IsCascadeForUpdate**|**bit**|Especifica que, si se actualiza el valor de la clave principal, también se actualizan todas las filas que hacen referencia a ese valor de clave.|  
|**IsCascadeForDelete**|**bit**|Especifica que, si se elimina el valor de la clave principal, también se eliminan todas las filas que hacen referencia a ese valor de clave.|  
|**IsEnforced**|**bit**|Especifica que se aplica la restricción FOREIGN KEY.|  
  
## <a name="queries"></a>Consultas  
Los metadatos de la consulta se exportan a la tabla **SSMA_Access_InventoryQueries** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene esta consulta.|  
|**QueryId**|**int**|Un entero que incrementa la consulta. Esta columna es la clave principal de la tabla.|  
|**Nombredeconsulta**|**nvarchar(4000)**|Nombre de la consulta.|  
|**QueryText**|**nvarchar(4000)**|Código de consulta SQL, como una instrucción SELECT.|  
|**IsUpdateable**|**bit**|Especifica si la consulta es actualizable o de solo lectura.|  
|**QueryType**|**nvarchar(4000)**|Especifica el tipo de consulta, como **Select** o **SetOperation**.|  
|**ExternalSource**|**nvarchar(4000)**|Si la consulta hace referencia a un origen de datos externo, se trata de la cadena de conexión utilizada por la consulta.|  
  
## <a name="forms"></a>Formularios  
Los metadatos del formulario se exportan a la tabla **SSMA_Access_InventoryForms** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene este formulario.|  
|**FormId**|**int**|Un entero que se incrementa y que identifica el formulario. Esta columna es la clave principal de la tabla.|  
|**FormName**|**nvarchar(4000)**|El nombre del formulario.|  
  
## <a name="macros"></a>Macros  
Los metadatos de macros se exportan a la tabla **SSMA_Access_InventoryMacros** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene la macro.|  
|**MacroId**|**int**|Entero que incrementa el valor de la macro. Esta columna es la clave principal de la tabla.|  
|**Nombredelamacro**|**nvarchar(4000)**|Nombre de la macro.|  
  
## <a name="reports"></a>Informes  
Los metadatos de informe se exportan a la tabla **SSMA_Access_InventoryReports** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene el informe.|  
|**ReportId**|**int**|Entero que aumenta el número de identificación del informe. Esta columna es la clave principal de la tabla.|  
|**ReportName**|**nvarchar(4000)**|Nombre del informe.|  
  
## <a name="modules"></a>Módulos  
Los metadatos del módulo se exportan a la tabla **SSMA_Access_InventoryModules** . Esta tabla contiene las columnas siguientes:  
  
|Nombre de columna|Tipo de datos|Descripción|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica la base de datos que contiene el módulo.|  
|**ModuleId**|**int**|Un entero que aumenta el número que identifica el módulo. Esta columna es la clave principal de la tabla.|  
|**ModuleName**|**nvarchar(4000)**|El nombre del módulo.|  
  
## <a name="see-also"></a>Consulte también  
[Exportación de un inventario de Access](exporting-an-access-inventory-accesstosql.md)  
  
