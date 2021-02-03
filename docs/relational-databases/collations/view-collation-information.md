---
description: Ver información de intercalación
title: Ver información de intercalación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- collations [SQL Server], view
ms.assetid: 1338b4ea-7142-44bc-a3b9-44e54431405f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a8806f26cd27ebe671946a0efa723ddaa030970
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251170"
---
# <a name="view-collation-information"></a>Ver información de intercalación
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
<a name="Top"></a> Puede ver la intercalación de un servidor, una base de datos o una columna en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mediante las opciones de menú del Explorador de objetos o mediante [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="how-to-view-a-collation-setting"></a><a name="Procedures"></a> Cómo ver una configuración de intercalación  
 Puede usar cualquiera de los siguientes medios:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para ver una configuración de intercalación de un servidor (instancia de SQL Server) en el Explorador de objetos**  
  
1.  En el Explorador de objetos, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Haga clic con el botón derecho en la instancia y seleccione **Propiedades**.  
  
 **Para ver una configuración de intercalación de una base de datos en el Explorador de objetos**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos y seleccione **Propiedades**.  
  
 **Para ver una configuración de intercalación de una columna en el Explorador de objetos**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, la base de datos y, por último, **Tablas**.  
  
3.  Expanda la tabla que contiene la columna y, a continuación, **Columnas**.  
  
4.  Haga clic con el botón derecho en la columna y seleccione **Propiedades**. Si la propiedad collation está vacía, la columna no es un tipo de datos de caracteres.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para ver la configuración de intercalación de un servidor**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, en la barra de herramientas, haga clic en **Nueva consulta**.  
  
2.  En la ventana de consulta, escriba la siguiente instrucción que usa la función de sistema SERVERPROPERTY.  
  
    ```sql  
    SELECT CONVERT (varchar(256), SERVERPROPERTY('collation'));  
    ```  
  
3.  También puede usar el procedimiento almacenado del sistema sp_helpsort.  
  
    ```sql  
    EXECUTE sp_helpsort;  
    ```  
  
 **Para ver todas las intercalaciones admitidas por [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, en la barra de herramientas, haga clic en **Nueva consulta**.  
  
2.  En la ventana de consulta, escriba la siguiente instrucción que usa la función de sistema SERVERPROPERTY.  
  
    ```sql  
    SELECT name, description FROM sys.fn_helpcollations();  
    ```  
  
 **Para ver la configuración de intercalación de una base de datos**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, en la barra de herramientas, haga clic en **Nueva consulta**.  
  
2.  En la ventana de consulta, escriba la siguiente instrucción que usa la vista de catálogo del sistema sys.databases.  
  
    ```sql  
    SELECT name, collation_name FROM sys.databases;  
    ```  
  
3.  También puede usar la función del sistema DATABASEPROPERTYEX.  
  
    ```sql  
    SELECT CONVERT (varchar(256), DATABASEPROPERTYEX('database_name','collation'));  
    ```  
  
 **Para ver la configuración de intercalación de una columna**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, en la barra de herramientas, haga clic en **Nueva consulta**.  
  
2.  En la ventana de consulta, escriba la siguiente instrucción que usa la vista de catálogo del sistema sys.columns.  
  
    ```sql  
    SELECT name, collation_name FROM sys.columns WHERE name = N'<insert character data type column name>';  
    ```  
  
 **Para ver la configuración de intercalación de las tablas y columnas**  

1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, en la barra de herramientas, haga clic en **Nueva consulta**.  
  
2.  En la ventana de consulta, escriba la siguiente instrucción que usa la vista de catálogo del sistema sys.columns.  
  
    ```sql  
    SELECT t.name TableName, c.name ColumnName, collation_name  
    FROM sys.columns c  
    inner join sys.tables t on c.object_id = t.object_id;  
    ```  



## <a name="see-also"></a>Vea también  
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [Prioridad de intercalación &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md)   
 [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)      
 [sp_helpsort &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsort-transact-sql.md)  
  
  
