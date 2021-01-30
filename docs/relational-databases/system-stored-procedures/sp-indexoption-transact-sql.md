---
description: sp_indexoption (Transact-SQL)
title: sp_indexoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_indexoption
- sp_indexoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexoption
ms.assetid: 75f836be-d322-4a53-a45d-25bee6b42a52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8f7011bc4ce2120164683009676953ec44d58c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205523"
---
# <a name="sp_indexoption-transact-sql"></a>sp_indexoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Define los valores de la opción de bloqueo para índices clúster y no clúster definidos por el usuario o tablas sin ningún índice clúster.  
  
 El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] selecciona automáticamente el bloqueo de nivel de página, fila o tabla. No es necesario establecer estas opciones manualmente. **sp_indexoption** se proporciona a los usuarios expertos que saben con certeza que un tipo de bloqueo determinado siempre es adecuado.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] En su lugar, use [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_indexoption [ @IndexNamePattern = ] 'table_or_index_name'   
    , [ @OptionName = ] 'option_name'   
    , [ @OptionValue = ] 'value'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @IndexNamePattern = ] 'table_or_index_name'` Es el nombre completo o no completo de una tabla o índice definido por el usuario. *table_or_index_name* es de tipo **nvarchar (1035)** y no tiene ningún valor predeterminado. Las comillas solo son necesarias si se especifica un índice o nombre de tabla completo. Si se proporciona un nombre de tabla completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el nombre de la base de datos actual. Si se especifica un nombre de tabla sin ningún índice, el valor de la opción especificada se define para todos los índices de dicha tabla y para la tabla misma si no existe ningún índice clúster.  
  
`[ @OptionName = ] 'option_name'` Es un nombre de opción de índice. *option_name* es de tipo **VARCHAR (35)** y no tiene ningún valor predeterminado. *option_name* puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**AllowRowLocks**|Cuando el valor es TRUE, se permiten bloqueos de fila al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila. Cuando es FALSE, no se utilizan bloqueos de fila. El valor predeterminado es TRUE.|  
|**AllowPageLocks**|Cuando el valor es TRUE, se permiten bloqueos de página al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página. Cuando es FALSE, no se utilizan bloqueos de página. El valor predeterminado es TRUE.|  
|**DisAllowRowLocks**|Cuando es TRUE no se utilizan bloqueos de fila. Cuando el valor es FALSE, se permiten bloqueos de fila al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila.|  
|**DisAllowPageLocks**|Cuando es TRUE, no se utilizan bloqueos de página. Cuando el valor es FALSE, se permiten bloqueos de página al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página.|  
  
`[ @OptionValue = ] 'value'` Especifica si el valor *option_name* está habilitado (true, on, Yes o 1) o deshabilitado (false, OFF, no o 0). el *valor* es **VARCHAR (12)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o mayor que 0 (error)  
  
## <a name="remarks"></a>Observaciones  
 Los índices XML no se admiten. Si se especifica un índice XML o un nombre de tabla sin ningún nombre de índice y la tabla contiene un índice XML, la instrucción produce un error. Para establecer estas opciones, utilice [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) en su lugar.  
  
 Para mostrar las propiedades actuales de bloqueo de fila y de página, use [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) o la vista de catálogo [Sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) .  
  
-   Los bloqueos de nivel de fila, página y tabla se permiten al obtener acceso al índice cuando **AllowRowLocks** = true o **DisAllowRowLocks** = false y **AllowPageLocks** = true o **DisAllowPageLocks** = false. [!INCLUDE[ssDE](../../includes/ssde-md.md)] elige el bloqueo apropiado y puede cambiar de escala el bloqueo: de un bloqueo de fila o página a un bloqueo de tabla.  
  
 Solo se permite un bloqueo de nivel de tabla cuando se tiene acceso al índice cuando **AllowRowLocks** = false o **DisAllowRowLocks** = true y **AllowPageLocks** = false o **DisAllowPageLocks** = true.  
  
 Si se especifica un nombre de tabla sin ningún índice, la configuración se aplica a todos los índices de esa tabla. Si la tabla subyacente no tiene ningún índice clúster (es decir, es un montón), la configuración se aplica de la siguiente manera:  
  
-   Cuando **AllowRowLocks** o **DisAllowRowLocks** se establecen en true o false, la configuración se aplica al montón y a cualquier índice no clúster asociado.  
  
-   Cuando la opción **AllowPageLocks** se establece en true o **DisAllowPageLocks** se establece en false, la configuración se aplica al montón y a cualquier índice no clúster asociado.  
  
-   Cuando la opción **AllowPageLocks** se establece en false o **DisAllowPageLocks** se establece en true, el valor se aplica por completo a los índices no clúster. Es decir, no se permite ningún bloqueo de página en los índices no clúster. En el montón, no se permiten únicamente los bloqueos compartidos (S), de actualización (U) y exclusivos (X) de la página. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] aún puede adquirir un bloqueo de página de intención (IS, IU o IX) por motivos internos.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en la tabla.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-an-option-on-a-specific-index"></a>A. Definir una opción en un índice específico  
 En el siguiente ejemplo se deniegan los bloqueos de página en el `IX_Customer_TerritoryID` Índice de la `Customer` tabla.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_indexoption N'Sales.Customer.IX_Customer_TerritoryID',  
    N'disallowpagelocks', TRUE;  
```  
  
### <a name="b-setting-an-option-on-all-indexes-on-a-table"></a>B. Definir una opción en todos los índices de una tabla  
 El siguiente ejemplo no permite bloqueos de fila en los índices asociados con la tabla `Product`. La vista de catálogo `sys.indexes` se consulta antes y después de ejecutar el procedimiento `sp_indexoption` para mostrar los resultados de la instrucción.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
-- Set the disallowrowlocks option on the Product table.   
EXEC sp_indexoption N'Production.Product',  
    N'disallowrowlocks', TRUE;  
GO  
--Verify the row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
```  
  
### <a name="c-setting-an-option-on-a-table-with-no-clustered-index"></a>C. Definir una opción en una tabla sin clúster  
 El siguiente ejemplo no permite bloqueos de página en una tabla sin clúster (un montón). La `sys.indexes` vista de catálogo se consulta antes y después de `sp_indexoption` ejecutar el procedimiento para mostrar los resultados de la instrucción.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options of the table.   
SELECT OBJECT_NAME (object_id) AS [Table], type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
-- Set the disallowpagelocks option on the table.   
EXEC sp_indexoption DatabaseLog,  
    N'disallowpagelocks', TRUE;  
GO  
--Verify the row and page lock settings of the table.  
SELECT OBJECT_NAME (object_id) AS [Table], allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
