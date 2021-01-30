---
description: sp_table_validation (Transact-SQL)
title: sp_table_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_table_validation_TSQL
- sp_table_validation
helpviewer_keywords:
- sp_table_validation
ms.assetid: 31b25f9b-9b62-496e-a97e-441d5fd6e767
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 255c30ea6b3144e781007bff3ae748c8e5a960ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189261"
---
# <a name="sp_table_validation-transact-sql"></a>sp_table_validation (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el recuento de filas o la suma de comprobación de una tabla o vista indizada, o bien compara el recuento de filas o la suma de comprobación proporcionados con los de la tabla o vista indizada especificada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones y en el suscriptor de la base de datos de suscripciones. *No se admite para publicadores de Oracle*.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_table_validation [ @table = ] 'table'  
    [ , [ @expected_rowcount = ] type_of_check_requested OUTPUT]  
    [ , [ @expected_checksum = ] expected_checksum OUTPUT]  
    [ , [ @rowcount_only = ] rowcount_only ]  
    [ , [ @owner = ] 'owner' ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @table_name = ] table_name ]  
    [ , [ @column_list = ] 'column_list' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table = ] 'table'` Es el nombre de la tabla. *TABLE* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @expected_rowcount = ] expected_rowcountOUTPUT` Especifica si se debe devolver el número esperado de filas de la tabla. *expected_rowcount* es de **tipo int** y su valor predeterminado es NULL. Si es NULL, el recuento de filas real se devuelve como parámetro de salida. Si se proporciona un valor, dicho valor se compara con el recuento de filas real para identificar posibles diferencias.  
  
`[ @expected_checksum = ] expected_checksumOUTPUT` Especifica si se debe devolver la suma de comprobación esperada para la tabla. *expected_checksum* es **numérica** y su valor predeterminado es NULL. Si es NULL, la suma de comprobación real se devuelve como parámetro de salida. Si se proporciona un valor, dicho valor se compara con la suma de comprobación real para identificar posibles diferencias.  
  
`[ @rowcount_only = ] type_of_check_requested` Especifica qué tipo de suma de comprobación o recuento de filas se va a realizar. *type_of_check_requested* es de **smallint** y su valor predeterminado es **1**.  
  
 Si es **0**, realice un recuento de filas y una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suma de comprobación compatible con 7,0.  
  
 Si es **1**, realizar solo una comprobación del recuento de filas.  
  
 Si es **2**, realice un recuento de filas y una suma de comprobación binaria.  
  
`[ @owner = ] 'owner'` Es el nombre del propietario de la tabla. *Owner* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @full_or_fast = ] full_or_fast` Es el método utilizado para calcular el recuento de filas. *full_or_fast* es de **tinyint**, su valor predeterminado es **2** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Realiza un recuento completo mediante COUNT(*).|  
|**1**|Realiza un recuento rápido de **sysindexes. Rows**. El recuento de filas en **sysindexes** es mucho más rápido que contar las filas de la tabla real. Sin embargo, dado que **sysindexes** se actualiza de forma diferida, es posible que el recuento de filas no sea preciso.|  
|**2** (predeterminado)|Realiza un recuento rápido condicional intentando primero el método rápido. Si el método rápido muestra diferencias, se utiliza el método completo. Si *expected_rowcount* es NULL y se usa el procedimiento almacenado para obtener el valor, siempre se usa un recuento completo (*).|  
  
`[ @shutdown_agent = ] shutdown_agent` Si el Agente de distribución se está ejecutando **sp_table_validation**, especifica si el agente de distribución debe cerrarse inmediatamente al completarse la validación. *shutdown_agent* es de **bit** y su valor predeterminado es **0**. Si es **0**, el agente de replicación no se cierra. Si es **1**, se genera el error 20578 y se indica al agente de replicación que se cierre. Este parámetro se omite cuando el usuario ejecuta **sp_table_validation** directamente.  
  
`[ @table_name = ] table_name` Es el nombre de la tabla de la vista utilizada para los mensajes de salida. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es **\@ TABLE**.  
  
`[ @column_list = ] 'column_list'` Es la lista de columnas que se deben utilizar en la función de suma de comprobación. *column_list* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. Habilita la validación de artículos de mezcla para especificar una lista de columnas que excluya las columnas calculadas y de marca de tiempo.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Si realiza una validación de suma de comprobación y la suma de comprobación esperada es igual a la suma de comprobación de la tabla, **sp_table_validation** devuelve un mensaje que indica que la tabla ha pasado la validación de la suma de comprobación. De lo contrario, devuelve un mensaje que indica que la tabla puede no estar sincronizada e informa de la diferencia entre el número de filas real y el esperado.  
  
 Si se realiza una validación de RowCount y el número esperado de filas es igual al número de la tabla, **sp_table_validation** devuelve un mensaje que indica que la tabla ha pasado la validación de RowCount. De lo contrario, devuelve un mensaje que indica que la tabla puede no estar sincronizada e informa de la diferencia entre el número de filas real y el esperado.  
  
## <a name="remarks"></a>Observaciones  
 **sp_table_validation** se utiliza en todos los tipos de replicación. los publicadores de Oracle no admiten **sp_table_validation** .  
  
 La suma de comprobación calcula una prueba de redundancia cíclica (CRC, Cyclic Redundancy Check) de 32 bits sobre la imagen completa de la fila en la página. No comprueba las columnas de forma selectiva y no puede operar sobre vistas ni particiones verticales de la tabla. Además, la suma de comprobación omite el contenido de las columnas de **texto** e **imagen** (por diseño).  
  
 Al realizar una suma de comprobación, la estructura de la tabla debe ser idéntica en los dos servidores; es decir, las tablas deben tener las mismas columnas y en el mismo orden, el mismo tipo de datos y la misma longitud, y las mismas condiciones NULL o NOT NULL. Por ejemplo, si el publicador ejecutó CREATE TABLE y, a continuación, ALTER TABLE para agregar columnas, pero el script aplicado en el suscriptor es simplemente CREATE TABLE, la estructura NO será la misma. Si no está seguro de que la estructura de las dos tablas es idéntica, mire [syscolumns](../../relational-databases/system-compatibility-views/sys-syscolumns-transact-sql.md) y confirme que el desplazamiento de cada tabla es el mismo.  
  
 Es probable que los valores de punto flotante generen diferencias de suma de comprobación si se utilizó **BCP** de modo de carácter, que es el caso si la publicación tiene suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dichas diferencias se deben a pequeñas e inevitables diferencias de precisión en la conversión desde y hacia el modo de carácter.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar **sp_table_validation**, debe tener permisos SELECT en la tabla que se va a validar.  
  
## <a name="see-also"></a>Consulte también  
 [SUMA de comprobación &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)   
 [sp_article_validation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_publication_validation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
