---
description: sp_estimate_data_compression_savings (Transact-SQL)
title: sp_estimate_data_compression_savings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_estimate_data_compression_savings_TSQL
- sp_estimate_data_compression_savings
dev_langs:
- TSQL
helpviewer_keywords:
- compression [SQL Server], estimating
- sp_estimate_data_compression_savings
ms.assetid: 6f6c7150-e788-45e0-9d08-d6c2f4a33729
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 93d169d364369e87f1c363297c2a5c73c58e442b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237853"
---
# <a name="sp_estimate_data_compression_savings-transact-sql"></a>sp_estimate_data_compression_savings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el tamaño actual del objeto solicitado y calcula el tamaño del objeto para el estado de compresión solicitado. La compresión se puede evaluar para tablas enteras o partes de tablas. Esto incluye montones, índices clúster, índices no clúster, índices de almacén de columnas, vistas indizadas y particiones de tablas e índices. Los objetos se pueden comprimir mediante la compresión de archivo de fila, página, almacén de columnas o almacén de columnas. Si la tabla, índice o partición ya están comprimidos, puede utilizar este procedimiento para calcular el tamaño de la tabla, del índice o de la partición en caso de que se volviera a comprimir.  
  
> [!NOTE]
> La compresión y el **sp_estimate_data_compression_savings** no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Para calcular el tamaño del objeto, en caso de que se use el valor de compresión solicitado, el procedimiento almacenado prueba el objeto de origen y carga los datos en una tabla e índice equivalentes creados en tempdb. La tabla o índice creados en tempdb se comprimen al valor solicitado y se calcula el ahorro estimado de la compresión.  
  
 Para cambiar el estado de compresión de una tabla, índice o partición, utilice las instrucciones [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) . Para obtener información general sobre la compresión, vea [compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
> [!NOTE]  
> Si se fragmentan los datos existentes, es posible que pueda reducir su tamaño regenerando el índice y sin necesidad de utilizar la compresión. Para los índices, el factor de relleno se aplicará cuando se vuelva a generar el índice. Esto podría aumentar el tamaño del índice.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_estimate_data_compression_savings   
     [ @schema_name = ] 'schema_name'    
   , [ @object_name = ] 'object_name'   
   , [ @index_id = ] index_id   
   , [ @partition_number = ] partition_number   
   , [ @data_compression = ] 'data_compression'   
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @schema_name =] '*schema_name*'  
 Es el nombre del esquema de la base de datos que contiene la tabla o vista indizada. *schema_name* es de **tipo sysname**. Si *schema_name* es null, se usa el esquema predeterminado del usuario actual.  
  
 [ @object_name =] '*object_name*'  
 Es el nombre de la tabla o vista indizada en la que está el índice. *object_name* es **sysname**.  
  
 [ @index_id =] *index_id*  
 Es el identificador del índice. *index_id* es de **tipo int** y puede tener uno de los valores siguientes: el número de identificación de un índice, NULL o 0 si *object_id* es un montón. Para obtener información de todos los índices de una tabla base o vista, especifique NULL. Si especifica NULL, también debe especificar NULL para *partition_number*.  
  
 [ @partition_number =] *partition_number*  
 Es el número de partición en el objeto. *partition_number* es de **tipo int** y puede tener uno de los valores siguientes: el número de partición de un índice o montón, null o 1 para un índice o montón sin particiones.  
  
 Para especificar la partición, también puede especificar la función [$Partition](../../t-sql/functions/partition-transact-sql.md) . Para obtener información sobre todas las particiones del objeto propietario, especifique NULL.  
  
 [ @data_compression =] '*data_compression*'  
 Es el tipo de compresión que se va a evaluar. *data_compression* puede ser uno de los siguientes valores: None, Row, Page, almacén de columnas o COLUMNSTORE_ARCHIVE.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El siguiente conjunto de resultados se devuelve para proporcionar el tamaño actual y estimado de la tabla, índice o partición.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_name|**sysname**|Nombre de la tabla o vista indizada.|  
|schema_name|**sysname**|Esquema de la tabla o vista indizada.|  
|index_id|**int**|Identificador de índice de un índice:<br /><br /> 0 = Montón<br /><br /> 1 = Índice clúster<br /><br /> > 1 = Índice no clúster|  
|partition_number|**int**|Número de partición. Devuelve 1 para una tabla o índice sin particiones.|  
|size_with_current_compression_setting (KB)|**bigint**|Tamaño actual de la tabla, índice o partición solicitados.|  
|size_with_requested_compression_setting (KB)|**bigint**|Tamaño estimado de la tabla, índice o partición que utiliza el valor de compresión solicitado y, si es aplicable, factor de relleno existente, suponiendo que no hay fragmentación.|  
|sample_size_with_current_compression_setting (KB)|**bigint**|Tamaño del ejemplo con la opción de compresión actual. Esto incluye cualquier fragmentación.|  
|sample_size_with_requested_compression_setting (KB)|**bigint**|Tamaño del ejemplo que se crea utilizando el valor de compresión solicitado y, si es aplicable, factor de relleno existente, sin fragmentación.|  
  
## <a name="remarks"></a>Notas  
 Use `sp_estimate_data_compression_savings` para calcular el ahorro que se puede producir al habilitar una tabla o partición para la compresión de archivo de fila, página, almacén de columnas o almacén de columnas. Por ejemplo, si el tamaño medio de una fila se puede reducir un 40 por ciento, potencialmente también se puede reducir el tamaño del objeto en un 40 por ciento. Es posible que no consiga ahorrar espacio, ya que depende del factor de relleno y del tamaño de la fila. Por ejemplo, si tiene una fila de 8.000 bytes de longitud y reduce su tamaño en un 40 por ciento, puede ajustar solo una fila en una página de datos. No se obtiene ningún ahorro.  
  
 Si los resultados de ejecutar `sp_estimate_data_compression_savings` indican que la tabla crecerá, eso quiere decir que muchas filas de la tabla utilizan prácticamente toda la precisión en los tipos de datos y la adición de la mínima sobrecarga necesaria para el formato comprimido es mayor que el ahorro obtenido por la compresión. En este caso excepcional, no habilite la compresión.  
  
 Si una tabla está habilitada para compresión, utilice `sp_estimate_data_compression_savings` para estimar el tamaño medio de la fila si se descomprime la tabla.  
  
 Durante esta operación, se adquiere un bloqueo con intención compartida (IS) en la tabla. Si no se puede obtener un bloqueo (IS), se bloqueará el procedimiento. La tabla se examina bajo el nivel de aislamiento READ COMMITTED.  
  
 Si el valor de compresión solicitado es mismo que el de la compresión actual, el procedimiento almacenado devolverá el tamaño estimado sin la fragmentación de los datos y utilizando el factor de relleno existente.  
  
 Si no existe el identificador de índice o la partición, no se devolverá ningún resultado.  
  
## <a name="permissions"></a>Permisos  
 Debe tener un permiso de `SELECT` sobre la tabla.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Antes de SQL Server 2019, este procedimiento no se aplicaba a los índices de almacén de columnas y, por lo tanto, no aceptó los parámetros de compresión de datos de almacén de columnas y COLUMNSTORE_ARCHIVE.  A partir de SQL Server 2019, los índices de almacén de columnas se pueden usar como un objeto de origen para la estimación y como un tipo de compresión solicitado.

 > [!IMPORTANT]
 > Cuando se habilitan los [metadatos de tempdb optimizados para memoria](../databases/tempdb-database.md#memory-optimized-tempdb-metadata) en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , no se admite la creación de índices de almacén de columnas en tablas temporales. Debido a esta limitación, no se admite sp_estimate_data_compression_savings con los parámetros de compresión de datos de almacén de columnas y COLUMNSTORE_ARCHIVE cuando Memory-Optimized los metadatos de TempDB están habilitados.

## <a name="considerations-for-columnstore-indexes"></a>Consideraciones para los índices de almacén de columnas
 A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , admite la estimación de la `sp_estimate_compression_savings` compresión de archivo de almacén de columnas y de almacén de columnas. A diferencia de la compresión de página y fila, la aplicación de la compresión de almacén de columnas a un objeto requiere la creación de un nuevo índice de almacén de columnas. Por esta razón, al usar las opciones de almacén de columnas y COLUMNSTORE_ARCHIVE de este procedimiento, el tipo del objeto de origen proporcionado al procedimiento determina el tipo de índice de almacén de columnas usado para la estimación del tamaño comprimido. En la tabla siguiente se muestran los objetos de referencia que se utilizan para calcular el ahorro de compresión para cada tipo de objeto de origen cuando el @data_compression parámetro se establece en el almacén de columnas o en el COLUMNSTORE_ARCHIVE.

 |Objeto de origen|Objeto de referencia|
 |-----------------|---------------|
 |Montón|Índice de almacén de columnas agrupado|
 |Índice agrupado|Índice de almacén de columnas agrupado|
 |Índice no clúster|Índice de almacén de columnas no agrupado (incluidas las columnas de clave y todas las columnas incluidas del índice no clúster proporcionado, así como la columna de partición de la tabla, si existe)|
 |índice no clúster de almacén de columnas|Índice de almacén de columnas no agrupado (incluidas las mismas columnas que el índice de almacén de columnas no agrupado proporcionado)|
 |Índice de almacén de columnas agrupado|Índice de almacén de columnas agrupado|

> [!NOTE]  
> Al estimar la compresión de almacén de columnas de un objeto de origen de almacén (índice clúster, índice no clúster o montón), si hay alguna columna en el objeto de origen que tenga un tipo de datos no admitido en un índice de almacén de columnas, sp_estimate_compression_savings producirá un error.

 Del mismo modo, cuando el `@data_compression` parámetro se establece en `NONE` , `ROW` o `PAGE` y el objeto de origen es un índice de almacén de columnas, en la tabla siguiente se describen los objetos de referencia usados.

 |Objeto de origen|Objeto de referencia|
 |-----------------|---------------|
 |Índice de almacén de columnas agrupado|Montón|
 |índice no clúster de almacén de columnas|Índice no clúster (incluidas las columnas contenidas en el índice no clúster de almacén de columnas como columnas de clave y la columna de partición de la tabla, si existe, como una columna incluida)|

> [!NOTE]  
> Al estimar la compresión almacén (NONE, ROW o PAGE) de un objeto de origen de almacén de columnas, asegúrese de que el índice de origen no contenga más de 32 columnas, ya que este es el límite admitido en un índice almacén (Nonclustered).
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se calcula el tamaño de la tabla `Production.WorkOrderRouting` si se comprime mediante la compresión `ROW`.  
  
```sql  
USE AdventureWorks2016;  
GO  
EXEC sp_estimate_data_compression_savings 'Production', 'WorkOrderRouting', NULL, NULL, 'ROW' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Implementación de la compresión Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)  
  
  
