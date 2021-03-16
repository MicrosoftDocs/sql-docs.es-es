---
description: Vea la definición de tabla
title: Vea la definición de tabla
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- showing table properties
- displaying table properties
- tables [SQL Server], properties
- viewing table properties
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49a0a836cb2e61bef9b4edb280685392f62544ec
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532341"
---
# <a name="view-the-table-definition"></a>Vea la definición de tabla
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Puede mostrar las propiedades de una tabla en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para mostrar las propiedades de la tabla con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Solamente puede ver las propiedades de una tabla si es propietario de la tabla o tiene concedidos permisos para esa tabla.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-show-table-properties-in-the-properties-window"></a>Para mostrar las propiedades de la tabla en la ventana Propiedades  
  
1.  En el Explorador de objetos, seleccione la tabla cuyas propiedades desea mostrar.  
  
2.  Haga clic con el botón derecho en la tabla y elija **Propiedades** en el menú contextual. Para obtener más información, vea [Propiedades de tabla - SSMS](../../relational-databases/tables/table-properties-ssms.md).  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-show-table-properties"></a>Para mostrar las propiedades de una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En el ejemplo se ejecuta el procedimiento almacenado del sistema sp_help para devolver toda la información de columna correspondiente al objeto especificado.  
  
```sql  
EXEC sp_help 'dbo.mytable';
```  
    
 Para más información, consulte [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md).

 También puede consultar directamente las vistas de catálogo del sistema para consultar información de metadatos de objetos sobre tablas, esquemas y columnas. Por ejemplo:  
  
```sql
SELECT s.name, t.name, c.* FROM sys.columns AS c
INNER JOIN sys.tables AS t ON t.object_id = c.object_id
INNER JOIN sys.schemas AS s ON s.schema_id = t.schema_id
WHERE t.object_id = object_id('mytable') AND s.name = 'dbo';
```
    
 Para más información, consulte: 

* [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)    
* [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)    
* [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)     

