---
description: DBCC DBREINDEX (Transact-SQL)
title: DBCC DBREINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC DBREINDEX
- DBREINDEX_TSQL
- DBREINDEX
- DBCC_DBREINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- index rebuilding [SQL Server]
- rebuilding indexes
- dynamic index rebuilding [SQL Server]
- DBCC DBREINDEX statement
ms.assetid: 6e929d09-ccb5-4855-a6af-b616022bc8f6
author: pmasl
ms.author: umajay
ms.openlocfilehash: 8e29696d30bc46a6e7c9fe644f7d4e19df6db622
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813399"
---
# <a name="dbcc-dbreindex-transact-sql"></a>DBCC DBREINDEX (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Vuelve a generar uno o varios índices de una tabla de la base de datos especificada.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Use [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) en su lugar.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level))
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC DBREINDEX   
(   
    table_name   
    [ , index_name [ , fillfactor ] ]  
)  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *table_name*  
 Es el nombre de la tabla que contiene los índices especificados que se van a volver a generar. Los nombres de las tablas deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md) *.*  
  
 *index_name*  
 Es el nombre del índice que se va a volver a generar. Los nombres de los índices deben ajustarse a las reglas de los identificadores. Si se especifica *index_name*, debe especificarse *table_name*. Si no se especifica *index_name* o se indica como " ", se recompilan todos los índices de la tabla.  
  
 *fillfactor*  
 Es el porcentaje de espacio de cada página de índice que se utiliza para almacenar los datos cuando el índice se crea o se vuelve a generar. *fillfactor* reemplaza al factor de relleno usado al crear el índice y se convierte en el nuevo valor predeterminado para el índice y para cualquier otro índice no agrupado que se recompile como consecuencia de la recompilación de un índice agrupado.  
 Cuando *fillfactor* es 0, DBCC DBREINDEX usa el último valor del factor de relleno que se ha especificado para el índice. Este valor se almacena en la vista de catálogo **sys.indexes**.   
 Si se especifica *fillfactor*, debe especificarse *table_name* e *index_name*. Si no se especifica *fillfactor*, se usa el factor de relleno predeterminado (100). Para obtener más información, vea [Especificar el factor de relleno para un índice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
 WITH NO_INFOMSGS  
 Suprime todos los mensajes informativos con niveles de gravedad entre 0 y 10.  
  
## <a name="remarks"></a>Observaciones  
DBCC DBREINDEX vuelve a generar un índice de una tabla o todos los índices definidos de una tabla. Al permitir que los índices se vuelvan a generar dinámicamente, los índices que implementen restricciones PRIMARY KEY o UNIQUE se pueden volver a generar sin tener que quitar y volver a crear las restricciones. Esto significa que un índice puede volver a generarse sin conocer la estructura de una tabla ni sus restricciones. Esto puede suceder después de copiar datos de forma masiva en la tabla.

DBCC DBREINDEX puede volver a generar todos los índices para una tabla en una instrucción. Es más sencillo que codificar varias instrucciones DROP INDEX y CREATE INDEX. Como todo el trabajo se hace con una instrucción, DBCC DBREINDEX es, automáticamente, una acción atómica, mientras que, para ser atómicas, las instrucciones DROP INDEX y CREATE INDEX individuales deben formar parte de una transacción. Además, DBCC DBREINDEX ofrece más optimizaciones que las instrucciones DROP INDEX y CREATE INDEX individuales.

A diferencia de DBCC INDEXDEFRAG o ALTER INDEX, con la opción REORGANIZE, DBCC DBREINDEX es una operación sin conexión. Si se vuelve a generar un índice no clúster, se mantiene un bloqueo compartido en la tabla en cuestión durante la operación. Esto evita que se modifique la tabla. Si el índice clúster se vuelve a generar, se mantiene un bloqueo de tabla exclusivo. Así se evita cualquier acceso a la tabla, haciendo que la tabla esté sin conexión. Utilice la instrucción ALTER INDEX REBUILD con la opción ONLINE para volver a agregar un índice en línea o para controlar el grado de paralelismo durante la operación de regeneración del índice.

Para más información sobre cómo seleccionar un método para recompilar o reorganizar un índice, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
  
## <a name="restrictions"></a>Restricciones  
No se admite el uso de DBCC DBREINDEX en los objetos siguientes:
-   Tablas del sistema  
-   Índices espaciales  
-   índices de almacén de columnas optimizados de memoria xVelocity  
  
## <a name="result-sets"></a>Conjuntos de resultados  
A menos que se especifique NO_INFOMSGS (se debe especificar el nombre de la tabla), DBCC DBREINDEX siempre devuelve:
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Permisos  
El autor de la llamada debe ser el propietario de la tabla, o bien un miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_owner** o **db_ddladmin**.
  
## <a name="examples"></a>Ejemplos  
### <a name="a-rebuilding-an-index"></a>A. Volver a generar un índice  
En este ejemplo se vuelve a generar el índice clúster `Employee_EmployeeID` con un factor de relleno de `80` en la tabla `Employee` de la base de datos `AdventureWorks`.
  
```sql  
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', PK_Employee_BusinessEntityID,80);  
GO  
```  
  
### <a name="b-rebuilding-all-indexes"></a>B. Volver a generar todos los índices  
En este ejemplo se vuelven a generar todos los índices de la tabla `Employee` de `AdventureWorks` con un valor de factor de relleno de `70`.
  
```sql
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', ' ', 70);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
