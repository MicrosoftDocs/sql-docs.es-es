---
description: DBCC CHECKIDENT (Transact-SQL)
title: DBCC CHECKIDENT (Transact-SQL)
ms.custom: ''
ms.date: 03/07/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKIDENT
- DBCC CHECKIDENT
- CHECKIDENT_TSQL
- DBCC_CHECKIDENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checking identity values
- reseeding identity values
- resetting identity values
- identity values [SQL Server]
- identity values [SQL Server], checking
- modifying identity values
- current identity values
- DBCC CHECKIDENT statement
- identity values [SQL Server], reseeding
- reporting current identity values
ms.assetid: 2c00ee51-2062-4e47-8b19-d90f524c6427
author: pmasl
ms.author: umajay
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017 || = azure-sqldw-latest
ms.openlocfilehash: bb5a426514c8ddd9607114f444119429deb58aeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346065"
---
# <a name="dbcc-checkident-transact-sql"></a>DBCC CHECKIDENT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Comprueba el valor de identidad actual de la tabla especificada en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] y, si fuera necesario, lo cambia. También puede utilizar DBCC CHECKIDENT para establecer manualmente un nuevo valor de identidad actual para la columna de identidad.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

DBCC CHECKIDENT
 (
    table_name  
        [, { NORESEED | { RESEED [, new_reseed_value ] } } ]  
)  
[ WITH NO_INFOMSGS ]  
```  

```syntaxsql
-- Syntax for Azure Synapse Analytics
DBCC CHECKIDENT   
 (   
    table_name  
        [RESEED, new_reseed_value ]   
)  
[ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *table_name*  
 Es el nombre de la tabla para la que se va a comprobar el valor de identidad actual. La tabla especificada debe contener una columna de identidad. Los nombres de las tablas deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). Se deben delimitar dos o tres nombres de partes, como "Person.AddressType" o [Person.AddressType].
  
 NORESEED  
 Especifica que el valor de identidad actual no se debe cambiar.  
  
 RESEED  
 Especifica que el valor de identidad actual se debería cambiar.  
  
 *new_reseed_value*  
 Es el nuevo valor que se va a usar como valor de identidad actual de la columna de identidad.  
  
 WITH NO_INFOMSGS  
 Suprime todos los mensajes de información.  
  
## <a name="remarks"></a>Observaciones

 Las correcciones concretas realizadas en el valor de identidad actual dependen de las especificaciones de los parámetros.  
  
|Comando DBCC CHECKIDENT|Corrección o correcciones de identidad realizadas|  
|-----------------------------|---------------------------------------------|  
|DBCC CHECKIDENT ( *table_name*, NORESEED )|No se restablece el valor de identidad actual. DBCC CHECKIDENT devuelve el valor de identidad actual y el valor máximo actual de la columna de identidad. Si los dos valores no coinciden, debe restablecer el valor de identidad para evitar posibles errores o espacios en la secuencia de valores.|  
|DBCC CHECKIDENT ( *table_name* )<br /><br /> or<br /><br /> DBCC CHECKIDENT ( *table_name*, RESEED )|Si el valor de identidad actual de una tabla es menor que el valor de identidad máximo almacenado en la columna de identidad, se restablece con el valor máximo de la columna de identidad. Vea la sección "Excepciones" que aparece más adelante.|  
|DBCC CHECKIDENT ( *table_name*, RESEED, *new_reseed_value* )|El valor de identidad actual se establece en *new_reseed_value*. Si no se han insertado filas en la tabla desde su creación, o si todas las filas se han quitado con la instrucción TRUNCATE TABLE, la primera fila insertada después de ejecutar DBCC CHECKIDENT usa *new_reseed_value* como identidad. Si hay filas en la tabla, o bien si todas las filas se han quitado mediante la instrucción DELETE, la siguiente fila insertada usa el valor *new_reseed_value* y el valor del [incremento actual](../../t-sql/functions/ident-incr-transact-sql.md). Si una transacción inserta una fila y posteriormente se revierte, la siguiente fila insertada usa *new_reseed_value* más el valor de [incremento actual](../../t-sql/functions/ident-incr-transact-sql.md) como si se hubiera eliminado la fila. Si la tabla no está vacía y se establece el valor de identidad en un número menor que el valor máximo de la columna de identidad, puede darse una de las siguientes condiciones:<br /><br /> \- Si existe una restricción PRIMARY KEY o UNIQUE en la columna de identidad, se generará el mensaje de error 2627 en las operaciones de inserción en la tabla posteriores, ya que el valor de identidad generado provocará un conflicto con los valores existentes.<br /><br /> \- Si no existe una restricción PRIMARY KEY o UNIQUE, las operaciones de inserción posteriores provocarán la duplicación de los valores de identidad.|  
  
## <a name="exceptions"></a>Excepciones

 En la tabla siguiente se muestran condiciones en las que DBCC CHECKIDENT no restablece automáticamente el valor de identidad actual y se proporcionan métodos para restablecer el valor.  
  
|Condición|Métodos para restablecer|  
|---------------|-------------------|  
|El valor de identidad actual es mayor que el valor máximo de la tabla.|Ejecute DBCC CHECKIDENT (*table_name*, NORESEED) para determinar el valor máximo actual de la columna. A continuación, especifique ese valor como *new_reseed_value* en un comando DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*).<br /><br /> O<br /><br /> Ejecute DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) con *new_reseed_value* establecido en un valor muy bajo y, luego, ejecute DBCC CHECKIDENT (*table_name*, RESEED) para corregir el valor.|  
|Se eliminan todas las filas de la tabla.|Ejecute DBCC CHECKIDENT (*table_name*, RESEED,*new_reseed_value*) con *new_reseed_value* establecido en el valor de inicio deseado.|  
  
## <a name="changing-the-seed-value"></a>Cambiar el valor de inicialización

 El valor de inicialización es el valor insertado en una columna de identidad para la primera fila cargada en la tabla. Todas las filas subsiguientes contienen el valor de identidad actual más el valor de incremento, donde el valor de identidad actual es el último valor de identidad generado para la tabla o vista.  
  
 No se puede usar DBCC CHECKIDENT en las siguientes tareas:  
  
- Cambiar el valor de inicialización original especificado para una columna de identidad cuando se creó la tabla o la vista.  
  
- Reinicializar las filas existentes de una tabla o vista.  
  
 Para cambiar el valor de inicialización original y reinicializar cualquier fila existente, quite la columna de identidad y vuelva a crearla mediante la especificación del nuevo valor de inicialización. Cuando la tabla contiene datos, los números de identidad se agregan a las filas existentes con los valores de inicialización e incremento especificados. No se garantiza el orden en que las filas se actualizan.  
  
## <a name="result-sets"></a>Conjuntos de resultados

 Tanto si especifica una opción para una tabla que contiene una columna de identidad como si no, DBCC CHECKIDENT devuelve el siguiente mensaje para todas las operaciones excepto una. Esa operación es especificar un nuevo valor de inicialización.  
  
`Checking identity information: current identity value '\<current identity value>', current column value '\<current column value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
 Cuando DBCC CHECKIDENT se usa para especificar un nuevo valor de inicialización mediante RESEED *new_reseed_value*, se devuelve el mensaje siguiente.  
  
`Checking identity information: current identity value '\<current identity value>'. DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
## <a name="permissions"></a>Permisos

 El autor de la llamada debe ser el propietario del esquema que contiene la tabla, o bien ser miembro del rol fijo de servidor **sysadmin**, el rol fijo de base de datos **db_owner** o el rol fijo de base de datos **db_ddladmin**.

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Requiere permisos de **db_owner**.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-resetting-the-current-identity-value-if-its-needed"></a>A. Restablecer el valor de identidad actual si es necesario  
 En el ejemplo siguiente, se restablece el valor de identidad actual, si es necesario, de la tabla especificada en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType');  
GO  
```  
  
### <a name="b-reporting-the-current-identity-value"></a>B. Informar del valor de identidad actual

 En el ejemplo siguiente se notifica el valor de identidad actual de la tabla especificada en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], y no se corrige el valor de identidad si es incorrecto.  
  
```sql
USE AdventureWorks2012;
GO  
DBCC CHECKIDENT ('Person.AddressType', NORESEED);
GO  
```  
  
### <a name="c-forcing-the-current-identity-value-to-a-new-value"></a>C. Hacer que el valor de identidad actual sea un nuevo valor  
 En el ejemplo siguiente, el valor de identidad actual de la columna `AddressTypeID` de la tabla `AddressType` se establece en el valor 10. Dado que la tabla ya contiene filas, la fila siguiente que se inserte usará el valor 11; es decir, el valor de identidad actual definido para la columna más 1 (que es el valor de incremento de la columna).  

```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKIDENT ('Person.AddressType', RESEED, 10);  
GO  
```

### <a name="d-resetting-the-identity-value-on-an-empty-table"></a>D. Restablecer el valor de identidad en una tabla vacía

 En el ejemplo siguiente, el valor de identidad actual de la columna `ErrorLogID` de la tabla `ErrorLog` se establece en el valor 1 después de eliminar todos los registros de la tabla. Puesto que la tabla no tiene ninguna fila, la siguiente fila insertada usará 1 como el valor, es decir, el nuevo valor de identidad actual, sin añadir el valor de incremento definido para la columna.  
  
```sql
USE AdventureWorks2012;  
GO  
TRUNCATE TABLE dbo.ErrorLog
GO
DBCC CHECKIDENT ('dbo.ErrorLog', RESEED, 1);  
GO  
```  
  
## <a name="see-also"></a>Consulte también

[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[IDENTITY &#40;propiedad de Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
[Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md)  
[USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
[IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)  
[IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)  
