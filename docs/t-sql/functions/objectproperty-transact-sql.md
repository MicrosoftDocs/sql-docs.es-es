---
description: OBJECTPROPERTY (Transact-SQL)
title: OBJECTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECTPROPERTY
- OBJECTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTY function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: 27569888-f8b5-4cec-a79f-6ea6d692b4ae
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c268fc5650495abec05abafee219c045189ba17b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237420"
---
# <a name="objectproperty-transact-sql"></a>OBJECTPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información acerca de los objetos de ámbito de esquema de la base de datos actual. Para obtener una lista de los objetos de ámbito de esquema, vea [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). Esta función no se puede utilizar para objetos que no pertenezcan al ámbito de esquema, como notificaciones de eventos y desencadenadores DDL (lenguaje de definición de datos).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
OBJECTPROPERTY ( id , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *id*  
 Es una expresión que representa el identificador del objeto en la base de datos actual. *id* es de tipo **int** y se considera que se trata de un objeto de ámbito de esquema en el contexto de la base de datos actual.  
  
 *property*  
 Es una expresión que representa la información que se devuelve para el objeto especificado en *id*. *property* puede tener uno de los valores siguientes.  
  
> [!NOTE]  
>  A menos que se especifique lo contrario, se devuelve NULL si *property* no es un nombre de propiedad válido, *id* no es un identificador de objeto válido, *id* es un tipo de objeto incompatible con el valor *property* especificado o el autor de la llamada no tiene permiso para ver los metadatos del objeto.  
  
|Nombre de propiedad|Tipo de objeto|Descripción y valores devueltos|  
|-------------------|-----------------|-------------------------------------|  
|CnstIsClustKey|Restricción|Restricción PRIMARY KEY con un índice clúster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsColumn|Restricción|Restricción CHECK, DEFAULT o FOREIGN KEY en una única columna.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDeleteCascade|Restricción|Restricción FOREIGN KEY con la opción ON DELETE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDisabled|Restricción|Restricción deshabilitada.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNonclustKey|Restricción|Restricción PRIMARY KEY o UNIQUE con un índice no clúster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotRepl|Restricción|La restricción se define utilizando las palabras clave NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotTrusted|Restricción|La restricción se ha habilitado sin comprobar las filas existentes, por lo que es posible que no se mantenga para todas las filas.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsUpdateCascade|Restricción|Restricción FOREIGN KEY con la opción ON UPDATE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAfterTrigger|Desencadenador|Desencadenador AFTER.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAnsiNullsOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Valor de ANSI_NULLS en el momento de su creación.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsDeleteTrigger|Desencadenador|Desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstDeleteTrigger|Desencadenador|Primer desencadenador que se activa cuando se ejecuta DELETE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstInsertTrigger|Desencadenador|Primer desencadenador que se activa cuando se ejecuta INSERT en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstUpdateTrigger|Desencadenador|Primer desencadenador que se activa cuando se ejecuta UPDATE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsertTrigger|Desencadenador|Desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsteadOfTrigger|Desencadenador|Desencadenador INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastDeleteTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta DELETE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastInsertTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta INSERT en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastUpdateTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta UPDATE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsQuotedIdentOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Valor de QUOTED_IDENTIFIER en el momento de su creación.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsStartup|Procedimiento|Procedimiento de inicio.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerDisabled|Desencadenador|Desencadenador deshabilitado.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerNotForRepl|Desencadenador|Desencadenador definido como NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsUpdateTrigger|Desencadenador|Desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsWithNativeCompilation|Procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)]|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El procedimiento se compila de forma nativa.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasAfterTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador AFTER.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasDeleteTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsertTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsteadOfTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasUpdateTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsAnsiNullsOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Especifica que el valor de la opción ANSI NULLS de la tabla es ON. Esto significa que todas las comparaciones con un valor NULL se evalúan como UNKNOWN. Este valor se aplica a todas las expresiones de la definición de tabla, incluidas las columnas calculadas y las restricciones, mientras la tabla exista.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsCheckCnst|Cualquier objeto en el ámbito de esquema|Restricción CHECK.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsConstraint|Cualquier objeto en el ámbito de esquema|Es una restricción CHECK, DEFAULT o FOREIGN KEY de columna única en una columna o una tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefault|Cualquier objeto en el ámbito de esquema|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Valor predeterminado enlazado.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefaultCnst|Cualquier objeto en el ámbito de esquema|Restricción DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDeterministic|Función, vista|Propiedad de determinismo de la función o vista.<br /><br /> 1 = Determinista<br /><br /> 0 = No determinista|  
|IsEncrypted|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Indica que el texto original de la instrucción del módulo se ha convertido a un formato confuso. La salida de la protección no es directamente visible en ninguna de las vistas de catálogo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Los usuarios sin acceso a las tablas del sistema o a los archivos de base de datos no pueden recuperar el texto ofuscado. En cambio, está disponible para los usuarios que puedan obtener acceso a las tablas del sistema a través del [puerto DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) o directamente a los archivos de base de datos. Además, los usuarios que pueden adjuntar un depurador al proceso del servidor pueden recuperar el procedimiento original de la memoria en tiempo de ejecución.<br /><br /> 1 = Cifrada<br /><br /> 0 = No cifrado<br /><br /> Tipo de datos base: **int**|  
|IsExecuted|Cualquier objeto en el ámbito de esquema|El objeto se puede ejecutar (vista, procedimiento, función o desencadenador).<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsExtendedProc|Cualquier objeto en el ámbito de esquema|Procedimiento extendido.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsForeignKey|Cualquier objeto en el ámbito de esquema|Restricción FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexed|Tabla, vista|Tabla o vista que tiene un índice.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexable|Tabla, vista|Tabla o vista en la que es posible crear un índice.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsInlineFunction|Función|Función insertada.<br /><br /> 1 = Función insertada<br /><br /> 0 = Función no insertada|  
|IsMSShipped|Cualquier objeto en el ámbito de esquema|Objeto creado durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsPrimaryKey|Cualquier objeto en el ámbito de esquema|Restricción PRIMARY KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> NULL = No es una función o el identificador de objeto no es válido.|  
|IsProcedure|Cualquier objeto en el ámbito de esquema|Procedimiento.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsQuotedIdentOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista, restricción CHECK, definición DEFAULT|Especifica que el valor del identificador entre comillas para el objeto es ON. Esto significa que los identificadores están delimitados por comillas dobles en todas las expresiones que participan en la definición del objeto.<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|IsQueue|Cualquier objeto en el ámbito de esquema|Cola de Service Broker<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsReplProc|Cualquier objeto en el ámbito de esquema|Procedimiento de replicación.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsRule|Cualquier objeto en el ámbito de esquema|Regla enlazada.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsScalarFunction|Función|Función escalar.<br /><br /> 1 = Función escalar<br /><br /> 0 = Función no escalar|  
|IsSchemaBound|Función, vista|Función o vista enlazada al esquema creada mediante SCHEMABINDING.<br /><br /> 1 = Enlazada al esquema<br /><br /> 0 = No enlazada al esquema.|  
|IsSystemTable|Tabla|Tabla del sistema.<br /><br /> 1 = True<br /><br /> 0 = False| 
|IsSystemVerified|Object|SQL Server puede comprobar las propiedades de determinismo y precisión del objeto.<br /><br /> 1 = True<br /><br /> 0 = False| 
|IsTable|Tabla|Tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsTableFunction|Función|Función con valores de tabla.<br /><br /> 1 = Función con valores de tabla<br /><br /> 0 = Función con valores no de tabla.|  
|IsTrigger|Cualquier objeto en el ámbito de esquema|Desencadenador.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUniqueCnst|Cualquier objeto en el ámbito de esquema|Restricción UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUserTable|Tabla|Tabla definida por el usuario.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsView|Ver|Vista.<br /><br /> 1 = True<br /><br /> 0 = False|  
|OwnerId|Cualquier objeto en el ámbito de esquema|Propietario del objeto.<br /><br /> **Nota:** El propietario del esquema no es necesariamente el propietario del objeto. Por ejemplo, los objetos secundarios (aquellos en los que *parent_object_id* no es NULL) siempre devolverán el mismo identificador de propietario que el primario.<br /><br /> Distinto de NULL = Identificador de usuario de base de datos que corresponde al propietario del objeto.|  
|SchemaId|Cualquier objeto en el ámbito de esquema| Id. de esquema del esquema al que pertenece el objeto.| 
|TableDeleteTrigger|Tabla|La tabla tiene un desencadenador DELETE.<br /><br /> >1 = Identificador del primer desencadenador con el tipo especificado.|  
|TableDeleteTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores DELETE.<br /><br /> >0 = Número de desencadenadores DELETE.|  
|TableFullTextMergeStatus|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Indica si una tabla que tiene un índice de texto completo se está combinando actualmente.<br /><br /> 0 = La tabla no tiene un índice de texto completo o el índice de texto completo no se está combinando.<br /><br /> 1 = El índice de texto completo se está combinando.|  
|TableFullTextBackgroundUpdateIndexOn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene habilitada la opción de actualización de índices de texto completo en segundo plano (seguimiento de cambios automáticos).<br /><br /> 1 = TRUE<br /><br /> 0 = False|  
|TableFulltextCatalogId|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Identificador del catálogo de texto completo en el que residen los datos de índice de texto completo para la tabla.<br /><br /> Distinto de cero = Identificador del catálogo de texto completo, asociado al índice único que identifica las filas en una tabla indizada de texto completo.<br /><br /> 0 = La tabla no tiene un índice de texto completo.|  
|TableFulltextChangeTrackingOn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene habilitado el seguimiento de cambios de texto completo.<br /><br /> 1 = TRUE<br /><br /> 0 = False|  
|TableFulltextDocsProcessed|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de filas procesadas desde el comienzo de la indización de texto completo. En una tabla que se indiza para búsquedas en texto completo, todas las columnas de una fila se consideran como parte de un documento que se va a indizar.<br /><br /> 0 = No se ha completado ningún rastreo activo ni ninguna indización de texto completo.<br /><br /> > 0 = Uno de los siguientes (A o B): A) El número de documentos procesados por operaciones de inserción o actualización desde el inicio del rellenado de seguimiento de cambios completo, incremental o manual. B) El número de filas procesadas por operaciones de inserción o actualización desde que se habilitó el seguimiento de cambios con el rellenado del índice de actualización en segundo plano, la modificación del esquema de índice de texto completo, la regeneración del catálogo de texto completo o el reinicio de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], etc.<br /><br /> NULL = La tabla no tiene un índice de texto completo.<br /><br /> Esta propiedad no supervisa ni cuenta las filas eliminadas.|  
|TableFulltextFailCount|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de filas no indizadas por Búsqueda de texto completo.<br /><br /> 0 = El rellenado se ha completado.<br /><br /> > 0 = Uno de los siguientes (A o B): A) El número de documentos que no se han indizado desde el inicio del llenado de seguimiento de cambios de actualización completa, incremental o manual. B) En el seguimiento de cambios con actualización de índices en segundo plano, el número de filas no indizadas desde el comienzo del rellenado o desde su reinicio. Esto puede deberse a un cambio del esquema, a la regeneración del catálogo, al reinicio del servidor, etc.<br /><br /> NULL = La tabla no tiene un índice de texto completo.|  
|TableFulltextItemCount|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de filas para las que se crearon índices de texto completo correctamente.|  
|TableFulltextKeyColumn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Identificador de la columna asociada al índice de columna único que participa en la definición de índice de texto completo.<br /><br /> 0 = La tabla no tiene un índice de texto completo.|  
|TableFulltextPendingChanges|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de entradas de seguimiento de cambios pendientes de procesamiento.<br /><br /> 0 = El seguimiento de cambios no está habilitado.<br /><br /> NULL = La tabla no tiene un índice de texto completo.|  
|TableFulltextPopulateStatus|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> 0 = Inactiva<br /><br /> 1 = Rellenado completo en curso.<br /><br /> 2 = Rellenado incremental en curso.<br /><br /> 3 = Propagación de cambios de seguimiento en curso.<br /><br /> 4 = Actualización de índices en segundo plano en curso, como el seguimiento de cambios automáticos.<br /><br /> 5 = Indización de texto completo acelerada o pausada.|  
|TableHasActiveFulltextIndex|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene un índice de texto completo activo.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasCheckCnst|Tabla|La tabla tiene una restricción CHECK.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasClustIndex|Tabla|La tabla tiene un índice clúster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDefaultCnst|Tabla|La tabla tiene una restricción DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDeleteTrigger|Tabla|La tabla tiene un desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignKey|Tabla|La tabla tiene una restricción FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignRef|Tabla|Una restricción FOREIGN KEY hace referencia a la tabla.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIdentity|Tabla|La tabla tiene una columna de identidad.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIndex|Tabla|La tabla tiene un índice de cualquier tipo.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasInsertTrigger|Tabla|El objeto tiene un desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasNonclustIndex|Tabla|La tabla tiene un índice no clúster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasPrimaryKey|Tabla|La tabla tiene una clave principal.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasRowGuidCol|Tabla|La tabla tiene un parámetro ROWGUIDCOL para una columna **uniqueidentifier**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTextImage|Tabla|La tabla tiene una columna **text**, **ntext** o **image**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTimestamp|Tabla|La tabla tiene una columna **timestamp**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUniqueCnst|Tabla|La tabla tiene una restricción UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUpdateTrigger|Tabla|El objeto tiene un desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasVarDecimalStorageFormat|Tabla|La tabla está habilitada para el formato de almacenamiento **vardecimal**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|Tabla|La tabla tiene un desencadenador INSERT.<br /><br /> >1 = Identificador del primer desencadenador con el tipo especificado.|  
|TableInsertTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores INSERT.<br /><br /> >0 = Número de desencadenadores INSERT.|  
|TableIsFake|Tabla|La tabla no es real. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] la materializa internamente a petición.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsLockedOnBulkLoad|Tabla|La tabla está bloqueada debido a una operación con **bcp** o BULK INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsMemoryOptimized|Tabla|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> La tabla tiene optimización para memoria<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**<br /><br /> Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).|  
|TableIsPinned|Tabla|La tabla se ancla para que se mantenga en la memoria caché de datos.<br /><br /> 0 = False<br /><br /> Esta característica no se admite en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ni en versiones posteriores.|  
|TableTextInRowLimit|Tabla|Número máximo de bytes permitidos para text in row.<br /><br /> 0 si no se ha establecido la opción text in row.|  
|TableUpdateTrigger|Tabla|La tabla tiene un desencadenador UPDATE.<br /><br /> > 1 = Identificador del primer desencadenador con el tipo especificado.|  
|TableUpdateTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores UPDATE.<br /><br /> > 0 = Número de desencadenadores UPDATE.|  
|TableHasColumnSet|Tabla|La tabla tiene un conjunto de columnas.<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> Para obtener más información, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).|  
|TableTemporalType|Tabla|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Especifica el tipo de tabla.<br /><br /> 0 = tabla no temporal<br /><br /> 1 = tabla de historial para la tabla con control de versiones del sistema<br /><br /> 2 = tabla temporal con control de versiones del sistema|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="exceptions"></a>Excepciones  
 Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.  
  
 Un usuario solo puede ver los metadatos de elementos protegibles que posea o para los que se le haya concedido permiso. Esto significa que las funciones integradas de emisión de metadatos, como OBJECTPROPERTY, pueden devolver NULL si el usuario no tiene ningún permiso para el objeto. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Observaciones  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] da por hecho que *object_id* se encuentra en el contexto de la base de datos actual. Una consulta que hace referencia a un parámetro *object_id* de otra base de datos devuelve NULL o resultados incorrectos. Por ejemplo, en la siguiente consulta, el contexto de base de datos es la base de datos maestra. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] intentará devolver el valor de propiedad correspondiente al *object_id* especificado en esa base de datos, en lugar de la base de datos especificada en la consulta. La consulta devuelve resultados incorrectos porque la vista `vEmployee` no se encuentra en la base de datos maestra.  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTY(*view_i* d, 'IsIndexable') puede consumir importantes recursos del equipo porque la evaluación de la propiedad IsIndexable requiere el análisis de la definición de la vista, la normalización y la optimización parcial. Aunque la propiedad IsIndexable identifica tablas o vistas que se pueden indizar, es posible que se produzca un error en la creación real del índice si no se cumplen ciertos requisitos de clave de índice. Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 OBJECTPROPERTY(*table_id*, 'TableHasActiveFulltextIndex') devolverá el valor 1 (verdadero) si se agrega al menos una columna de una tabla para la indización. El índice de texto completo se activa para su llenado en el momento en que se agrega la primera columna para la indización.  
  
 Al crear una tabla, la opción QUOTED IDENTIFIER siempre se almacena como ON en los metadatos de la tabla, incluso si la opción está establecida en OFF al crear la tabla. Por tanto, OBJECTPROPERTY(*table_id*, 'IsQuotedIdentOn') siempre devolverá el valor 1 (verdadero).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-verifying-that-an-object-is-a-table"></a>A. Comprobar si un objeto es una tabla  
 En el ejemplo siguiente se comprueba si `UnitMeasure` es una tabla de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 1  
   PRINT 'UnitMeasure is a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 0  
   PRINT 'UnitMeasure is not a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') IS NULL  
   PRINT 'ERROR: UnitMeasure is not a valid object.';  
GO  
```  
  
### <a name="b-verifying-that-a-scalar-valued-user-defined-function-is-deterministic"></a>B. Comprobar si una función escalar definida por el usuario es determinista  
 En el siguiente ejemplo se comprueba si es determinista la función escalar definida por el usuario, `ufnGetProductDealerPrice`, que devuelve un valor **money**.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID('dbo.ufnGetProductDealerPrice'), 'IsDeterministic');  
GO  
```  
  
 El conjunto de resultados muestra que `ufnGetProductDealerPrice` no es una función determinista.  
  
 ```
-----  
0
```  
  
### <a name="c-finding-the-tables-that-belong-to-a-specific-schema"></a>C. Buscar las tablas que pertenecen a un esquema específico  
 En el siguiente ejemplo se devuelven todas las tablas del esquema dbo.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT name, object_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTY(object_id, N'SchemaId') = SCHEMA_ID(N'dbo')  
ORDER BY type_desc, name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-verifying-that-an-object-is-a-table"></a>D. Comprobar si un objeto es una tabla  
 En el ejemplo siguiente se comprueba si `dbo.DimReseller` es una tabla de la base de datos [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```sql  
-- Uses AdventureWorks  
  
IF OBJECTPROPERTY (OBJECT_ID(N'dbo.DimReseller'),'ISTABLE') = 1  
   SELECT 'DimReseller is a table.'  
ELSE   
   SELECT 'DimReseller is not a table.';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  

