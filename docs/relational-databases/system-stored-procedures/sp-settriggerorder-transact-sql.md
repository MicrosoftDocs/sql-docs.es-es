---
description: sp_settriggerorder (Transact-SQL)
title: sp_settriggerorder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7977873f4b2887b736ffc16f5592340f5b87dee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207365"
---
# <a name="sp_settriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica los desencadenadores AFTER que se activan en primer o último lugar. Los desencadenadores AFTER que se activan entre el primero y el último se ejecutan en un orden indefinido.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @triggername = ] '[ _triggerschema.] _triggername'` Es el nombre del desencadenador y el esquema al que pertenece, si procede, cuyo orden se va a establecer o cambiar. [_triggerschema_**.**] *triggername* es de **tipo sysname**. Si el nombre no corresponde a un desencadenador o si corresponde a un desencadenador INSTEAD OF, el procedimiento devolverá un error. no se puede especificar *triggerschema* para los desencadenadores DDL o Logon.  
  
`[ @order = ] 'value'` Es el valor del nuevo orden del desencadenador. el *valor* es **VARCHAR (10)** y puede ser cualquiera de los valores siguientes.  
  
> [!IMPORTANT]  
>  Los desencadenadores **primero** y **último** deben ser dos desencadenadores diferentes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Primero**|El desencadenador se activa primero.|  
|**Último**|El desencadenador se activa el último.|  
|**None**|El desencadenador se activa sin un orden definido.|  
  
`[ @stmttype = ] 'statement_type'` Especifica la instrucción SQL que activa el desencadenador. *statement_type* es **VARCHAR (50)** y puede ser INSERT, Update, Delete, Logon o cualquier [!INCLUDE[tsql](../../includes/tsql-md.md)] evento de instrucción enumerado en [eventos DDL](../../relational-databases/triggers/ddl-events.md). Los grupos de eventos no se pueden especificar.  
  
 Un desencadenador se puede designar como **primer** o **último** desencadenador para un tipo de instrucción solo después de que ese desencadenador se haya definido como desencadenador para ese tipo de instrucción. Por ejemplo, el desencadenador **TR1** se puede designar **primero** para INSERT en la tabla **T1** si **TR1** se define como un desencadenador INSERT. [!INCLUDE[ssDE](../../includes/ssde-md.md)]Devuelve un error si **TR1**, que se ha definido solo como desencadenador de inserción, se establece como **primer** o **último** desencadenador para una instrucción UPDATE. Para obtener más información, vea la sección Comentarios.  
  
 **\@ espacio de nombres =** { **' base de datos**'  |  **' servidor '** | ACEPTA  
 Cuando *triggername* es un desencadenador DDL, el **\@ espacio de nombres** especifica si *triggername* se creó con ámbito de base de datos o ámbito de servidor. Si *triggername* es un desencadenador Logon, se debe especificar Server. Para obtener más información sobre el ámbito del desencadenador DDL, vea [desencadenadores DDL](../../relational-databases/triggers/ddl-triggers.md). Si no se especifica, o si se especifica NULL, *triggername* es un desencadenador DML.  
  
* SERVER se aplica a: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) y 1 (error)  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="dml-triggers"></a>Desencadenadores DML  
 Solo puede haber un **primero** y un **último** desencadenador para cada instrucción en una sola tabla.  
  
 Si ya hay un desencadenador **First** definido en la tabla, base de datos o servidor, no se puede designar un nuevo desencadenador como **primero** para la misma tabla, base de datos o servidor para el mismo *statement_type*. Esta restricción también aplica los **últimos** desencadenadores.  
  
 La replicación genera automáticamente un primer desencadenador para cualquier tabla incluida en una suscripción de actualización inmediata o en cola. La replicación requiere que su desencadenador sea el primero. Generará un error si se intenta incluir una tabla con un primer desencadenador en una suscripción de actualización inmediata o en cola. Si intenta convertir un desencadenador en el primero después de haber incluido una tabla en una suscripción, **sp_settriggerorder** devolverá un error. Si usa ALTER TRIGGER en el desencadenador de replicación o utiliza **sp_settriggerorder** para cambiar el desencadenador de replicación a un desencadenador **último** o **ninguno** , la suscripción no funcionará correctamente.  
  
## <a name="ddl-triggers"></a>Desencadenadores DDL  
 Si un desencadenador DDL con ámbito de base de datos y un desencadenador DDL con ámbito de servidor existen en el mismo evento, puede especificar que ambos desencadenadores sean un **primer** desencadenador o un **último** desencadenador. Sin embargo, los desencadenadores con ámbito de servidor siempre se inician en primer lugar. En general, el orden de ejecución de los desencadenadores DDL que existen en el mismo evento es el siguiente:  
  
1.  Desencadenador de nivel de servidor marcado **primero**.  
  
2.  Otros desencadenadores de servidor.  
  
3.  Desencadenador de nivel de servidor marcado en **último lugar**.  
  
4.  Desencadenador de nivel de base de datos marcado **primero**.  
  
5.  Otros desencadenadores de base de datos.  
  
6.  Desencadenador de nivel de base de datos marcado en **último lugar**.  
  
## <a name="general-trigger-considerations"></a>Consideraciones generales sobre los desencadenadores  
 Si una instrucción ALTER TRIGGER cambia un desencadenador primero o último, se quita el **primer** o **último** atributo establecido originalmente en el desencadenador y el valor se reemplaza por **ninguno**. El valor de orden se debe restablecer mediante **sp_settriggerorder**.  
  
 Si el mismo desencadenador debe designarse como el primer o el último orden de más de un tipo de instrucción, se debe ejecutar **sp_settriggerorder** para cada tipo de instrucción. Además, el desencadenador debe definirse primero para un tipo de instrucción antes de que se pueda designar como el **primer** o **último** desencadenador que se activará para ese tipo de instrucción.  
  
## <a name="permissions"></a>Permisos  
 Para establecer el orden de un desencadenador DDL con ámbito de servidor (ON ALL SERVER) o de un desencadenador de inicio de sesión se requiere el permiso CONTROL SERVER.  
  
 Para establecer el orden de un desencadenador DDL con ámbito de base de datos (ON DATABASE), se necesita el permiso ALTER ANY DATABASE DDL TRIGGER.  
  
 Para establecer el orden de un desencadenador DML, se necesita el permiso ALTER para la tabla o la vista donde está definido el desencadenador.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>A. Configurar el orden de activación de un desencadenador DML  
 En el siguiente ejemplo se especifica que `uSalesOrderHeader` sea el primer desencadenador que se active después de que se produzca una operación `UPDATE` en la tabla `Sales.SalesOrderHeader`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. Configurar el orden de activación de un desencadenador DDL  
 El siguiente ejemplo especifica que `ddlDatabaseTriggerLog` sea el primer desencadenador que se active después de que se produzca un evento `ALTER_TABLE` en la tabla [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
