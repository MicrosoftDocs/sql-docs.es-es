---
description: sp_stored_procedures (Transact-SQL)
title: sp_stored_procedures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stored_procedures_TSQL
- sp_stored_procedures
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stored_procedures
ms.assetid: fe52dd83-000a-4665-83fb-7a0024193dec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3266b5619701d28a4fc65eb79d3f3e16c75e40b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193217"
---
# <a name="sp_stored_procedures-transact-sql"></a>sp_stored_procedures (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una lista de los procedimientos almacenados del entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_stored_procedures [ [ @sp_name = ] 'name' ]   
    [ , [ @sp_owner = ] 'schema']   
    [ , [ @sp_qualifier = ] 'qualifier' ]  
    [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @sp_name = ] 'name'` Es el nombre del procedimiento utilizado para devolver información del catálogo. *Name* es de tipo **nvarchar (390)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín.  
  
`[ @sp_owner = ] 'schema'` Es el nombre del esquema al que pertenece el procedimiento. *Schema* es de tipo **nvarchar (384)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas predeterminadas de visibilidad de procedimientos del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el esquema actual contiene un procedimiento con el nombre especificado, se devuelve ese procedimiento. Si se especifica un procedimiento almacenado no calificado, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] busca el procedimiento siguiendo este orden:  
  
-   El esquema **sys** de la base de datos actual.  
  
-   El esquema predeterminado del autor de la llamada se ejecuta en un lote o en SQL dinámico; o, si el nombre del procedimiento no calificado aparece dentro del cuerpo de otra definición de procedimiento, se busca el esquema que contiene este otro procedimiento a continuación.  
  
-   El esquema **dbo** de la base de datos actual.  
  
`[ @qualifier = ] 'qualifier'` Es el nombre del calificador del procedimiento. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas en el formulario (_calificador_**.** _esquema_**.** _nombre_. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el *calificador* representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina si el carácter de subrayado (_), porcentaje (%) o corchetes []) se interpreta como caracteres comodín. *fUsePattern* es de **bit** y su valor predeterminado es 1.  
  
 **0** = la coincidencia de patrones está desactivada.  
  
 **1** = la coincidencia de patrones está activada.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nombre del calificador del procedimiento. Esta columna puede ser NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nombre del propietario del procedimiento. Esta columna siempre devuelve un valor.|  
|**PROCEDURE_NAME**|**nvarchar (134)**|Nombre del procedimiento. Esta columna siempre devuelve un valor.|  
|**NUM_INPUT_PARAMS**|**int**|Reservado para un uso futuro.|  
|**NUM_OUTPUT_PARAMS**|**int**|Reservado para un uso futuro.|  
|**NUM_RESULT_SETS**|**int**|Reservado para un uso futuro.|  
|**COMENTARIOS**|**VARCHAR (254)**|Descripción del procedimiento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no devuelve ningún valor para esta columna.|  
|**PROCEDURE_TYPE**|**smallint**|Tipo de procedimiento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre devuelve 2.0. Este valor puede ser uno de los siguientes:<br /><br /> 0 = SQL_PT_UNKNOWN<br /><br /> 1 = SQL_PT_PROCEDURE<br /><br /> 2 = SQL_PT_FUNCTION|  
  
## <a name="remarks"></a>Observaciones  
 Para obtener la máxima interoperatividad, el cliente de la puerta de enlace solo debe dar por supuesta la coincidencia de patrón estándar de SQL (los caracteres de comodín % y _).  
  
 La información de permisos acerca del acceso de ejecución del usuario actual para un procedimiento almacenado específico no se comprueba necesariamente, por lo tanto, el acceso no está garantizado. Observe que solo se utilizan los nombres en tres partes. Esto significa que solo se devolverán los procedimientos almacenados locales, y no los remotos (que precisan nombres de cuatro partes), cuando se ejecuten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el atributo de servidor ACCESSIBLE_SPROC es Y en el conjunto de resultados de **sp_server_info**, solo se devuelven los procedimientos almacenados que puede ejecutar el usuario actual.  
  
 **sp_stored_procedures** es equivalente a **SQLProcedures** en ODBC. Los resultados devueltos se ordenan por **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER** y **procedure_name**.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-all-stored-procedures-in-the-current-database"></a>A. Devolver todos los procedimientos almacenados en la base de datos actual  
 En el ejemplo siguiente se devuelven todos los procedimientos almacenados en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_stored_procedures;  
```  
  
### <a name="b-returning-a-single-stored-procedure"></a>B. Devolver un solo procedimiento almacenado  
 En el ejemplo siguiente se devuelve un conjunto de resultados para el procedimiento almacenado `uspLogError`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_stored_procedures N'uspLogError', N'dbo', N'AdventureWorks2012', 1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
