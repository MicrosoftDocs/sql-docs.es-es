---
description: OBJECTPROPERTYEX (Transact-SQL)
title: OBJECTPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECTPROPERTYEX
- OBJECTPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTYEX function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: be36b3e3-3309-4332-bfb5-c7e9cf8dc8bd
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 611d0f17ae0ed8cd0e7679d149b186563f802ba4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237480"
---
# <a name="objectpropertyex-transact-sql"></a>OBJECTPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información acerca de los objetos de ámbito de esquema de la base de datos actual. Para obtener una lista de estos objetos, vea [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). OBJECTPROPERTYEX no se puede utilizar con los objetos que no pertenecen al ámbito de esquema, como los desencadenadores de lenguaje de definición de datos (DDL) y las notificaciones de eventos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
OBJECTPROPERTYEX ( id , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *id*  
 Es una expresión que representa el identificador del objeto en la base de datos actual. *id* es de tipo **int** y se considera que se trata de un objeto de ámbito de esquema en el contexto de la base de datos actual.  
  
 *property*  
 Es una expresión que contiene la información sobre el objeto especificado por el identificador que se va a devolver. El tipo de valor devuelto es **sql_variant**. En la siguiente tabla se muestra el tipo de datos base de cada valor de propiedad.  
  
> [!NOTE]  
>  A menos que se especifique lo contrario, se devuelve NULL si *property* no es un nombre de propiedad válido, *id* no es un identificador de objeto válido, *id* es un tipo de objeto incompatible con el valor *property* especificado o el autor de la llamada no tiene permiso para ver los metadatos del objeto.  
  
|Nombre de propiedad|Tipo de objeto|Descripción y valores devueltos|  
|-------------------|-----------------|-------------------------------------|  
|BaseType|Cualquier objeto en el ámbito de esquema|Identifica el tipo base del objeto. Cuando el objeto especificado es un sinónimo, se devuelve el tipo base del objeto subyacente.<br /><br /> NonNULL = Tipo de objeto<br /><br /> Tipo de datos base: **char(2)**|  
|CnstIsClustKey|Restricción|Restricción PRIMARY KEY con un índice clúster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsColumn|Restricción|Restricción CHECK, DEFAULT o FOREIGN KEY en una única columna.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsDeleteCascade|Restricción|Restricción FOREIGN KEY con la opción ON DELETE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsDisabled|Restricción|Restricción deshabilitada.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsNonclustKey|Restricción|Restricción PRIMARY KEY con un índice no clúster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsNotRepl|Restricción|La restricción se define utilizando las palabras clave NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsNotTrusted|Restricción|La restricción se ha habilitado sin comprobar las filas existentes. Por lo tanto, es posible que no pueda mantenerse para todas las filas.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|CnstIsUpdateCascade|Restricción|Restricción FOREIGN KEY con la opción ON UPDATE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsAfterTrigger|Desencadenador|Desencadenador AFTER.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsAnsiNullsOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|El valor de ANSI_NULLS en el momento de su creación.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsDeleteTrigger|Desencadenador|Desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsFirstDeleteTrigger|Desencadenador|El primer desencadenador que se activa cuando se ejecuta DELETE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsFirstInsertTrigger|Desencadenador|El primer desencadenador que se activa cuando se ejecuta INSERT en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsFirstUpdateTrigger|Desencadenador|El primer desencadenador que se activa cuando se ejecuta UPDATE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsInsertTrigger|Desencadenador|Desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsInsteadOfTrigger|Desencadenador|Desencadenador INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsLastDeleteTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta DELETE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsLastInsertTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta INSERT en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsLastUpdateTrigger|Desencadenador|Último desencadenador que se activa cuando se ejecuta UPDATE en la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsQuotedIdentOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Valor de QUOTED_IDENTIFIER en el momento de su creación.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsStartup|Procedimiento|Procedimiento de inicio.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsTriggerDisabled|Desencadenador|Desencadenador deshabilitado.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsTriggerNotForRepl|Desencadenador|Desencadenador definido como NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsUpdateTrigger|Desencadenador|Desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|ExecIsWithNativeCompilation|Procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)]|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El procedimiento se compila de forma nativa.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasAfterTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador AFTER.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasDeleteTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasInsertTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasInsteadOfTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|HasUpdateTrigger|Tabla, vista|La tabla o la vista tiene un desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsAnsiNullsOn|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Especifica que el valor de la opción ANSI NULLS para la tabla es ON, lo que significa que todas las comparaciones con un valor NULL se evalúan como UNKNOWN. Este valor se aplica a todas las expresiones de la definición de tabla, incluidas las columnas calculadas y las restricciones, mientras la tabla exista.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsCheckCnst|Cualquier objeto en el ámbito de esquema|Restricción CHECK.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsConstraint|Cualquier objeto en el ámbito de esquema|Restricción.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsDefault|Cualquier objeto en el ámbito de esquema|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Valor predeterminado enlazado.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsDefaultCnst|Cualquier objeto en el ámbito de esquema|Restricción DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsDeterministic|Funciones escalares y con valores de tabla, vista|Propiedad de determinismo de la función o vista.<br /><br /> 1 = Determinista<br /><br /> 0 = No determinista<br /><br /> Tipo de datos base: **int**|  
|IsEncrypted|Función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Indica que el texto original de la instrucción del módulo se ha convertido a un formato confuso. La salida de la protección no es directamente visible en ninguna de las vistas de catálogo de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Los usuarios sin acceso a las tablas del sistema o a los archivos de base de datos no pueden recuperar el texto ofuscado. En cambio, está disponible para los usuarios que puedan obtener acceso a las tablas del sistema a través del [puerto DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) o directamente a los archivos de base de datos. Además, los usuarios que pueden adjuntar un depurador al proceso del servidor pueden recuperar el procedimiento original de la memoria en tiempo de ejecución.<br /><br /> 1 = Cifrada<br /><br /> 0 = No cifrado<br /><br /> Tipo de datos base: **int**|  
|IsExecuted|Cualquier objeto en el ámbito de esquema|Especifica que el objeto se puede ejecutar (vista, procedimiento, función o desencadenador).<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsExtendedProc|Cualquier objeto en el ámbito de esquema|Procedimiento extendido.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsForeignKey|Cualquier objeto en el ámbito de esquema|Restricción FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsIndexed|Tabla, vista|Una tabla o vista con un índice.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsIndexable|Tabla, vista|Una tabla o una vista en la que es posible crear un índice.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsInlineFunction|Función|Función insertada.<br /><br /> 1 = Función insertada<br /><br /> 0 = Función no insertada<br /><br /> Tipo de datos base: **int**|  
|IsMSShipped|Cualquier objeto en el ámbito de esquema|Un objeto creado durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsPrecise|Columna calculada, función, tipo definido por el usuario, vista|Indica si el objeto contiene un cálculo impreciso, como una operación de punto flotante.<br /><br /> 1 = Preciso<br /><br /> 0 = Impreciso<br /><br /> Tipo de datos base: **int**|  
|IsPrimaryKey|Cualquier objeto en el ámbito de esquema|Restricción PRIMARY KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsProcedure|Cualquier objeto en el ámbito de esquema|Procedimiento.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsQuotedIdentOn|Restricción CHECK, definición DEFAULT, función de [!INCLUDE[tsql](../../includes/tsql-md.md)], procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], tabla, desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)], vista|Especifica que el valor del identificador entrecomillado para el objeto es ON, lo que significa que las comillas dobles delimitan los identificadores en todas las expresiones de la definición de objeto.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsQueue|Cualquier objeto en el ámbito de esquema|Cola de Service Broker<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsReplProc|Cualquier objeto en el ámbito de esquema|Procedimiento de replicación.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsRule|Cualquier objeto en el ámbito de esquema|Regla enlazada.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsScalarFunction|Función|Función escalar.<br /><br /> 1 = Función escalar<br /><br /> 0 = Función no escalar<br /><br /> Tipo de datos base: **int**|  
|IsSchemaBound|Función, procedimiento y vista|Función o vista enlazada al esquema creada mediante SCHEMABINDING.<br /><br /> 1 = Enlazada al esquema<br /><br /> 0 = No enlazada al esquema<br /><br /> Tipo de datos base: **int**|  
|IsSystemTable|Tabla|Tabla del sistema.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsSystemVerified|Columna calculada, función, tipo definido por el usuario, vista|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede comprobar las propiedades de precisión y determinismo del objeto.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsTable|Tabla|Tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsTableFunction|Función|Función con valores de tabla.<br /><br /> 1 = Función con valores de tabla<br /><br /> 0 = Función con valores no de tabla.<br /><br /> Tipo de datos base: **int**|  
|IsTrigger|Cualquier objeto en el ámbito de esquema|Desencadenador.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsUniqueCnst|Cualquier objeto en el ámbito de esquema|Restricción UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsUserTable|Tabla|Tabla definida por el usuario.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|IsView|Ver|Vista.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|OwnerId|Cualquier objeto en el ámbito de esquema|Propietario del objeto.<br /><br /> **Nota:** El propietario del esquema no es necesariamente el propietario del objeto. Por ejemplo, los objetos secundarios (aquellos en los que *parent_object_id* no es NULL) siempre devolverán el mismo identificador de propietario que el primario.<br /><br /> NonNULL = Id. de usuario de la base de datos del propietario del objeto.<br /><br /> NULL = Tipo de objeto no compatible o identificador de objeto no válido.<br /><br /> Tipo de datos base: **int**|  
|SchemaId|Cualquier objeto en el ámbito de esquema|Identificador de esquema asociado al objeto.<br /><br /> NonNULL = Identificador de esquema del objeto.<br /><br /> Tipo de datos base: **int**|  
|SystemDataAccess|Función, vista|El objeto obtiene acceso a los datos del sistema, los catálogos del sistema o las tablas virtuales del sistema en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = Ninguna<br /><br /> 1 = Lectura<br /><br /> Tipo de datos base: **int**|  
|TableDeleteTrigger|Tabla|La tabla tiene un desencadenador DELETE.<br /><br /> >1 = Identificador del primer desencadenador con el tipo especificado.<br /><br /> Tipo de datos base: **int**|  
|TableDeleteTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores DELETE.<br /><br /> NonNULL = Número de desencadenadores DELETE<br /><br /> Tipo de datos base: **int**|  
|TableFullTextMergeStatus|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Indica si una tabla que tiene un índice de texto completo se está combinando actualmente.<br /><br /> 0 = La tabla no tiene un índice de texto completo o el índice de texto completo no se está combinando.<br /><br /> 1 = El índice de texto completo se está combinando.|  
|TableFullTextBackgroundUpdateIndexOn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene habilitado el índice de actualización de texto completo en segundo plano (seguimiento de cambios automáticos).<br /><br /> 1 = TRUE<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableFulltextCatalogId|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Identificador del catálogo de texto completo en el que residen los datos de índice de texto completo para la tabla.<br /><br /> Distinto de cero = Identificador del catálogo de texto completo, asociado al índice único que identifica las filas en una tabla indizada de texto completo.<br /><br /> 0 = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**|  
|TableFullTextChangeTrackingOn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene habilitado el seguimiento de cambios de texto completo.<br /><br /> 1 = TRUE<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableFulltextDocsProcessed|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de filas procesadas desde el comienzo de la indización de texto completo. En una tabla que se indiza para búsquedas en texto completo, todas las columnas de una fila se consideran como parte de un documento que se va a indizar.<br /><br /> 0 = No se ha completado ningún rastreo activo ni ninguna indización de texto completo.<br /><br /> > 0 = Uno de los casos siguientes (A o B): A) El número de documentos procesados por operaciones de inserción o actualización desde el inicio del rellenado de seguimiento de cambios completo, incremental o manual; B) El número de filas procesadas por operaciones de inserción o actualización desde que se habilitó el seguimiento de cambios con el rellenado del índice de actualización en segundo plano, la modificación del esquema de índice de texto completo, la regeneración del catálogo de texto completo o el reinicio de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], etc.<br /><br /> NULL = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**<br /><br /> **Nota**: Esta propiedad no supervisa ni cuenta las filas eliminadas.|  
|TableFulltextFailCount|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de filas que no ha indizado la búsqueda de texto completo.<br /><br /> 0 = El rellenado se ha completado.<br /><br /> > 0 = Uno de los casos siguientes (A o B): A) Número de documentos no indizados desde el comienzo del rellenado de seguimiento de cambios de actualización completa, incremental y manual; B) Para el seguimiento de cambios con actualización de índices en segundo plano, el número de filas no indizadas desde el comienzo del rellenado o desde su reinicio. Esto podría ser debido a un cambio de esquema, una regeneración del catálogo, un reinicio del servidor, etc.<br /><br /> NULL = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**|  
|TableFulltextItemCount|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> NonNULL = El número de filas que se han indizado por texto completo correctamente.<br /><br /> NULL = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**|  
|TableFulltextKeyColumn|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Identificador de la columna asociada al índice único de una sola columna que forma parte de la definición de un índice de texto completo y un índice semántico.<br /><br /> 0 = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**|  
|TableFulltextPendingChanges|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de entradas de seguimiento de cambios pendientes de procesamiento.<br /><br /> 0 = El seguimiento de cambios no está habilitado.<br /><br /> NULL = La tabla no tiene un índice de texto completo.<br /><br /> Tipo de datos base: **int**|  
|TableFulltextPopulateStatus|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> 0 = Inactiva<br /><br /> 1 = Rellenado completo en curso.<br /><br /> 2 = Rellenado incremental en curso.<br /><br /> 3 = Propagación de cambios de seguimiento en curso.<br /><br /> 4 = Actualización de índices en segundo plano en curso, como el seguimiento de cambios automáticos.<br /><br /> 5 = Indización de texto completo acelerada o pausada.<br /><br /> 6 = Se produjo un error. Examine el registro de rastreo para obtener más información. Para obtener más información, consulte la sección **Solución de errores en un rellenado de texto completo (rastreo)** de [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Tipo de datos base: **int**|  
|TableFullTextSemanticExtraction|Tabla|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> La tabla está habilitada para la indización semántica.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasActiveFulltextIndex|Tabla|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> La tabla tiene un índice de texto completo activo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasCheckCnst|Tabla|La tabla tiene una restricción CHECK.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasClustIndex|Tabla|La tabla tiene un índice clúster.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasDefaultCnst|Tabla|La tabla tiene una restricción DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasDeleteTrigger|Tabla|La tabla tiene un desencadenador DELETE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasForeignKey|Tabla|La tabla tiene una restricción FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasForeignRef|Tabla|Una restricción FOREIGN KEY hace referencia a la tabla.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasIdentity|Tabla|La tabla tiene una columna de identidad.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasIndex|Tabla|La tabla tiene un índice de cualquier tipo.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasInsertTrigger|Tabla|El objeto tiene un desencadenador INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasNonclustIndex|Tabla|La tabla tiene un índice no agrupado.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasPrimaryKey|Tabla|La tabla tiene una clave principal.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasRowGuidCol|Tabla|La tabla tiene un parámetro ROWGUIDCOL para una columna **uniqueidentifier**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasTextImage|Tabla|La tabla tiene una columna **text**, **ntext** o **image**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasTimestamp|Tabla|La tabla tiene una columna **timestamp**.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasUniqueCnst|Tabla|La tabla tiene una restricción UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasUpdateTrigger|Tabla|El objeto tiene un desencadenador UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableHasVarDecimalStorageFormat|Tabla|La tabla está habilitada para el formato de almacenamiento **vardecimal**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|Tabla|La tabla tiene un desencadenador INSERT.<br /><br /> >1 = Identificador del primer desencadenador con el tipo especificado.<br /><br /> Tipo de datos base: **int**|  
|TableInsertTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores INSERT.<br /><br /> >0 = Número de desencadenadores INSERT.<br /><br /> Tipo de datos base: **int**|  
|TableIsFake|Tabla|La tabla no es real. [!INCLUDE[ssDE](../../includes/ssde-md.md)] la materializa internamente a petición.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableIsLockedOnBulkLoad|Tabla|La tabla está bloqueada debido a un trabajo **bcp** o BULK INSERT.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**|  
|TableIsMemoryOptimized|Tabla|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> La tabla tiene optimización para memoria<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Tipo de datos base: **int**<br /><br /> Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).|  
|TableIsPinned|Tabla|La tabla se ancla para que se mantenga en la memoria caché de datos.<br /><br /> 0 = False<br /><br /> Esta característica no es compatible con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores.|  
|TableTextInRowLimit|Tabla|La tabla tiene establecida la opción text in row.<br /><br /> >0 = Número máximo de bytes permitidos para text in row.<br /><br /> 0 = La opción text in row no está establecida.<br /><br /> Tipo de datos base: **int**|  
|TableUpdateTrigger|Tabla|La tabla tiene un desencadenador UPDATE.<br /><br /> > 1 = Identificador del primer desencadenador con el tipo especificado.<br /><br /> Tipo de datos base: **int**|  
|TableUpdateTriggerCount|Tabla|La tabla tiene el número especificado de desencadenadores UPDATE.<br /><br /> > 0 = Número de desencadenadores UPDATE.<br /><br /> Tipo de datos base: **int**|  
|UserDataAccess|Función, vista|Indica que el objeto obtiene acceso a datos y tablas de usuario en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Lectura<br /><br /> 0 = Ninguna<br /><br /> Tipo de datos base: **int**|  
|TableHasColumnSet|Tabla|La tabla tiene un conjunto de columnas.<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> Para obtener más información, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).|  
|Cardinalidad|Tabla (del sistema o definida por el usuario), vista o índice|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> El número de filas del objeto especificado.|  
|TableTemporalType|Tabla|**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Especifica el tipo de tabla.<br /><br /> 0 = tabla no temporal<br /><br /> 1 = tabla de historial para la tabla con control de versiones del sistema<br /><br /> 2 = tabla temporal con control de versiones del sistema|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **sql_variant**  
  
## <a name="exceptions"></a>Excepciones  
 Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.  
  
 Un usuario solo puede ver los metadatos de elementos protegibles que posea o para los que se le haya concedido permiso. Esto significa que las funciones integradas de emisión de metadatos, como OBJECTPROPERTYEX, pueden devolver NULL si el usuario no tiene ningún permiso para el objeto. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Observaciones  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] da por hecho que *object_id* se encuentra en el contexto de la base de datos actual. Una consulta que hace referencia a un parámetro *object_id* de otra base de datos devuelve NULL o resultados incorrectos. Por ejemplo, en la siguiente consulta, el contexto de base de datos es la base de datos maestra. [!INCLUDE[ssDE](../../includes/ssde-md.md)] intentará devolver el valor de propiedad correspondiente al *object_id* especificado en esa base de datos, en lugar de la base de datos especificada en la consulta. La consulta devuelve resultados incorrectos porque la vista `vEmployee` no se encuentra en la base de datos maestra.  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTYEX(*view_i* d, 'IsIndexable') puede consumir importantes recursos del equipo porque la evaluación de la propiedad IsIndexable requiere el análisis de la definición de la vista, la normalización y la optimización parcial. Aunque la propiedad IsIndexable identifica tablas o vistas que se pueden indizar, es posible que se produzca un error en la creación real del índice si no se cumplen ciertos requisitos de clave de índice. Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 OBJECTPROPERTYEX (*table_id*, 'TableHasActiveFulltextIndex') devolverá el valor 1 (verdadero) si se agrega al menos una columna de una tabla para la indización. El índice de texto completo se activa para su llenado en el momento en que se agrega la primera columna para la indización.  
  
 Se aplican restricciones sobre la visibilidad de los metadatos al conjunto de resultados. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-finding-the-base-type-of-an-object"></a>A. Buscar el tipo base de un objeto  
 En el siguiente ejemplo se crea un sinónimo `MyEmployeeTable` para la tabla `Employee` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y, a continuación, se devuelve el tipo base del sinónimo.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SYNONYM MyEmployeeTable FOR HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX ( object_id(N'MyEmployeeTable'), N'BaseType')AS [Base Type];  
GO  
```  
  
 El conjunto de resultados muestra que el tipo base del objeto subyacente, la tabla `Employee`, es una tabla de usuario.  
  
 ```
Base Type 
--------  
U
```  
  
### <a name="b-returning-a-property-value"></a>B. Devolver un valor de propiedad  
 En el siguiente ejemplo se devuelve el número de desencadenadores UPDATE de la tabla especificada.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'HumanResources.Employee'), N'TABLEUPDATETRIGGERCOUNT');  
GO  
```  
  
### <a name="c-finding-tables-that-have-a-foreign-key-constraint"></a>C. Buscar tablas que tengan una restricción FOREIGN KEY  
 En el ejemplo siguiente se utiliza la propiedad `TableHasForeignKey` para devolver todas las tablas que tengan una restricción FOREIGN KEY.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name, object_id, schema_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTYEX(object_id, N'TableHasForeignKey') = 1  
ORDER BY name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-finding-the-base-type-of-an-object"></a>D. Buscar el tipo base de un objeto  
 En el ejemplo siguiente se devuelve el tipo de objeto `dbo.DimReseller` de base.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OBJECTPROPERTYEX ( object_id(N'dbo.DimReseller'), N'BaseType')AS BaseType;  
```  
  
 El conjunto de resultados muestra que el tipo base del objeto subyacente, la tabla `dbo.DimReseller`, es una tabla de usuario.  
  
```  
BaseType   
--------   
U   
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)  
  
  

