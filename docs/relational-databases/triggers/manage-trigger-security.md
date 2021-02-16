---
description: Administrar la seguridad de los desencadenadores
title: Administración de la seguridad de los desencadenadores | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], security
ms.assetid: e94720a8-a3a2-4364-b0a3-bbe86e3ce4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 931ba1487b8212c6c088b044df5e0b9fa30b5ef1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347589"
---
# <a name="manage-trigger-security"></a>Administrar la seguridad de los desencadenadores

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

De forma predeterminada, los desencadenadores DML y DDL se ejecutan en el contexto del usuario que llama al desencadenador. El autor de la llamada a un desencadenador es el usuario que ejecuta la instrucción que hace que el desencadenador se ejecute. Por ejemplo, si el usuario **Mary** ejecuta una instrucción DELETE que hace que el desencadenador DML **DML_trigMary** se ejecute, el código incluido en **DML_trigMary** se ejecutará en el contexto de los privilegios de usuario de **Mary**. Este comportamiento predeterminado podría ser utilizado por usuarios que deseen introducir código dañino en la base de datos o la instancia de servidor. Por ejemplo, el usuario **JohnDoe** crea el siguiente desencadenador DDL:  

```sql
CREATE TRIGGER DDL_trigJohnDoe
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON;

BEGIN TRY
  EXEC(N'
    USE [master];
    GRANT CONTROL SERVER TO [JohnDoe];
');
END TRY
BEGIN CATCH
  DECLARE @DoNothing INT;
END CATCH;
GO
```

Este desencadenador significa que cuando un usuario con permiso para ejecutar una instrucción `GRANT CONTROL SERVER`, por ejemplo, un miembro del rol fijo de servidor **sysadmin**, ejecute una instrucción `ALTER TABLE`, **JohnDoe** obtendrá el permiso `CONTROL SERVER`. En otras palabras, aunque **JohnDoe** no puede otorgarse el permiso `CONTROL SERVER` a sí mismo, ha habilitado el código de desencadenador que le otorga permiso para ejecutar instrucciones con privilegios aumentados. Tanto el desencadenador DML como el DLL están sujetos a este tipo de amenaza de seguridad.  
  
## <a name="trigger-security-best-practices"></a>Prácticas recomendadas de seguridad de los desencadenadores  
 Para evitar que se ejecute código de desencadenador con privilegios aumentados, puede adoptar las siguientes medidas:  
  
::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"

-   Tenga en cuenta los desencadenadores DML y DDL que se encuentran en la base de datos y en la instancia del servidor al consultar la vistas de catálogo [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) y [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md) . La consulta siguiente devuelve todos los desencadenadores DML y DLL de base de datos para la base de datos actual, además de todos los desencadenadores DDL de servidor para la instancia de servidor:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers
    UNION ALL
    SELECT type, name, parent_class_desc FROM sys.server_triggers;
    ```  

   > [!NOTE]
   > Solo **sys.triggers** está disponible para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], a menos que se esté usando [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)].

::: moniker-end

::: moniker range="=azuresqldb-current"

-   Sepa cuáles son los desencadenadores DML y DDL que existen en la base de datos mediante una consulta a la vista del catálogo [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). La consulta siguiente devuelve todos los desencadenadores DDL de nivel de base de datos y DML de la base de datos actual:  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers;
    ```  
  
::: moniker-end

-   Utilice [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) para deshabilitar desencadenadores que podrían dañar la integridad de la base de datos o el servidor si se ejecutan con privilegios aumentados. La siguiente instrucción deshabilita todos los desencadenadores DDL de base de datos para la base de datos actual:  
  
    ```sql
    DISABLE TRIGGER ALL ON DATABASE;
    ```  
  
     Esta instrucción deshabilita todos los desencadenadores DDL en la instancia de servidor:  
  
    ```sql
    DISABLE TRIGGER ALL ON ALL SERVER;
    ```  
  
     Esta instrucción deshabilita todos los desencadenadores DML en la base de datos actual:  
  
    ```sql
    DECLARE @schema_name sysname, @trigger_name sysname, @object_name sysname;
    DECLARE @sql nvarchar(max);
    DECLARE trig_cur CURSOR FORWARD_ONLY READ_ONLY FOR
        SELECT SCHEMA_NAME(schema_id) AS schema_name,
            name AS trigger_name,
            OBJECT_NAME(parent_object_id) AS object_name
        FROM sys.objects WHERE type IN ('TR', 'TA');

    OPEN trig_cur;
    FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
  
    WHILE @@FETCH_STATUS = 0
    BEGIN
        SELECT @sql = N'DISABLE TRIGGER ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@trigger_name)
            + N' ON ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@object_name) + N'; ';
        EXEC (@sql);
        FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
    END;
    GO

    -- Verify triggers are disabled. Should return an empty result set.
    SELECT * FROM sys.triggers WHERE is_disabled = 0;
    GO

    CLOSE trig_cur;
    DEALLOCATE trig_cur;
    ```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Desencadenadores DML](../../relational-databases/triggers/dml-triggers.md)   
 [Desencadenadores DDL](../../relational-databases/triggers/ddl-triggers.md)  
 [Desencadenadores logon](../../relational-databases/triggers/logon-triggers.md)  
  
