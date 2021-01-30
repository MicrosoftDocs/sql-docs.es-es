---
description: sp_tables (Transact-SQL)
title: sp_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a176073f8951cd15acd34aa00b92fc268f90c27e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180577"
---
# <a name="sp_tables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una lista de objetos que se pueden consultar en el entorno actual. Esto significa cualquier tabla o vista, excepto los objetos sinónimos.  
  
> [!NOTE]  
>  Para determinar el nombre del objeto base de un sinónimo, consulte la vista de catálogo [Sys. sinónimos](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_name = ] 'name'` Es la tabla que se usa para devolver información de catálogo. *Name* es de tipo **nvarchar (384)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín.  
  
`[ @table_owner = ] 'owner'` Es el propietario de la tabla que se utiliza para devolver información del catálogo. *Owner* es **nvarchar (384)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín. Si no se especifica el propietario, se aplican las reglas de visibilidad de tabla predeterminadas del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual posee una tabla en la que se especifica el nombre, se devuelven las columnas de esa tabla. Si no se especifica el propietario y el usuario actual no es el propietario de una tabla con el nombre especificado, este procedimiento busca una tabla con el nombre especificado que pertenezca al propietario de la base de datos. Si existe una, se devuelven las columnas de esa tabla.  
  
`[ @table_qualifier = ] 'qualifier'` Es el nombre del calificador de tabla. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
``[ , [ @table_type = ] "'type', 'type'" ]`` Es una lista de valores, separados por comas, que proporciona información sobre todas las tablas de los tipos de tabla especificados. Estos incluyen **TABLE**, **SYSTEMTABLE** y **View**. *Type* es de tipo **VARCHAR (100)** y su valor predeterminado es NULL.  
  
> [!NOTE]  
>  Cada tipo de tabla debe especificarse entre comillas simples y todo el parámetro debe especificarse entre comillas dobles. Los tipos de tabla deben especificarse en mayúsculas. Si SET QUOTED_IDENTIFIER está establecido en ON (activado), las comillas simples deben ser comillas dobles y todo el parámetro debe especificarse entre comillas simples.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina si los caracteres de subrayado (_), porcentaje (%) y corchete ([o]) se interpretan como caracteres comodín. Los valores válidos son 0 (coincidencia de patrón desactivada) y 1 (coincidencia de patrón activada). *fUsePattern* es de **bit** y su valor predeterminado es 1.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nombre del calificador de tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. Este campo puede ser NULL.|  
|**TABLE_OWNER**|**sysname**|Nombre del propietario de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna representa el nombre del usuario de la base de datos que creó la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_TYPE**|**varchar(32)**|Tabla, tabla del sistema o vista.|  
|**COMENTARIOS**|**VARCHAR (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no devuelve ningún valor para esta columna.|  
  
## <a name="remarks"></a>Observaciones  
 Para obtener la máxima interoperatividad, el cliente de la puerta de enlace solo debe dar por supuesta la concordancia del patrón estándar de SQL-92 (los caracteres comodín % y _).  
  
 No siempre se comprueba la información de privilegios acerca del acceso de lectura o de escritura del usuario actual para una tabla específica. Por lo tanto, el acceso no está garantizado. Este conjunto de resultados no solo incluye tablas y vistas, sino también sinónimos y alias para las puertas de enlace a productos DBMS que admiten dichos tipos. Si el atributo de servidor **ACCESSIBLE_TABLES** es Y en el conjunto de resultados de **sp_server_info**, solo se devuelven las tablas a las que puede tener acceso el usuario actual.  
  
 **sp_tables** es equivalente a **SQLTables** en ODBC. Los resultados devueltos se ordenan por **TABLE_TYPE**, **TABLE_QUALIFIER**, **table_owner** y **TABLE_NAME**.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>A. Devolver una lista de objetos que se pueden consultar en el entorno actual  
 En el siguiente ejemplo se devuelve una lista de objetos que pueden ser consultas en el entorno actual.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>B. Devolver información sobre las tablas en un esquema especificado  
 En el siguiente ejemplo se devuelve información sobre las tablas que pertenecen al esquema `Person` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>C. Devolver una lista de objetos que se pueden consultar en el entorno actual  
 En el siguiente ejemplo se devuelve una lista de objetos que pueden ser consultas en el entorno actual.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>D. Devolver información sobre las tablas en un esquema especificado  
 En el ejemplo siguiente se devuelve información acerca de las tablas de dimensiones de la `AdventureWorksPDW201` base de datos.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Sys. sinónimos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

