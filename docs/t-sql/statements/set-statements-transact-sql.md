---
description: Instrucciones SET (Transact-SQL)
title: Instrucciones SET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET
- SET_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ISO SET statements
- queries [SQL Server], executing
- dates [SQL Server], SET statements
- time [SQL Server], SET statements
- SET statement, about SET statement
- SET statement
- statistical information [SQL Server], SET statements
- locking [SQL Server], SET statements
ms.assetid: f7e107f8-0fcf-408b-b30f-da2323eeb714
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azure-sqldw-latest ||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: b00353ebc513ecdab6b0be57a64c05f84e1ae461
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189403"
---
# <a name="set-statements-transact-sql"></a>Instrucciones SET (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

El lenguaje de programación [!INCLUDE[tsql](../../includes/tsql-md.md)] ofrece varias instrucciones SET que cambian el tratamiento de información específica por parte de la sesión actual. Las instrucciones SET se agrupan en las categorías que figuran en la siguiente tabla.  
  
Para obtener información sobre cómo establecer variables locales con la instrucción SET, vea [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
|Category|Instrucciones|  
|--------------|----------------|  
|Instrucciones de fecha y hora|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)<br /><br /> [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|  
|Instrucciones de bloqueo|[SET DEADLOCK_PRIORITY](../../t-sql/statements/set-deadlock-priority-transact-sql.md)<br /><br /> [SET LOCK_TIMEOUT](../../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|Otras instrucciones|[SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)<br /><br /> [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)<br /><br /> [SET FIPS_FLAGGER](../../t-sql/statements/set-fips-flagger-transact-sql.md)<br /><br /> [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)<br /><br /> [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)<br /><br /> [SET OFFSETS](../../t-sql/statements/set-offsets-transact-sql.md)<br /><br /> [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)|  
|Instrucciones de ejecución de consultas|[SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)<br /><br /> [SET ARITHIGNORE](../../t-sql/statements/set-arithignore-transact-sql.md)<br /><br /> [SET FMTONLY](../../t-sql/statements/set-fmtonly-transact-sql.md)<br /> Nota: [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br /><br /> [SET NOCOUNT](../../t-sql/statements/set-nocount-transact-sql.md)<br /><br /> [SET NOEXEC](../../t-sql/statements/set-noexec-transact-sql.md)<br /><br /> [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)<br /><br /> [SET PARSEONLY](../../t-sql/statements/set-parseonly-transact-sql.md)<br /><br /> [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)<br /><br /> [SET RESULT SET CACHING](../../t-sql/statements/set-result-set-caching-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) (versión preliminar)<br /> Nota:  Esta característica solo se aplica a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].<br /><br /> [SET ROWCOUNT](../../t-sql/statements/set-rowcount-transact-sql.md)<br /><br /> [SET TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md)|  
|Instrucciones de configuración de ISO|[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_OFF](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)<br /><br /> [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)<br /><br /> [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)<br /><br /> [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)|  
|Instrucciones de estadísticas|[SET FORCEPLAN](../../t-sql/statements/set-forceplan-transact-sql.md)<br /><br /> [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md)<br /><br /> [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md)<br /><br /> [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)<br /><br /> [SET STATISTICS IO](../../t-sql/statements/set-statistics-io-transact-sql.md)<br /><br /> [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)<br /><br /> [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)<br /><br /> [SET STATISTICS TIME](../../t-sql/statements/set-statistics-time-transact-sql.md)|  
|Instrucciones de transacciones|[SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)<br /><br /> [SET REMOTE_PROC_TRANSACTIONS](../../t-sql/statements/set-remote-proc-transactions-transact-sql.md)<br /><br /> [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)<br /><br /> [SET XACT_ABORT](../../t-sql/statements/set-xact-abort-transact-sql.md)| 
  
## <a name="considerations-when-you-use-the-set-statements"></a>Consideraciones al utilizar las instrucciones SET  
  
- Todas las instrucciones SET se ejecutan en tiempo de ejecución, salvo estas instrucciones, que se ejecutan en tiempo de análisis:

  - SET FIPS_FLAGGER
  - SET OFFSETS
  - SET PARSEONLY
  - y SET QUOTED_IDENTIFIER  
  
- Si se ejecuta una instrucción SET en un procedimiento almacenado o desencadenador, el valor de la opción de SET se restaura cuando el procedimiento almacenado o desencadenador devuelve el control. Además, si especifica una instrucción SET en una cadena de SQL dinámico que se ejecuta mediante **sp_executesql** o EXECUTE, el valor de la opción de SET se restaura cuando el control vuelve del lote que especificó en la cadena de SQL dinámico.  
  
- Los procedimientos almacenados se ejecutan con las opciones de SET especificadas en tiempo de ejecución, excepto en el caso de SET ANSI_NULLS y SET QUOTED_IDENTIFIER. Los procedimientos almacenados que utilizan SET ANSI_NULLS o SET QUOTED_IDENTIFIER usan las opciones especificadas en el momento de crear el procedimiento almacenado. Si se usan dentro de un procedimiento almacenado, las opciones SET no tienen ningún efecto.  
  
- El parámetro **user options** de **sp_configure** permite aplicar opciones a todo el servidor y sirve para múltiples bases de datos. Además, esta opción se comporta como una instrucción SET explícita, pero con la diferencia de que tiene lugar en el momento de iniciar la sesión.  
  
- Las opciones de base de datos establecidas con ALTER DATABASE solo son válidas en el nivel de base de datos y solo tienen efecto cuando se establecen explícitamente. Las opciones de base de datos anulan las opciones de instancia que se establecen mediante **sp_configure**.  
  
- Si una instrucción SET usa ON y OFF, puede especificar cualquiera para varias opciones de SET.
  
    > [!NOTE]  
    >  Esto no se aplica a las opciones de SET relacionadas con estadísticas.
  
     Por ejemplo, `SET QUOTED_IDENTIFIER, ANSI_NULLS ON` establece QUOTED_IDENTIFIER y ANSI_NULLS en ON.  
  
- Las opciones de las instrucciones SET anulan las opciones de base de datos idénticas que se establecen mediante ALTER DATABASE. Por ejemplo, el valor especificado en una instrucción SET ANSI_NULLS anulará la opción de la base de datos de ANSI_NULLS. Además, en algunas opciones de conexión se establece ON automáticamente cuando un usuario se conecta a una base de datos con los valores efectivos de la vez anterior que se utilizó la opción de **sp_configure user options** o los valores que se aplican a todas las conexiones ODBC y OLE/DB.  
  
- Las instrucciones ALTER, CREATE y DROP DATABASE no respetan la opción de SET LOCK_TIMEOUT.  
  
- Cuando una instrucción SET global o abreviada establece varias opciones, al ejecutar la instrucción SET abreviada se restablecen los valores anteriores de todas las opciones a las que afecta. Si una opción de SET que se ve afectada por una instrucción SET abreviada se establece después de ejecutar la instrucción SET abreviada, la instrucción SET individual tiene precedencia sobre las opciones comparables de la instrucción abreviada. Un ejemplo de una instrucción SET abreviada sería SET ANSI_DEFAULTS.  
  
- Al utilizar lotes, el lote establecido mediante la instrucción USE determina el contexto de la base de datos. Las consultas no planeadas y todas las demás instrucciones que se ejecuten fuera del procedimiento almacenado y que se encuentren en lotes heredan las opciones de base de datos y conexión establecidas mediante la instrucción USE.  
  
- Las solicitudes MARS (conjuntos de resultados activos múltiples) comparten un estado global que contiene los valores de SET de la sesión más reciente. Al ejecutar una solicitud, se pueden modificar las opciones de SET. Los cambios son específicos al contexto de solicitud en el que se encuentran, y no afectan a otras solicitudes MARS simultáneas. No obstante, una vez ejecutada la solicitud, las nuevas opciones de SET se copian en el estado de sesión global. Las nuevas solicitudes que se ejecuten en la misma sesión después de este cambio utilizarán estas nuevas opciones de SET.  
  
- Cuando se ejecuta un procedimiento almacenado desde un lote o desde otro procedimiento almacenado, la ejecución se realiza con las opciones configuradas en la base de datos que tiene el procedimiento almacenado. Por ejemplo, cuando el procedimiento almacenado **db1.dbo.sp1** llama al procedimiento almacenado **db2.dbo.sp2**, el procedimiento almacenado **sp1** se ejecuta con la opción de nivel de compatibilidad actual de la base de datos **db1**, y el procedimiento almacenado **sp2** se ejecuta con la opción de nivel de compatibilidad actual de la base de datos **db2**.  
  
- Cuando una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] está relacionada con objetos que se encuentran en varias bases de datos, el contexto de la base de datos actual y el contexto de la conexión actual se aplican a dicha instrucción. En este caso, si la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] está en un lote, el contexto de conexión actual es la base de datos definida mediante la instrucción USE; si la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] está en un procedimiento almacenado, el contexto de conexión es la base de datos que contiene el procedimiento almacenado.  
  
- Al crear y manipular índices en columnas calculadas o vistas indexadas, debe establecer estas opciones de SET en ON: ARITHABORT, CONCAT_NULL_YIELDS_NULL, QUOTED_IDENTIFIER, ANSI_NULLS, ANSI_PADDING y ANSI_WARNINGS. Establezca la opción de NUMERIC_ROUNDABORT en OFF.  
  
  Si no establece ninguna de estas opciones en los valores exigidos, se producirá un error en las acciones INSERT, UPDATE, DELETE, DBCC CHECKDB y DBCC CHECKTABLE en las tablas o vistas indexadas con índices en las columnas calculadas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generará un error indicando las opciones que no se han establecido correctamente. Además, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procesará las instrucciones SELECT en estas tablas o vistas indexadas como si no existieran los índices en las columnas calculadas ni en las vistas. 

- Cuando SET RESULT_SET_CACHING es ON, permite la característica de almacenamiento en caché de los resultados para la sesión actual del cliente.   Result_set_caching no se puede activar para una sesión si estaba desactivado en el nivel de base de datos.    Cuando SET RESULT_SET_CACHING es OFF, la característica de almacenamiento en caché del conjunto de resultados para la sesión actual del cliente. Para cambiar esta configuración, se requiere la pertenencia al rol público.
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2
