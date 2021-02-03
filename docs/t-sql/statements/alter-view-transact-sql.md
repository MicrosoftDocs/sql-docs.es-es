---
description: ALTER VIEW (Transact-SQL)
title: ALTER VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
ms.assetid: 03eba220-13e2-49e3-bd9d-ea9df84dc28c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e01212fb92fc5b589ace24258551b48ae1dbfdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199481"
---
# <a name="alter-view-transact-sql"></a>ALTER VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica una vista creada anteriormente. Esto incluye una vista indizada. ALTER VIEW no afecta a desencadenadores ni procedimientos almacenados dependientes y no cambia permisos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ]   
[ WITH <view_attribute> [ ,...n ] ]   
AS select_statement   
[ WITH CHECK OPTION ] [ ; ]  
  
<view_attribute> ::=   
{   
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]       
}   
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
``` 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece la vista.  
  
 *view_name*  
 Es la vista que se va a cambiar.  
  
 *column*  
 Es el nombre de una o más columnas, separadas por comas, que van a formar parte de la vista especificada.  
  
> [!IMPORTANT]  
>  Los permisos de columna se mantienen solo cuando las columnas tienen el mismo nombre antes y después de que se ejecute ALTER VIEW.  
  
> [!NOTE]  
>  En las columnas de la vista, los permisos de un nombre de columna se aplican mediante una instrucción CREATE VIEW o ALTER VIEW, independientemente del origen de los datos subyacentes. Por ejemplo, si se conceden permisos para la columna **SalesOrderID** en una instrucción CREATE VIEW, una instrucción ALTER VIEW puede cambiar el nombre de la columna **SalesOrderID**, por ejemplo, a **OrderRef**, y aún tener los permisos asociados con la vista usando **SalesOrderID**.  
  
 ENCRYPTION  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Cifra las entradas en [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) que contienen el texto de la instrucción ALTER VIEW. WITH ENCRYPTION evita que la vista se publique como parte de la replicación de SQL Server.  
  
 SCHEMABINDING  
 Enlaza la vista al esquema de las tablas subyacentes. Cuando se especifica SCHEMABINDING, las tablas base no se pueden modificar de forma que afecten a la definición de la vista. La propia definición de la vista debe modificarse o quitarse primero para eliminar las dependencias de la tabla que se va a modificar. Cuando se usa SCHEMABINDING, _select\_statement_ debe incluir los nombres de dos partes (_schema_ **.** _object_) de tablas, vistas o funciones definidas por el usuario a las que se hace referencia. Todos los objetos a los que se hace referencia se deben encontrar en la misma base de datos.  
  
 Las vistas o las tablas que participan en una vista creada con la cláusula SCHEMABINDING no se pueden quitar a menos que se quite o cambie esa vista de forma que deje de tener un enlace de esquema. En caso contrario, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error. Además, se genera un error al ejecutar las instrucciones ALTER TABLE sobre tablas que participan en vistas que tienen enlaces de esquemas si estas instrucciones afectan a la definición de la vista.  
  
 VIEW_METADATA  
 Especifica que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá a las API de DB-Library, ODBC y OLE DB la información de metadatos sobre la vista en vez de las tablas base cuando se soliciten los metadatos del modo de exploración para una consulta que hace referencia a la vista. Los metadatos del modo de exploración son metadatos adicionales que la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] devuelve a las API de DB-Library, ODBC y OLE DB del lado cliente. Estos metadatos permiten a las API del lado cliente implementar cursores del lado cliente actualizables. Los metadatos del modo de exploración incluyen información sobre la tabla base a la que pertenecen las columnas del conjunto de resultados.  
  
 Para las vistas creadas con VIEW_METADATA, los metadatos del modo de exploración devuelven el nombre de vista y no los nombres de tablas base cuando describen columnas de la vista en el conjunto de resultados.  
  
 Cuando se crea una vista mediante WITH VIEW_METADATA, todas sus columnas (excepto una columna **timestamp**) son actualizables si la vista tiene los desencadenadores INSERT o UPDATE INSTEAD OF. Para más información, vea la sección Comentarios de [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 AS  
 Son las acciones que va a llevar a cabo la vista.  
  
 *select_statement*  
 Es la instrucción SELECT que define la vista.  
  
 WITH CHECK OPTION  
 Fuerza que todas las instrucciones de modificación de datos que se ejecuten en la vista sigan los criterios establecidos en *select_statement*.  
  
## <a name="remarks"></a>Comentarios  
 Para más información sobre ALTER VIEW, vea la sección Comentarios de [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
> [!NOTE]  
>  Si la anterior definición de vista se creó utilizando WITH ENCRYPTION o CHECK OPTION, estas opciones solo se habilitan si se incluyen en ALTER VIEW.  
  
 Si una vista que está actualmente en uso se modifica mediante ALTER VIEW, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] impone un bloqueo exclusivo de esquema sobre la vista. Cuando se concede el bloqueo, y no hay usuarios activos de la vista, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] elimina todas las copias de la vista de la caché de procedimientos. Los planes existentes que hacen referencia a la vista permanecen en la caché, pero se vuelven a compilar cuando se llaman.  
  
 ALTER VIEW se puede aplicar a vistas indizadas; no obstante, quita incondicionalmente todos los índices de la vista.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar ALTER VIEW, como mínimo, se necesita el permiso ALTER en OBJECT.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea una vista que contiene todos los empleados y sus fechas de contratación denominada `EmployeeHireDate`. Se conceden permisos sobre la vista, pero los requisitos se han cambiado para seleccionar los empleados que tienen fechas de contratación anteriores a una fecha determinada. A continuación, se utiliza `ALTER VIEW` para reemplazar la vista.  
  
```sql 
USE AdventureWorks2012 ;  
GO  
CREATE VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
```  
  
 La vista debe cambiarse para que incluya solo a los empleados que se contrataron antes de `2002`. Si no se utiliza ALTER VIEW, sino que la vista se quita y se vuelve a crear, deben volver a crearse la instrucción GRANT utilizada anteriormente y cualquier otra instrucción relacionada con permisos pertenecientes a esta vista.  
  
```sql  
ALTER VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID  
WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
