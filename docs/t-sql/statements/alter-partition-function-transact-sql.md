---
description: ALTER PARTITION FUNCTION (Transact-SQL)
title: ALTER PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER PARTITION FUNCTION
- ALTER_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- splitting partitions [SQL Server]
- partitioned tables [SQL Server], splitting
- ALTER PARTITION FUNCTION statement
- merging partitions [SQL Server]
- partitioned indexes [SQL Server], merging
- partitioned indexes [SQL Server], splitting
- modifying partition functions
- partition functions [SQL Server], modifying
- partitioned tables [SQL Server], merging
ms.assetid: 70866dac-0a8f-4235-8108-51547949ada4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8358d1fa100458cdd6a383d46e2550d146ca8741
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204121"
---
# <a name="alter-partition-function-transact-sql"></a>ALTER PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Modifica una función de partición dividiendo o mezclando sus valores de límite. Al ejecutar una instrucción ALTER PARTITION FUNCTION, se puede dividir un índice o una partición de tabla que use la función de partición en dos particiones. La instrucción también puede combinar dos particiones en una partición menos.  
  
> [!CAUTION]  
>  Varias tablas o índices pueden utilizar la misma función de partición. ALTER PARTITION FUNCTION afecta a todas ellas en una única transacción.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
ALTER PARTITION FUNCTION partition_function_name()  
{   
    SPLIT RANGE ( boundary_value )  
  | MERGE RANGE ( boundary_value )   
} [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*partition_function_name*  
Es el nombre de la función de partición que se va a modificar.  
  
SPLIT RANGE ( *boundary_value* )  
Agrega una partición a la función de partición. *boundary_value* determina el intervalo de la nueva partición y debe ser distinto de los intervalos de límites existentes de la función de partición. [!INCLUDE[ssDE](../../includes/ssde-md.md)] divide en dos uno de los intervalos existentes, basándose en *boundary_value*. De estos dos rangos, el que tiene el valor *boundary_value* es la nueva partición.  
  
Debe haber un grupo de archivos en línea y estar marcado por el esquema de partición que utiliza la función de partición como NEXT USED para contener la nueva partición. La instrucción CREATE PARTITION SCHEME asigna grupos de archivos a particiones. La instrucción CREATE PARTITION FUNCTION crea menos particiones que grupos de archivos para almacenarlos. Una instrucción CREATE PARTITION SCHEME puede reservar más grupos de archivos que los necesarios. Si esto sucede, habrá grupos de archivos sin asignar. Además, el esquema de partición marca uno de los grupos de archivos como NEXT USED. Este grupo de archivos almacena la partición nueva. Si no hay ningún grupo de archivos que el esquema de partición marca como NEXT USED, debe usar una instrucción ALTER PARTITION SCHEME. 

La instrucción ALTER PARTITION SCHEME puede agregar un grupo de archivos, o seleccionar uno existente, para almacenar la nueva partición. Puede asignar un grupo de archivos que ya contenga particiones para almacenar particiones adicionales. Una función de partición puede participar en más de un esquema de particiones. Por este motivo, todos los esquemas de partición que usan la función de partición a la que va a agregar particiones deben tener un grupo de archivos NEXT USED. De lo contrario, la instrucción ALTER PARTITION FUNCTION produce un error que muestra los esquemas de partición a los que les falta un grupo de archivos NEXT USED.  
  
Si crea todas las particiones en el mismo grupo de archivos, ese grupo se asigna inicialmente para que sea el grupo de archivos NEXT USED automáticamente. Sin embargo, después de realizarse una operación de división, ya no hay ningún grupo de archivos NEXT USED seleccionado. Asigne explícitamente el grupo de archivos para que sea el grupo de archivos NEXT USED mediante ALTER PARTITION SCHEME; de lo contrario, una operación de división posterior producirá errores.  
  
> [!NOTE]  
>  Limitaciones con el índice de almacén de columnas: solo las particiones vacías se pueden dividir cuando existe un índice de almacén de columnas en la tabla. Tendrá que quitar o deshabilitar el índice de almacén de columnas antes de realizar esta operación.  
  
MERGE [ RANGE ( *boundary_value*) ]  
Quita una partición y combina cualquier valor que exista en la partición en una de las particiones restantes. RANGE (*boundary_value*) debe ser un valor límite existente de la partición que se va a quitar. Este argumento elimina el grupo de archivos que originalmente contenía el valor *boundary_value* del esquema de partición, a menos que lo use una partición restante o que esté marcado con la propiedad NEXT USED. La partición combinada se encuentra en el grupo de archivos que originalmente no contenía *boundary_value*. *boundary_value* es una expresión constante que puede hacer referencia a variables (incluidas las variables de tipos definidos por el usuario) o funciones (incluidas las funciones definidas por el usuario). No puede hacer referencia a una expresión de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *boundary_value* debe coincidir con el tipo de datos de su columna de partición correspondiente, o debe poder convertirse a dicho tipo de datos de forma implícita. Tampoco se puede truncar *boundary_value* durante la conversión implícita de forma que el tamaño y la escala del valor no coincidan con su correspondiente valor *input_parameter_type*.  
  
> [!NOTE]  
>  Limitaciones con el índice de almacén de columnas: no se pueden combinar dos particiones no vacías que contengan un índice de almacén de columnas. Tendrá que quitar o deshabilitar el índice de almacén de columnas antes de realizar esta operación.  
  
## <a name="best-practices"></a>Prácticas recomendadas  
Mantenga siempre las particiones vacías en ambos extremos del rango de partición. Mantenga las particiones en ambos extremos para garantizar que la división y la combinación de particiones no realizan ningún movimiento de datos. La división de particiones se produce al principio y la combinación de particiones, al final. Evite dividir o combinar particiones con datos. Dividir o combinar particiones con datos puede ser ineficaz, ya que puede multiplicar por cuatro la generación de registros y causar bloqueos graves.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
ALTER PARTITION FUNCTION vuelve a crear particiones en cualquier tabla o índice que utilice la función en una única operación atómica. Sin embargo, esta operación se produce sin conexión, y dependiendo del alcance de las reparticiones puede consumir muchos recursos.  
  
ALTER PARTITION FUNCTION se puede utilizar solamente para dividir una partición en dos o combinar dos particiones en una. Para cambiar el modo en el que se crean particiones en una tabla (por ejemplo, de 10 particiones a 5), puede aplicar cualquiera de las opciones siguientes. Dependiendo de la configuración del sistema, estas opciones pueden tener un consumo de recursos diferente:  
  
-   Cree una tabla con particiones con la función de partición necesaria. A continuación, inserte los datos de la tabla antigua en la nueva utilizando la instrucción INSERT INTO...SELECT FROM.  
  
-   Cree un índice clúster con particiones en un montón.  
  
    > [!NOTE]  
    >  El resultado de quitar un índice clúster con particiones es un montón con particiones.  
  
-   Quite y vuelva a generar un índice con particiones existente mediante la instrucción CREATE INDEX de [!INCLUDE[tsql](../../includes/tsql-md.md)] con la cláusula DROP EXISTING = ON.  
  
-   Ejecute una secuencia de instrucciones ALTER PARTITION FUNCTION.  
  
Todos los grupos de archivos que estén afectados por ALTER PARTITION FUNCTION deben estar en línea.  
  
ALTER PARTITION FUNCTION produce un error cuando existe un índice clúster deshabilitado en cualquiera de las tablas que utilizan la función de partición.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no proporciona compatibilidad de replicación para modificar una función de partición. Los cambios en la función de partición de la base de datos de publicaciones se deben aplicar manualmente en la base de datos de suscripciones.  
  
## <a name="permissions"></a>Permisos  
Se pueden utilizar cualquiera de los siguientes permisos para ejecutar ALTER PARTITION FUNCTION:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se ha creado la función de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se ha creado la función de partición.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-splitting-a-partition-of-a-partitioned-table-or-index-into-two-partitions"></a>A. Dividir una partición de una tabla o un índice con particiones en dos particiones  
En el ejemplo siguiente se crea una función de partición para crear cuatro particiones en una tabla o en un índice. `ALTER PARTITION FUNCTION` divide una de las particiones en dos para crear un total de cinco particiones.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Split the partition between boundary_values 100 and 1000  
--to create two partitions between boundary_values 100 and 500  
--and between boundary_values 500 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
SPLIT RANGE (500);  
```  
  
### <a name="b-merging-two-partitions-of-a-partitioned-table-into-one-partition"></a>B. Mezclar dos particiones de una tabla con particiones en una partición  
En el ejemplo siguiente se crea la misma función de partición que antes y después se mezclan dos de las particiones en una partición para conseguir un total de tres particiones.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Merge the partitions between boundary_values 1 and 100  
--and between boundary_values 100 and 1000 to create one partition  
--between boundary_values 1 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
MERGE RANGE (100);  
```  
  
## <a name="see-also"></a>Consulte también  
[Tablas e índices con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
[DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
[ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
[DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
[sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
[sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md) [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
[sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
[sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
