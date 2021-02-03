---
description: SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
title: SET TRANSACTION ISOLATION LEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEVEL
- LEVEL_TSQL
- SET TRANSACTION ISOLATION LEVEL
- ISOLATION
- ISOLATION_TSQL
- SET_TRANSACTION_ISOLATION_LEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET TRANSACTION ISOLATION LEVEL statement
- row versioning [SQL Server], isolation levels
- TRANSACTION ISOLATION LEVEL option
- isolation levels [SQL Server], setting
- locking [SQL Server], isolation levels
- transactions [SQL Server], isolation levels
ms.assetid: 016fb05e-a702-484b-bd2a-a6eabd0d76fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7fd0098e9b4e213619e65973c2367a03f0197852
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161102"
---
# <a name="set-transaction-isolation-level-transact-sql"></a>SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Controla el comportamiento del bloqueo y de las versiones de fila de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] emitidas por una conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET TRANSACTION ISOLATION LEVEL
    { READ UNCOMMITTED
    | READ COMMITTED
    | REPEATABLE READ
    | SNAPSHOT
    | SERIALIZABLE
    }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse
  
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

>[!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] implementa transacciones ACID. El nivel de aislamiento de la compatibilidad transaccional se establece de forma predeterminada en READ UNCOMMITTED.  Para cambiarlo a READ COMMITTED SNAPSHOT ISOLATION, active la opción de base de datos READ_COMMITTED_SNAPSHOT de una base de datos de usuario cuando se conecte a la base de datos maestra.  Una vez habilitada, todas las transacciones de esta base de datos se ejecutan bajo READ COMMITTED SNAPSHOT ISOLATION y no se respetará la opción de configuración READ UNCOMMITTED en el nivel de sesión. Consulte [Opciones de ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md) para obtener más información.  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 READ UNCOMMITTED  
 Especifica que las instrucciones pueden leer filas que han sido modificadas por otras transacciones pero todavía no se han confirmado.  
  
 Las transacciones que se ejecutan en el nivel READ UNCOMMITTED no emiten bloqueos compartidos para impedir que otras transacciones modifiquen los datos leídos por la transacción actual. Las transacciones READ UNCOMMITTED tampoco se bloquean mediante bloqueos exclusivos que impedirían que la transacción actual leyese las filas modificadas pero no confirmadas por otras transacciones. Cuando se establece esta opción, es posible leer las modificaciones no confirmadas, denominadas lecturas de datos sucios. Los valores de los datos se pueden cambiar, y las filas pueden aparecer o desaparecer en el conjunto de datos antes de que finalice la transacción. Esta opción tiene el mismo efecto que establecer NOLOCK en todas las tablas y en todas las instrucciones SELECT de una transacción. Se trata del nivel de aislamiento menos restrictivo.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], también se puede reducir al mínimo la contención de bloqueos y, al mismo tiempo, proteger las transacciones de las lecturas de datos sucios de modificaciones de datos no confirmadas mediante una de estas dos alternativas:  
  
-   El nivel de aislamiento READ COMMITTED con la opción de base de datos READ_COMMITTED_SNAPSHOT establecida en ON.  
  
-   El nivel de aislamiento SNAPSHOT. Para obtener más información sobre el aislamiento de instantáneas, vea [Aislamiento de instantáneas en SQL Server](/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server). 
  
 READ COMMITTED  
 Especifica que las instrucciones no pueden leer datos que hayan sido modificados, pero no confirmados, por otras transacciones. Esto evita las lecturas de datos sucios. Otras transacciones pueden cambiar datos entre cada una de las instrucciones de la transacción actual, dando como resultado lecturas no repetibles o datos fantasma. Esta opción es la predeterminada para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 El comportamiento de READ COMMITTED depende del valor de la opción de base de datos READ_COMMITTED_SNAPSHOT:  
  
-   Si READ_COMMITTED_SNAPSHOT se establece en OFF (el valor predeterminado de SQL Server), el [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa bloqueos compartidos para impedir que otras transacciones modifiquen las filas mientras la transacción actual esté ejecutando una operación de lectura. Los bloqueos compartidos impiden también que la instrucción lea las filas modificadas por otras transacciones hasta que la otra transacción haya finalizado. El tipo de bloqueo compartido determina cuándo se liberará. Los bloqueos de fila se liberan antes de que se procese la fila siguiente. Los bloqueos de página se liberan cuando se lee la página siguiente, y los bloqueos de tabla se liberan cuando la instrucción finaliza.  
  
-   Si READ_COMMITTED_SNAPSHOT se establece en ON (el valor predeterminado de Azure SQL Database), el [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa versiones de fila para presentar a cada instrucción una instantánea coherente, desde el punto de vista transaccional, de los datos tal como se encontraban al comenzar la instrucción. No se emplean bloqueos para impedir que otras transacciones actualicen los datos.

> [!IMPORTANT]  
> La elección de un nivel de aislamiento de transacción no afecta a los bloqueos adquiridos para proteger la modificación de datos. Siempre se obtiene un bloqueo exclusivo en los datos modificados de una transacción, bloqueo que se mantiene hasta que se completa la transacción, independientemente del nivel de aislamiento seleccionado para la misma. Además, una actualización realizada en el nivel de aislamiento READ_COMMITTED usa bloqueos de actualización en las filas de datos seleccionadas, mientras que una actualización realizada en el nivel de aislamiento SNAPSHOT emplea versiones de fila para seleccionar filas para actualizar. En el caso de las operaciones de lectura, los niveles de aislamiento de transacción definen básicamente el nivel de protección contra los efectos de las modificaciones que realizan otras transacciones. Vea [Guía de versiones de fila y bloqueo de transacciones](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md) para obtener más información.

> [!NOTE]  
>  El aislamiento de instantánea admite datos FILESTREAM. En el modo de aislamiento de instantánea, los datos FILESTREAM leídos por cualquier instrucción de una transacción serán la versión coherente, desde el punto de vista transaccional, de los datos existentes al comienzo de la transacción.  
  
 Cuando la opción de base de datos READ_COMMITTED_SNAPSHOT es ON, se puede usar la sugerencia de tabla READCOMMITTEDLOCK para solicitar el uso del bloqueo compartido en lugar de versiones de fila para las instrucciones individuales de las transacciones que se ejecutan en el nivel de aislamiento READ COMMITTED.  
  
> [!NOTE]  
>  Al establecer la opción READ_COMMITTED_SNAPSHOT, solo se permite en la base de datos la conexión que ejecuta el comando ALTER DATABASE. No debe haber ninguna otra conexión de base de datos abierta hasta que ALTER DATABASE haya finalizado. La base de datos no tiene que estar en modo de usuario único.  
  
 REPEATABLE READ  
 Especifica que las instrucciones no pueden leer datos que han sido modificados pero aún no confirmados por otras transacciones y que ninguna otra transacción puede modificar los datos leídos por la transacción actual hasta que ésta finalice.  
  
 Se aplican bloqueos compartidos a todos los datos leídos por cada instrucción de la transacción, y se mantienen hasta que la transacción finaliza. De esta forma, se evita que otras transacciones modifiquen las filas que han sido leídas por la transacción actual. Otras transacciones pueden insertar filas nuevas que coincidan con las condiciones de búsqueda de las instrucciones emitidas por la transacción actual. Si la transacción actual vuelve a ejecutar la instrucción, recuperará las filas nuevas, dando como resultado lecturas fantasma. Debido a que los bloqueos compartidos se mantienen hasta el final de la transacción en lugar de liberarse al final de cada instrucción, la simultaneidad es inferior que en el nivel de aislamiento predeterminado READ COMMITTED. Utilice esta opción solamente cuando sea necesario.  
  
 SNAPSHOT  
 Especifica que los datos leídos por cualquier instrucción de una transacción serán la versión coherente, desde el punto de vista transaccional, de los datos existentes al inicio de la transacción. La transacción únicamente puede reconocer las modificaciones de datos confirmadas antes del comienzo de la misma. Las instrucciones que se ejecuten en la transacción actual no verán las modificaciones de datos efectuadas por otras transacciones después del inicio de la transacción actual. El efecto es como si las instrucciones de una transacción obtienen una instantánea de los datos confirmados tal como se encontraban al inicio de la transacción.  
  
 Las transacciones SNAPSHOT no solicitan bloqueos al leer los datos, excepto cuando se recupera una base de datos. Las transacciones SNAPSHOT que leen datos no bloquean la escritura de datos de otras transacciones. Las transacciones que escriben datos no bloquean la lectura de datos de las transacciones SNAPSHOT.  
  
 Durante la fase de reversión de la recuperación de una base de datos, las transacciones SNAPSHOT solicitan un bloqueo si se intenta leer datos bloqueados por otra transacción que está en proceso de reversión. La transacción SNAPSHOT se bloquea hasta que finalice la reversión de esa transacción. El bloqueo se libera justo después de haberse concedido.  
  
 La opción de base de datos ALLOW_SNAPSHOT_ISOLATION debe establecerse en ON para poder iniciar una transacción que utilice el nivel de aislamiento SNAPSHOT. Si una transacción que utiliza el nivel de aislamiento SNAPSHOT obtiene acceso a datos de varias bases de datos, será necesario establecer ALLOW_SNAPSHOT_ISOLATION en ON en cada una de ellas.  
  
 No es posible establecer en el nivel de aislamiento SNAPSHOT una transacción que se inició con otro nivel de aislamiento; si lo hace, la cancelará. Si una transacción comienza en el nivel de aislamiento SNAPSHOT, puede cambiarla a otro nivel de aislamiento y, después, de nuevo a SNAPSHOT. Una transacción se inicia la primera vez que obtiene acceso a los datos.  
  
 Una transacción que se ejecuta en el nivel de aislamiento SNAPSHOT puede ver los cambios realizados por esa transacción. Por ejemplo, si la transacción realiza una operación UPDATE en una tabla y después emite una instrucción SELECT para la misma tabla, los datos modificados se incluirán en el conjunto de resultados.  
  
> [!NOTE]  
>  En el modo de aislamiento de instantánea, los datos FILESTREAM leídos por cualquier instrucción de una transacción serán la versión coherente, desde el punto de vista transaccional, de los datos existentes al comienzo de la transacción, no al comienzo de la instrucción.  
  
 SERIALIZABLE  
 Especifica lo siguiente:  
  
-   Las instrucciones no pueden leer datos que hayan sido modificados, pero aún no confirmados, por otras transacciones.  
  
-   Ninguna otra transacción puede modificar los datos leídos por la transacción actual hasta que la transacción actual finalice.  
  
-   Otras transacciones no pueden insertar filas nuevas con valores de clave que pudieran estar incluidos en el intervalo de claves leído por las instrucciones de la transacción actual hasta que ésta finalice.  
  
 Se colocan bloqueos de intervalo en el intervalo de valores de clave que coincidan con las condiciones de búsqueda de cada instrucción ejecutada en una transacción. De esta manera, se impide que otras transacciones actualicen o inserten filas que satisfagan los requisitos de alguna de las instrucciones ejecutadas por la transacción actual. Esto significa que, si alguna de las instrucciones de una transacción se ejecuta por segunda vez, leerá el mismo conjunto de filas. Los bloqueos de intervalo se mantienen hasta que la transacción finaliza. Este es el nivel de aislamiento más restrictivo, porque bloquea intervalos de claves completos y mantiene esos bloqueos hasta que la transacción finaliza. Al ser menor la simultaneidad, solo se debe utilizar esta opción cuando sea necesario. Esta opción tiene el mismo efecto que establecer HOLDLOCK en todas las tablas de todas las instrucciones SELECT de la transacción.  
  
## <a name="remarks"></a>Observaciones  
 Solo es posible establecer una de las opciones de nivel de aislamiento cada vez, y permanecerá activa para la conexión hasta que se cambie explícitamente. Todas las operaciones de lectura realizadas dentro de la transacción se rigen por las reglas del nivel de aislamiento especificado, a menos que se utilice una sugerencia de tabla en la cláusula FROM de una instrucción para especificar un comportamiento de bloqueo o versiones diferente para una tabla.  
  
 Los niveles de aislamiento de transacciones definen el tipo de bloqueo que se adquiere en las operaciones de lectura. Los bloqueos compartidos que se adquieren para READ COMMITTED o REPEATABLE READ suelen ser bloqueos de fila, aunque éstos se pueden escalar a bloqueos de página o tabla si la operación de lectura hace referencia a un número significativo de filas de una página o tabla. Si la transacción modifica una fila después de haberse leído, la transacción adquiere un bloqueo exclusivo para proteger esa fila, y ese bloqueo exclusivo se mantiene hasta que la transacción finaliza. Por ejemplo, si una transacción REPEATABLE READ tiene un bloqueo compartido en una fila y, después, la transacción modifica esa fila, el bloqueo compartido de fila se convierte en un bloqueo exclusivo de fila.  
  
 Con una excepción, se puede cambiar de un nivel de aislamiento a otro en cualquier momento de una transacción. La excepción se produce cuando se cambia de cualquier nivel de aislamiento al aislamiento SNAPSHOT. Esta acción generará un error en la transacción y hará que se revierta. Sin embargo, puede cambiar una transacción iniciada en aislamiento SNAPSHOT a cualquier otro nivel de aislamiento.  
  
 Cuando se cambia el nivel de aislamiento de una transacción por otro, los recursos leídos después del cambio se protegen de acuerdo con las reglas del nuevo nivel. Los recursos leídos antes del cambio siguen estando protegidos en función de las reglas del nivel anterior. Por ejemplo, si una transacción ha cambiado de READ COMMITTED a SERIALIZABLE, los bloqueos compartidos adquiridos después del cambio se mantienen hasta el final de la transacción.  
  
 Si se ejecuta SET TRANSACTION ISOLATION LEVEL en un procedimiento almacenado o un desencadenador, cuando el objeto devuelve el control, el nivel de aislamiento se restablece en el nivel en efecto cuando se invocó el objeto. Por ejemplo, si se establece REPEATABLE READ en un lote y, después, este lote llama a un procedimiento almacenado que establece el nivel de aislamiento en SERIALIZABLE, el valor del nivel de aislamiento vuelve a REPEATABLE READ cuando el procedimiento almacenado devuelve el control al lote.  
  
> [!NOTE]  
>  Las funciones definidas por el usuario y los tipos definidos por el usuario para CLR (Common Language Runtime) no pueden ejecutar SET TRANSACTION ISOLATION LEVEL. Sin embargo, se puede anular este nivel de aislamiento mediante una sugerencia de tabla. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Cuando se utiliza sp_bindsession para enlazar dos sesiones, cada sesión mantiene su nivel de aislamiento. Si se utiliza SET TRANSACTION ISOLATION LEVEL para cambiar el valor del nivel de aislamiento de una sesión, no se verán afectados los valores de las sesiones enlazadas a ella.  
  
 SET TRANSACTION ISOLATION LEVEL se aplica en tiempo de ejecución, no en tiempo de análisis.  
  
 Las operaciones de carga masiva optimizadas que se realizan en montones bloquean las consultas que se ejecutan con los siguientes niveles de aislamiento:  
  
-   SNAPSHOT  
  
-   READ UNCOMMITTED  
  
-   READ COMMITTED con versiones de fila  
  
 A la inversa, las consultas que se ejecutan con estos niveles de aislamiento bloquean las operaciones de carga masiva optimizadas que se realizan en montones: Para más información sobre operaciones de carga masiva, vea [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
 Las bases de datos habilitadas con FILESTREAM admiten los niveles de aislamiento de transacción siguientes:  
  
|Nivel de aislamiento|Acceso Transact SQL|Acceso al sistema de archivos|  
|---------------------|-------------------------|------------------------|  
|Lectura no confirmada|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|No compatible|  
|Lectura confirmada|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Lectura repetible|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|No compatible|  
|Serializable|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|No compatible|  
|Instantánea de lectura confirmada|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|Instantánea|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establece `TRANSACTION ISOLATION LEVEL` para la sesión. En cada instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] siguiente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantendrá todos los bloqueos compartidos hasta el final de la transacción.  
  
```sql
USE AdventureWorks2012;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT *   
    FROM HumanResources.EmployeePayHistory;  
GO  
SELECT *   
    FROM HumanResources.Department;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC USEROPTIONS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)  
  
