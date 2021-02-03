---
description: ALTER TRIGGER (Transact-SQL)
title: ALTER TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER TRIGGER
- ALTER_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, modifying
- triggers [SQL Server], modifying
- modifying triggers
- ALTER TRIGGER statement
- DML triggers, modifying
ms.assetid: 2a99c7c1-ac2f-4637-aa7c-3d1bf514e500
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a15c0353b56857ec5ccd2c1acff5479d3c312d10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199447"
---
# <a name="alter-trigger-transact-sql"></a>ALTER TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica la definición de un desencadenador logon, DDL o DML creado anteriormente por una instrucción CREATE TRIGGER. Los desencadenadores se crean con CREATE TRIGGER. Se pueden crear directamente a partir de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] o métodos de ensamblados creados en Common Language Runtime (CLR) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] y cargados en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información sobre los parámetros que se usan en la instrucción ALTER TRIGGER, vea [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- SQL Server Syntax  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table | view )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
[ NOT FOR REPLICATION ]   
AS { sql_statement [ ; ] [ ...n ] | EXTERNAL NAME <method specifier>   
[ ; ] }   
  
<dml_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table 
-- (DML Trigger on memory-optimized tables)  

ALTER TRIGGER schema_name.trigger_name   
ON  ( table  )   
[ WITH <dml_trigger_option> [ ,...n ] ]  
 ( FOR | AFTER )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [ ...n ] }   
  
<dml_trigger_option> ::=  
    [ NATIVE_COMPILATION ]  
    [ SCHEMABINDING ]  
    [ <EXECUTE AS Clause> ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, 
-- or UPDATE statement (DDL Trigger)  
  
ALTER TRIGGER trigger_name   
ON { DATABASE | ALL SERVER }   
[ WITH <ddl_trigger_option> [ ,...n ] ]  
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement [ ; ] | EXTERNAL NAME <method specifier>   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ <EXECUTE AS Clause> ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
  
-- Trigger on a LOGON event (Logon Trigger)  

ALTER TRIGGER trigger_name   
ON ALL SERVER   
[ WITH <logon_trigger_option> [ ,...n ] ]  
{ FOR| AFTER } LOGON   
AS { sql_statement  [ ; ] [ ,...n ] | EXTERNAL NAME < method specifier >  
  [ ; ] }  
  
<logon_trigger_option> ::=  
    [ ENCRYPTION ]  
    [ EXECUTE AS Clause ]  
  
<method_specifier> ::=  
    assembly_name.class_name.method_name  
```  
  
```syntaxsql
-- Azure SQL Database Syntax   
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)   
  
ALTER TRIGGER schema_name. trigger_name   
ON (table | view )   
 [ WITH <dml_trigger_option> [ ,...n ] ]   
 ( FOR | AFTER | INSTEAD OF )   
{ [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] }   
AS { sql_statement [ ; ] [...n ] }   
  
<dml_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]   
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE, or UPDATE statement (DDL Trigger)   
  
ALTER TRIGGER trigger_name   
ON { DATABASE }   
 [ WITH <ddl_trigger_option> [ ,...n ] ]   
{ FOR | AFTER } { event_type [ ,...n ] | event_group }   
AS { sql_statement   
[ ; ] }  
}   
  
<ddl_trigger_option> ::=   
    [ <EXECUTE AS Clause> ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece un desencadenador DML. Los desencadenadores DML tienen como ámbito el esquema de la tabla o la vista donde se crean. *schema**_name* es opcional únicamente si el desencadenador DML y su tabla o vista correspondiente pertenecen al esquema predeterminado. *schema_name* no se puede especificar para los desencadenadores DDL o de inicio de sesión.  
  
 *trigger_name*  
 Es el desencadenador existente que se va a modificar.  
  
 *table* | *view*  
 Es la tabla o la vista en que se ejecuta el desencadenador DML. Especificar el nombre completo de la tabla o vista es opcional.  
  
 DATABASE  
 Aplica el ámbito de un desencadenador DDL a la base de datos actual. Si se especifica, el desencadenador se activa cada vez que *event_type* o *event_group* tienen lugar en la base de datos actual.  
  
 ALL SERVER  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Aplica el ámbito de un desencadenador DDL o logon al servidor actual. Si se especifica, el desencadenador se activa cada vez que *event_type* o *event_group* tienen lugar en cualquier lugar del servidor actual.  
  
 WITH ENCRYPTION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Cifra las entradas sys.syscomments y sys.sql_modules que contienen el texto de la instrucción ALTER TRIGGER. El uso de WITH ENCRYPTION impide que el desencadenador se publique como parte de la replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. WITH ENCRYPTION no se puede especificar para desencadenadores CLR.  
  
> [!NOTE]  
>  Si se crea un desencadenador mediante WITH ENCRYPTION, debe volver a especificarse en la instrucción ALTER TRIGGER para que esta opción se mantenga habilitada.  
  
 EXECUTE AS  
 Especifica el contexto de seguridad en el que se ejecuta el desencadenador. Permite controlar la cuenta de usuario que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza para validar los permisos en los objetos de base de datos a los que hace referencia el desencadenador.  
  
 Para obtener más información, vea [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 NATIVE_COMPILATION  
 Indica que el desencadenador está compilado de forma nativa.  
  
 Esta opción es necesaria para los desencadenadores en tablas optimizadas para memoria.  
  
 SCHEMABINDING  
 Se garantiza que las tablas a las que un desencadenador hace referencia no se pueden quitar o modificar.  
  
 Esta opción es necesaria para desencadenadores en tablas optimizadas para memoria y no se admite en desencadenadores de tablas tradicionales.  
  
 AFTER  
 Especifica que el desencadenador se activa solo después de que se ejecute correctamente la instrucción SQL desencadenadora. También todas las acciones referenciales en cascada y las comprobaciones de restricciones deben ser correctas para que este desencadenador se active.  
  
 AFTER es el valor predeterminado, si se especifica solo la palabra clave FOR.  
  
 Los desencadenadores DML AFTER solo pueden definirse en tablas.  
  
 INSTEAD OF  
 Especifica que el desencadenador DML se ejecuta en vez de la instrucción SQL desencadenadora, por lo que reemplaza las acciones de esta última. INSTEAD OF no se puede especificar para los desencadenadores DDL o logon.  
  
 Como máximo, se puede definir un desencadenador INSTEAD OF por cada instrucción INSERT, UPDATE o DELETE en cada tabla o vista. No obstante, en las vistas es posible definir otras vistas que tengan su propio desencadenador INSTEAD OF.  
  
 Los desencadenadores INSTEAD OF no se pueden utilizar en vistas creadas con WITH CHECK OPTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un error cuando se agrega un desencadenador INSTEAD OF a una vista para la que se especificó WITH CHECK OPTION. El usuario debe quitar esta opción por medio ALTER VIEW antes de definir el desencadenador INSTEAD OF.  
  
 { [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] } | { [INSERT ] [ , ] [ UPDATE ] }  
 Especifica las instrucciones de modificación de datos que, cuando se ejecutan en esta tabla o vista, activan el desencadenador DML. Se debe especificar al menos una opción. En la definición del desencadenador se permite cualquier combinación de éstas, en cualquier orden. Si especifica más de una opción, deben separarse con comas.  
  
 Para los desencadenadores INSTEAD OF, no se permite la opción DELETE en tablas que tengan una relación de integridad referencial que especifica una acción ON DELETE en cascada. Tampoco se permite la opción UPDATE en tablas que tengan una relación referencial que especifique una acción ON UPDATE en cascada. Para obtener más información, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
 *event_type*  
 Es el nombre de un evento de lenguaje [!INCLUDE[tsql](../../includes/tsql-md.md)] que, después de su ejecución, hace que se active un desencadenador DDL. Los eventos válidos para los desencadenadores DDL se enumeran en [Eventos DDL](../../relational-databases/triggers/ddl-events.md).  
  
 *event_group*  
 Es el nombre de un agrupamiento predefinido de eventos de lenguaje de [!INCLUDE[tsql](../../includes/tsql-md.md)]. El desencadenador DDL se activa tras la ejecución de cualquier evento de lenguaje [!INCLUDE[tsql](../../includes/tsql-md.md)] que pertenezca a *event_group*. Los grupos de eventos válidos para los desencadenadores DDL se enumeran en [Grupos de eventos DDL](../../relational-databases/triggers/ddl-event-groups.md). Una vez que ALTER TRIGGER ha terminado de ejecutarse, *event_group* actúa también como una macro al agregar los tipos de eventos que abarca a la vista de catálogo sys.trigger_events.  
  
 NOT FOR REPLICATION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Indica que el desencadenador no debe ejecutarse cuando un Agente de replicación modifica la tabla utilizada en el desencadenador.  
  
 *sql_statement*  
 Son las condiciones y acciones del desencadenador.  
  
 Para los desencadenadores en tablas optimizadas para memoria, el único *sql_statement* permitido en el nivel superior es un bloque ATOMIC. El código T-SQL permitido dentro del bloque ATOMIC está limitado por el código T-SQL permitido dentro de los procedimientos nativos.  
  
 EXTERNAL NAME \<method_specifier>  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica el método de un ensamblado que se enlaza al desencadenador. El método no debe tomar argumentos y debe devolver void. *class_name* debe ser un identificador válido de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y debe existir como clase en el ensamblado con visibilidad de ensamblado. La clase no puede ser anidada.  
  
## <a name="remarks"></a>Observaciones  
 Para más información sobre ALTER TRIGGER, vea la sección Comentarios de [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
> [!NOTE]  
>  Las opciones EXTERNAL_NAME y ON_ALL_SERVER no están disponibles en una base de datos independiente.  
  
## <a name="dml-triggers"></a>Desencadenadores DML  
 ALTER TRIGGER admite vistas que se pueden actualizar manualmente mediante el uso de desencadenadores INSTEAD OF en las tablas y las vistas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplica ALTER TRIGGER de la misma manera para todos los tipos de desencadenadores (AFTER, INSTEAD-OF).  
  
 El primer y último desencadenador AFTER que se ejecuta en una tabla se puede especificar mediante sp_settriggerorder. Solo se pueden especificar un primer desencadenador y un último desencadenador AFTER en una tabla. Si hay otros desencadenadores AFTER en la misma tabla, se ejecutan aleatoriamente.  
  
 Si una instrucción ALTER TRIGGER modifica el primer o último desencadenador, se elimina el primer o último atributo establecido en el desencadenador modificado, y el valor del orden se debe restablecer mediante sp_settriggerorder.  
  
 Un desencadenador AFTER se ejecuta solo después de ejecutar correctamente la instrucción SQL desencadenadora. La ejecución correcta incluye todas las acciones referenciales en cascada y las comprobaciones de restricciones asociadas al objeto actualizado o eliminado. La operación del desencadenador AFTER comprueba los efectos de la instrucción desencadenadora, así como todas las acciones UPDATE y DELETE referenciales en cascada provocadas por la instrucción desencadenadora.  
  
 Cuando una acción DELETE en una tabla de referencia o secundaria es el resultado de una acción CASCADE de una instrucción DELETE de la tabla principal y el desencadenador INSTEAD OF de DELETE está definido en esta tabla secundaria, el desencadenador se pasa por alto y se ejecuta la acción DELETE.  
  
## <a name="ddl-triggers"></a>Desencadenadores DDL  
 A diferencia de los desencadenadores DML, los desencadenadores DDL no tienen como ámbito los esquemas. Por tanto, no se pueden utilizar OBJECT_ID, OBJECT_NAME, OBJECTPROPERTY ni OBJECTPROPERTY(EX) al consultar metadatos acerca de desencadenadores DDL. Utilice en su lugar las vistas de catálogo. Para más información, vea [Obtener información sobre los desencadenadores DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md).  
  
## <a name="logon-triggers"></a>Desencadenadores logon  
 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no admite desencadenadores en eventos de inicio de sesión.  
  
## <a name="permissions"></a>Permisos  
 Para modificar un desencadenador DML se requiere el permiso ALTER en la tabla o vista en la que se define el desencadenador.  
  
 Para alterar un desencadenador DDL definido con ámbito de servidor (ON ALL SERVER) o un desencadenador logon se requiere el permiso CONTROL SERVER en el servidor. Para modificar un desencadenador DDL definido con el ámbito de base de datos (ON DATABASE), se requiere el permiso ALTER ANY DATABASE DDL TRIGGER en la base de datos actual.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea un desencadenador DML en la base de datos AdventureWorks 2012 que imprime para el cliente un mensaje definido por el usuario cuando alguien intenta agregar o cambiar los datos de la tabla `SalesPersonQuotaHistory`. Después, el desencadenador se modifica utilizando `ALTER TRIGGER` para aplicar el desencadenador solo en las actividades `INSERT`. Este desencadenador es útil porque recuerda al usuario que actualiza o inserta filas en esta tabla que debe notificar también al departamento `Compensation` .  
  
```sql  
CREATE TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  

-- Now, change the trigger.  
ALTER TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
AFTER INSERT  
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [Transactions](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [Obtener información acerca de los desencadenadores DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [Obtener información sobre los desencadenadores DDL](../../relational-databases/triggers/get-information-about-ddl-triggers.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)   
 [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
