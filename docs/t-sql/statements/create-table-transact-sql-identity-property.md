---
description: CREATE TABLE (Transact-SQL) IDENTITY (propiedad)
title: IDENTITY (propiedad de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY property
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY property
- autonumbers, identity numbers
ms.assetid: 8429134f-c821-4033-a07c-f782a48d501c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e239ff0edf653c5c172e0dfacb6b9ee835901a91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177942"
---
# <a name="create-table-transact-sql-identity-property"></a>CREATE TABLE (Transact-SQL) IDENTITY (propiedad)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Crea una columna de identidad en una tabla. Esta propiedad se usa con las instrucciones CREATE TABLE y ALTER TABLE de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  La propiedad IDENTITY no es la misma que la propiedad **Identity** de SQL-DMO, que expone la propiedad de identidad de las filas de una columna.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
IDENTITY [ (seed , increment) ]
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *seed*  
 Es el valor que se utiliza para la primera fila cargada en la tabla.  
  
 *increment*  
 Se trata del valor incremental que se agrega al valor de identidad de la anterior fila cargada.

 > [!NOTE]
 > En Azure Synapse Analytics, los valores de identidad no son incrementales debido a la arquitectura distribuida del almacenamiento de datos. Para más información, consulte [Uso de IDENTITY para crear claves suplentes en un grupo de Synapse SQL](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#allocation-of-values).
  
 Debe especificar tanto el valor de inicialización como el incremento, o bien ninguno de los dos. Si no se especifica ninguno, el valor predeterminado es (1,1).  
  
## <a name="remarks"></a>Observaciones  
 Las columnas de identidad pueden usarse para generar valores de clave. La propiedad de identidad de una columna garantiza lo siguiente:  
  
-   Cada nuevo valor se genera basándose en el valor actual de inicialización e incremento.  
  
-   Cada nuevo valor de una transacción determinada es diferente de otras transacciones simultáneas de la tabla.  
  
 La propiedad de identidad de una columna no garantiza lo siguiente:  
  
-   **Uniqueness of the value** (Unicidad del valor): La unicidad debe aplicarse mediante una restricción **PRIMARY KEY** o **UNIQUE**, o mediante un índice **UNIQUE**. - 
 
> [!NOTE]
> Azure Synapse Analytics no admite **PRIMARY KEY**, la restricción **UNIQUE** ni el índice **UNIQUE**. Para más información, consulte [Uso de IDENTITY para crear claves suplentes en un grupo de Synapse SQL](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#what-is-a-surrogate-key).

-   **Consecutive values within a transaction** (Valores consecutivos en una transacción): No se garantiza que una transacción que inserta varias filas obtenga valores consecutivos para las filas porque podrían producirse otras inserciones simultáneas en la tabla. Si los valores deben ser consecutivos, la transacción debe usar un bloqueo exclusivo en la tabla o usar el nivel de aislamiento **SERIALIZABLE**.  
  
-   **Consecutive values after server restart or other failures** (Valores consecutivos después de un reinicio del servidor u otros errores) -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría almacenar en memoria caché los valores de identidad por motivos de rendimiento y algunos de los valores asignados podrían perderse durante un error de la base de datos o un reinicio del servidor. Esto puede tener como resultado espacios en el valor de identidad al insertarlo. Si no es aceptable que haya espacios, la aplicación debe usar mecanismos propios para generar valores de clave. El uso de un generador de secuencias con la opción **NOCACHE** puede limitar los espacios a transacciones que nunca se llevan a cabo.  
  
-   **Reuse of values** (Reutilización de valores): Para una propiedad de identidad determinada, con un valor de inicialización e incremento específico, el motor no reutiliza los valores de identidad. Si una instrucción de inserción concreta produce un error o si la instrucción de inserción se revierte, los valores de identidad utilizados se pierden y no volverán a generarse. Esto puede tener como resultado espacios cuando se generan los valores de identidad siguientes.  
  
 Estas restricciones forman parte del diseño para mejorar el rendimiento y porque son aceptables en muchas situaciones comunes. Si no puede usar valores de identidad debido a estas restricciones, cree una tabla independiente que contenga un valor actual y administre el acceso a la tabla y la asignación de números con su aplicación.  
  
 Si una tabla con una columna de identidad se publica para replicarla, la columna de identidad debe administrarse de manera apropiada para el tipo de replicación utilizado. Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Solo se puede crear una columna de identidad para cada tabla.  
  
 En las tablas optimizadas para memoria, el valor de inicialización y el valor de incremento debe establecerse en 1,1. Si seed o increment se establecen en un valor distinto de 1, se produce este error: The use of seed and increment values other than 1 is not supported with memory optimized tables (No se admite el uso de valores seed e increment distintos de 1 con tablas optimizadas en memoria).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-identity-property-with-create-table"></a>A. Utilizar la propiedad IDENTITY con CREATE TABLE  
 En el siguiente ejemplo se crea una nueva tabla con la propiedad `IDENTITY` para un número de identificación que se incrementa automáticamente.  
  
```sql  
USE AdventureWorks2012;  
  
IF OBJECT_ID ('dbo.new_employees', 'U') IS NOT NULL  
   DROP TABLE new_employees;  
GO  
CREATE TABLE new_employees  
(  
 id_num int IDENTITY(1,1),  
 fname varchar (20),  
 minit char(1),  
 lname varchar(30)  
);  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Karin', 'F', 'Josephs');  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Pirkko', 'O', 'Koskitalo');  
```  
  
### <a name="b-using-generic-syntax-for-finding-gaps-in-identity-values"></a>B. Utilizar la sintaxis genérica para buscar espacios en los valores de identidad  
 En este ejemplo se muestra la sintaxis genérica utilizada para buscar espacios en valores de identidad cuando se quitan datos.  
  
> [!NOTE]  
>  La primera parte del siguiente script de [!INCLUDE[tsql](../../includes/tsql-md.md)] se ha diseñado solo como ejemplo. Puede ejecutar el script de [!INCLUDE[tsql](../../includes/tsql-md.md)] que comienza por el comentario: `-- Create the img table`.  
  
```sql 
-- Here is the generic syntax for finding identity value gaps in data.  
-- The illustrative example starts here.  
SET IDENTITY_INSERT tablename ON;  
DECLARE @minidentval column_type;  
DECLARE @maxidentval column_type;  
DECLARE @nextidentval column_type;  
SELECT @minidentval = MIN($IDENTITY), @maxidentval = MAX($IDENTITY)  
    FROM tablename  
IF @minidentval = IDENT_SEED('tablename')  
   SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('tablename')  
   FROM tablename t1  
   WHERE $IDENTITY BETWEEN IDENT_SEED('tablename') AND   
      @maxidentval AND  
      NOT EXISTS (SELECT * FROM tablename t2  
         WHERE t2.$IDENTITY = t1.$IDENTITY +   
            IDENT_INCR('tablename'))  
ELSE  
   SELECT @nextidentval = IDENT_SEED('tablename');  
SET IDENTITY_INSERT tablename OFF;  
-- Here is an example to find gaps in the actual data.  
-- The table is called img and has two columns: the first column   
-- called id_num, which is an increasing identification number, and the   
-- second column called company_name.  
-- This is the end of the illustration example.  
  
-- Create the img table.  
-- If the img table already exists, drop it.  
-- Create the img table.  
IF OBJECT_ID ('dbo.img', 'U') IS NOT NULL  
   DROP TABLE img;  
GO  
CREATE TABLE img (id_num INT IDENTITY(1,1), company_name sysname);  
INSERT img(company_name) VALUES ('New Moon Books');  
INSERT img(company_name) VALUES ('Lucerne Publishing');  
-- SET IDENTITY_INSERT ON and use in img table.  
SET IDENTITY_INSERT img ON;  
  
DECLARE @minidentval SMALLINT;  
DECLARE @nextidentval SMALLINT;  
SELECT @minidentval = MIN($IDENTITY) FROM img  
 IF @minidentval = IDENT_SEED('img')  
    SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('img')  
    FROM img t1  
    WHERE $IDENTITY BETWEEN IDENT_SEED('img') AND 32766 AND  
      NOT    EXISTS (SELECT * FROM img t2  
          WHERE t2.$IDENTITY = t1.$IDENTITY + IDENT_INCR('img'))  
 ELSE  
    SELECT @nextidentval = IDENT_SEED('img');  
SET IDENTITY_INSERT img OFF;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;función de Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md)   
 [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
