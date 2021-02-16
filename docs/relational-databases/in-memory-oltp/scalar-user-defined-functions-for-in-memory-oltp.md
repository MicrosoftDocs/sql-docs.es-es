---
title: Funciones escalares definidas por el usuario para OLTP en memoria | Microsoft Docs
description: Obtenga información sobre cómo crear y quitar funciones escalares definidas por el usuario y compiladas de forma nativa para OLTP en memoria en SQL Server. La compilación nativa mejora el rendimiento.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: d2546e40-fdfc-414b-8196-76ed1f124bf5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ed83ee45844bd7299ca58eeeeed95849d0399f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353661"
---
# <a name="scalar-user-defined-functions-for-in-memory-oltp"></a>Funciones escalares definidas por el usuario para OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear y eliminar funciones escalares definidas por el usuario y compiladas de forma nativa. También puede modificar estas funciones definidas por el usuario. La compilación nativa mejora el rendimiento de la evaluación de funciones definidas por el usuario en Transact-SQL.  
  
 Cuando se modifica una función escalar definida por el usuario y compilada de forma nativa, la aplicación sigue estando disponible mientras se está ejecutando la operación y compilando la nueva versión.  
  
 Para saber qué construcciones T-SQL se admiten, vea [Características admitidas en los módulos T-SQL compilados de forma nativa](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="creating-dropping-and-altering-user-defined-functions"></a>Creación, eliminación y modificación de funciones definidas por el usuario  
 Utilice CREATE FUNCTION para crear la función escalar definida por el usuario y compilada de forma nativa, DROP FUNCTION para quitar la función definida por el usuario y ALTER FUNCTION para cambiarla. El bloque BEGIN ATOMIC WITH es necesario para las funciones definidas por el usuario.  
  
 Para obtener información sobre la sintaxis admitida y las restricciones, consulte los temas siguientes.  
  
-   [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
  
-   [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)  
  
-   [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)  
  
     La sintaxis de DROP FUNCTION para las funciones escalares definidas por el usuario y compiladas de forma nativa es la misma que para las funciones definidas por el usuario interpretadas.  
  
-   [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
 El procedimiento almacenado [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) se puede usar con la función escalar definida por el usuario y compilada de forma nativa. Se creará la función que se va a volver a compilar con la definición que hay en los metadatos.  
  
 En el ejemplo siguiente se muestra una UDF escalar de la base de datos de muestra [AdventureWorks2016CTP3](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) .  
  
```sql  
CREATE FUNCTION [dbo].[ufnLeadingZeros_native](@Value int)   
RETURNS varchar(8)   
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')  
  
    DECLARE @ReturnValue varchar(8);  
    SET @ReturnValue = CONVERT(varchar(8), @Value);  
       DECLARE @i int = 0, @count int = 8 - LEN(@ReturnValue)  
  
    WHILE @i < @count  
       BEGIN  
            SET @ReturnValue = '0' + @ReturnValue;  
            SET @i += 1  
       END  
  
    RETURN (@ReturnValue);  
  
END  
```  
  
## <a name="calling-user-defined-functions"></a>Llamar a funciones definidas por el usuario  
 Las funciones escalares definidas por el usuario y compiladas de forma nativa pueden utilizarse en las expresiones, en el mismo lugar que las funciones escalares integradas y las escalares definidas por el usuario interpretadas. Las funciones escalares definidas por el usuario y compiladas de forma nativa pueden utilizarse también con la instrucción EXECUTE, en una instrucción Transact-SQL y en un procedimiento almacenado compilado de forma nativa.  
  
 Puede utilizar estas funciones escalares definidas por el usuario en procedimientos almacenados compilados de forma nativa y en funciones definidas por el usuario compiladas de forma nativa, siempre que se permitan funciones integradas. También puede utilizar las funciones escalares definidas por el usuario y compiladas de forma nativa en los módulos de Transact-SQL tradicionales.  
  
 Puede usar estas funciones escalares definidas por el usuario en el modo de interoperabilidad, siempre que puede usarse una función escalar definida por el usuario interpretada. Este uso está sujeto a limitaciones de transacción entre contenedores, tal y como se describe en la sección sobre **niveles de aislamiento admitidos para transacciones entre contenedores** del tema [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)(Transacciones con tablas con optimización para memoria). Para obtener más información sobre el modo de interoperabilidad, vea [Acceso a tablas con optimización para memoria mediante Transact-SQL interpretado](../../relational-databases/in-memory-oltp/accessing-memory-optimized-tables-using-interpreted-transact-sql.md).  
  
 Las funciones escalares definidas por el usuario y compiladas de forma nativa requieren un contexto de ejecución explícita. Para obtener más información, vea [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md). No se admite EXECUTE AS CALLER. Para obtener más información, vea [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Para conocer la sintaxis compatible con las instrucciones Transact-SQL Execute para funciones escalares definidas por el usuario y compiladas de forma nativa, vea [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md). Para conocer la sintaxis compatible con la ejecución de las funciones definidas por el usuario en un procedimiento almacenado compilado de forma nativa, vea [Características admitidas en los módulos T-SQL compilados de forma nativa](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="hints-and-parameters"></a>Sugerencias y parámetros  
 La compatibilidad con las sugerencias de tablas, combinaciones y consultas de las funciones escalares definidas por el usuario y compiladas de forma nativa es la misma que la de estas sugerencias para procedimientos almacenados compilados de forma nativa. Al igual que con las funciones escalares definidas por el usuario, las sugerencias de consulta incluidas en una consulta Transact-SQL que hace referencia a una función escalar definida por el usuario y compilada de forma nativa no afectan el plan de consulta de esta función definida por el usuario.  
  
 Los parámetros compatibles con las funciones escalares definidas por el usuario y compiladas de forma nativa son aquellos que se admiten en los procedimientos almacenados compilados de forma nativa, siempre que se permitan los parámetros en las funciones escalares definidas por el usuario. Un ejemplo de un parámetro compatible es el parámetro con valores de tabla.  
  
## <a name="schema-bound"></a>Enlazadas al esquema  
 La siguiente información se aplica a las funciones escalares definidas por el usuario y compiladas de forma nativa.  
  
-   Deben estar enlazadas al esquema mediante el argumento WITH SCHEMABINDING en CREATE FUNCTION y ALTER FUNCTION.  
  
-   No se pueden quitar ni modificar cuando se hacen referencia a estas mediante una función definida por el usuario o un procedimiento almacenado enlazados al esquema.  
  
## <a name="showplan_xml"></a>SHOWPLAN_XML  
 Las funciones escalares definidas por el usuario y compiladas de forma nativa admiten SHOWPLAN_XML. Siguen las especificaciones del esquema general SHOWPLAN_XML, al igual que con los procedimientos almacenados compilados de forma nativa. El elemento base de las funciones definidas por el usuario es `<UDF>`.  
  
 STATISTICS XML no es compatible con las funciones escalares definidas por el usuario y compiladas de forma nativa. Al ejecutar una consulta que hace referencia a la función definida por el usuario, con la opción STATISTICS XML habilitada, se devuelve el contenido XML sin la parte de la función definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Al igual que con los procedimientos almacenados compilados de forma nativa, se comprueban los permisos de los objetos a los que se hacen referencia desde una función escalar definida por el usuario y compilada de forma nativa cuando se crea la función. CREATE FUNCTION no funciona correctamente si el usuario suplantado no tiene los permisos correctos. Si como resultado de los cambios en los permisos, el usuario suplantado no vuelve a tener los permisos correctos, se producirá un error en las ejecuciones posteriores de la función definida por el usuario.  
  
 Cuando se utiliza una función escalar definida por el usuario y compilada de forma nativa dentro de un procedimiento almacenado compilado de forma nativa, se comprueban los permisos para ejecutar la función definida por el usuario cuando se crea el procedimiento externo. Si el usuario suplantado por el procedimiento externo no tiene permisos de ejecución de la función definida por el usuario, se produce un error en la creación del procedimiento almacenado. Si debido a cambios en los permisos, el usuario ya no vuelve a tener permisos de ejecución, se producirá un error en la ejecución del procedimiento externo.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Guardar un plan de ejecución en formato XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
