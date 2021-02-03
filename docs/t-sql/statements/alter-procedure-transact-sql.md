---
description: ALTER PROCEDURE (Transact-SQL)
title: ALTER PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_PROCEDURE_TSQL
- ALTER_PROC_TSQL
- ALTER PROC
- ALTER PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROCEDURE statement
- stored procedure modifications [SQL Server]
- modifying stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: ed9b2f76-11ec-498d-a95e-75b490a75733
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef55b1a81b4189f80bfe45410128e492a96bb191
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204147"
---
# <a name="alter-procedure-transact-sql"></a>ALTER PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica un procedimiento creado anteriormente por la ejecución de la instrucción CREATE PROCEDURE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo al tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ VARYING ] [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH <procedure_option> [ ,...n ] ]  
[ FOR REPLICATION ]   
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }  
[;]  
  
<procedure_option> ::=   
    [ ENCRYPTION ]  
    [ RECOMPILE ]  
    [ EXECUTE AS Clause ]  
```  
  
```syntaxsql  
-- Syntax for SQL Server CLR Stored Procedure  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH EXECUTE AS Clause ]  
AS { EXTERNAL NAME assembly_name.class_name.method_name }  
[;]  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name  
    [ { @parameterdata_type } [= ] ] [ ,...n ]  
AS { [ BEGIN ] sql_statement [ ; ] [ ,...n ] [ END ] }  
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 El nombre del esquema al que pertenece el procedimiento.  
  
 *procedure_name*  
 El nombre del procedimiento que se va a cambiar. Los nombres de los procedimientos se deben ajustar a las reglas para los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 **;** *number*  
 Entero opcional existente que se usa para agrupar los procedimientos del mismo nombre, de forma que puedan quitarse juntos mediante una sola instrucción DROP PROCEDURE.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 **@** *parameter*  
 Parámetro del procedimiento. Se pueden especificar hasta 2.100 parámetros.  
  
 [ _type\_schema\_name_ **.** ] _data\_type_  
 Es el tipo de datos del parámetro y el esquema al que pertenece.  
  
 Para obtener información acerca de las restricciones a los tipos de datos, vea [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 VARYING  
 Especifica el conjunto de resultados admitido como parámetro de salida. Este parámetro se genera dinámicamente por medio del procedimiento almacenado y su contenido puede variar. Solo se aplica a los parámetros de cursor. Esta opción no es válida para los procedimientos CLR.  
  
 *default*  
 Es un valor predeterminado para el parámetro.  
  
 OUT | OUTPUT  
 Indica que se trata de un parámetro devuelto.  
  
 READONLY  
 Indica que el parámetro no se puede actualizar ni modificar dentro del cuerpo del procedimiento. Si el tipo de parámetro es un tipo con valores de tabla, se debe especificar READONLY.  
  
 RECOMPILE  
 Indica que [!INCLUDE[ssDE](../../includes/ssde-md.md)] no almacena en la memoria caché ningún plan para este procedimiento y el procedimiento se vuelve a compilar en tiempo de ejecución.  
  
 ENCRYPTION  
 **Se aplica a**: SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores), y [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]  
  
 Indica que [!INCLUDE[ssDE](../../includes/ssde-md.md)] convertirá el texto original de la instrucción ALTER PROCEDURE en un formato protegido. La salida de la protección no es directamente visible en ninguna de las vistas de catálogo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los usuarios que no disponen de acceso a las tablas del sistema o a los archivos de base de datos no pueden recuperar el texto protegido. En cambio, estará disponible para los usuarios con privilegios que puedan obtener acceso a las tablas del sistema a través del [puerto DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) o directamente a los archivos de base de datos. Además, los usuarios que pueden adjuntar un depurador al proceso del servidor pueden recuperar el procedimiento original de la memoria en tiempo de ejecución. Para más información sobre cómo tener acceso al sistema, vea [Configuración de visibilidad de los metadatos](../../relational-databases/security/metadata-visibility-configuration.md).  
  
 Los procedimientos creados mediante esta opción no se pueden publicar como parte de la replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Esta opción no se puede especificar para procedimientos almacenados de Common Language Runtime (CLR).  
  
> [!NOTE]  
>  Durante una actualización, [!INCLUDE[ssDE](../../includes/ssde-md.md)] utiliza los comentarios protegidos almacenados en **sys.sql_modules** para volver a crear los procedimientos.  
  
 EXECUTE AS  
 Especifica el contexto de seguridad en el que se debe ejecutar el procedimiento almacenado una vez que se obtiene acceso al mismo.  
  
 Para obtener más información, vea [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 FOR REPLICATION  

  
 Especifica que los procedimientos almacenados creados para la replicación no se pueden ejecutar en el suscriptor. Se utiliza un procedimiento almacenado creado con la opción FOR REPLICATION como un filtro de procedimiento almacenado y solo se ejecuta durante la replicación. No se pueden declarar los parámetros si se especifica FOR REPLICATION. Esta opción no es válida para los procedimientos CLR. La opción RECOMPILE no se tiene en cuenta en el caso de procedimientos creados con FOR REPLICATION.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 { [ BEGIN ] *sql_statement* [;] [ ...*n* ] [ END ] }  
 Una o más instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que comprenden el cuerpo del procedimiento. Puede usar las palabras clave BEGIN y END opcionales para incluir las instrucciones. Para obtener más información, vea las secciones Prácticas recomendadas, Comentarios generales, así como Limitaciones y restricciones que aparecen en [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica el método de un ensamblado de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] para un procedimiento CLR almacenado al que se va a hacer referencia. *class_name* debe ser un identificador [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido y debe existir como clase en el ensamblado. Si la clase tiene un nombre calificado como espacio de nombres que utiliza un punto (**.**) para separar las partes del espacio de nombres, el nombre de la clase debe estar delimitado por corchetes (**[]**) o comillas (**""**). El método especificado debe ser un método estático de la clase.  
  
 De manera predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede ejecutar código CLR. Se pueden crear, modificar y quitar objetos de base de datos que hagan referencia a módulos de Common Language Runtime, pero estas referencias no se pueden ejecutar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hasta que se habilite la [opción clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Para habilitar esta opción, use [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Los procedimientos CLR no se admiten en las bases de datos independientes.  
  
## <a name="general-remarks"></a>Notas generales  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] no se pueden modificar para que sean procedimientos almacenados CLR y viceversa.  
  
 ALTER PROCEDURE no cambia los permisos ni afecta a ningún procedimiento almacenado ni desencadenador dependientes. Sin embargo, la configuración de la sesión actual para QUOTED_IDENTIFIER y ANSI_NULLS se incluye en el procedimiento almacenado cuando se modifica. Si la configuración es distinta de la que se estaba aplicando cuando se creó originalmente el procedimiento almacenado, el comportamiento de este último puede cambiar.  
  
 Si anteriormente se creó una definición de procedimiento mediante WITH ENCRYPTION o WITH RECOMPILE, estas opciones solo se habilitan si se incluyen en ALTER PROCEDURE.  
  
 Para obtener más información sobre los procedimientos almacenados, vea [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Necesita el permiso **ALTER** en el procedimiento o la pertenencia al rol fijo de base de datos **db_ddladmin**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea el procedimiento almacenado `uspVendorAllInfo`. Este procedimiento devuelve los nombres de todos los proveedores que proporciona [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], los productos que suministran, su solvencia y su disponibilidad. Después de crear este procedimiento, se modifica para devolver un conjunto de resultados diferente.  
  
```sql
IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE Purchasing.uspVendorAllInfo;  
GO  
CREATE PROCEDURE Purchasing.uspVendorAllInfo  
WITH EXECUTE AS CALLER  
AS  
    SET NOCOUNT ON;  
    SELECT v.Name AS Vendor, p.Name AS 'Product name',   
      v.CreditRating AS 'Rating',   
      v.ActiveFlag AS Availability  
    FROM Purchasing.Vendor v   
    INNER JOIN Purchasing.ProductVendor pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product p  
      ON pv.ProductID = p.ProductID   
    ORDER BY v.Name ASC;  
GO    
```  
  
 En el ejemplo siguiente se modifica el procedimiento almacenado `uspVendorAllInfo`. Quita la cláusula EXECUTE AS CALLER y modifica el cuerpo del procedimiento para devolver solo los proveedores que proporcionan el producto especificado. Las funciones `LEFT` y `CASE` permiten personalizar la apariencia del conjunto de resultados.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER PROCEDURE Purchasing.uspVendorAllInfo  
    @Product VARCHAR(25)   
AS  
    SET NOCOUNT ON;  
    SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
    'Rating' = CASE v.CreditRating   
        WHEN 1 THEN 'Superior'  
        WHEN 2 THEN 'Excellent'  
        WHEN 3 THEN 'Above average'  
        WHEN 4 THEN 'Average'  
        WHEN 5 THEN 'Below average'  
        ELSE 'No rating'  
        END  
    , Availability = CASE v.ActiveFlag  
        WHEN 1 THEN 'Yes'  
        ELSE 'No'  
        END  
    FROM Purchasing.Vendor AS v   
    INNER JOIN Purchasing.ProductVendor AS pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product AS p   
      ON pv.ProductID = p.ProductID   
    WHERE p.Name LIKE @Product  
    ORDER BY v.Name ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```   
 Vendor               Product name  Rating    Availability  
-------------------- ------------- -------   ------------  
Proseware, Inc.      LL Crankarm   Average   No  
Vision Cycles, Inc.  LL Crankarm   Superior  Yes  
(2 row(s) affected)`  
```  

## <a name="see-also"></a>Consulte también  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Procedimientos almacenados &#40;motor de base de datos&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)  
  
  
