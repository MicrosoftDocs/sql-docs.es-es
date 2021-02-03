---
title: Actualizar estadísticas | Microsoft Docs
description: Obtenga más información sobre cómo actualizar las estadísticas de optimización de consultas en una tabla o vista indizada en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- updating statistics
- statistics [SQL Server], updating
ms.assetid: 4b97c0b4-03ff-4cfb-9c3f-3b33290b7a2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e503301eef971cc5eec21ddf4e96caf2389923dc
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250254"
---
# <a name="update-statistics"></a>Actualizar estadísticas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Puede actualizar las estadísticas de optimización de consultas en una tabla o en una vista indizada de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. De forma predeterminada, el optimizador de consultas ya actualiza las estadísticas como requisito para mejorar el plan de consulta; en algunos casos puede mejorar el rendimiento de las consultas utilizando UPDATE STATISTICS o el procedimiento almacenado `sp_updatestats` para actualizar las estadísticas con más frecuencia que la de las actualizaciones predeterminadas.  
  
 La actualización de las estadísticas asegura que las consultas se compilan con estadísticas actualizadas. Sin embargo, la actualización de las estadísticas hace que las consultas se vuelvan a compilar. Recomendamos no actualizar las estadísticas con demasiada frecuencia, porque hay que elegir el punto válido entre la mejora de los planes de consulta y el tiempo empleado en volver a compilar las consultas. Las compensaciones específicas dependen de su aplicación. UPDATE STATISTICS puede usar tempdb para ordenar la muestra de filas con fines de creación de estadísticas.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para actualizar un objeto de estadísticas, con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Si usa UPDATE STATISTICS o realiza cambios con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], es necesario el permiso ALTER en la tabla o vista. Si usa `sp_updatestats`, necesita pertenecer al rol fijo de servidor **sysadmin** o ser propietario de la base de datos (**dbo**).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-update-a-statistics-object"></a>Para actualizar un objeto de estadísticas  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir la base de datos en la que desea actualizar la estadística.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que desea actualizar la estadística.  
  
4.  Haga clic en el signo más para expandir la carpeta **Estadísticas** .  
  
5.  Haga clic con el botón derecho en el objeto de estadísticas que quiere actualizar y seleccione **Propiedades**.  
  
6.  En el cuadro de diálogo **Propiedades de estadísticas -** _nombre\_estadísticas_, active la casilla **Actualizar estadísticas de estas columnas** y haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
### <a name="to-update-a-specific-statistics-object"></a>Para actualizar un objeto concreto de estadísticas  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- The following example updates the statistics for the AK_SalesOrderDetail_rowguid index of the SalesOrderDetail table.   
    UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;   
    GO  
    ```  
  
### <a name="to-update-all-statistics-in-a-table"></a>Para actualizar todas las estadísticas en una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    -- The following example updates the statistics for all indexes on the SalesOrderDetail table.   
    UPDATE STATISTICS Sales.SalesOrderDetail;   
    GO  
    ```  
  
Para obtener más información, vea [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md).  
  
### <a name="to-update-all-statistics-in-a-database"></a>Para actualizar todas las estadísticas de una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    -- The following example updates the statistics for all tables in the database.   
    EXEC sp_updatestats;  
    ```  

Para obtener más información, vea [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md).   

### <a name="automatic-index-and-statistics-management"></a>Administración automática de índice y estadísticas
Aproveche soluciones como la [desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) para administrar automáticamente las actualizaciones de estadísticas y la desfragmentación de índices para una o varias bases de datos. Este procedimiento elige automáticamente si se debe volver a generar o reorganizar un índice según su nivel de fragmentación, entre otros parámetros y actualiza las estadísticas con un umbral lineal.

