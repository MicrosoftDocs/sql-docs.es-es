---
title: Nueva compilación de un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo volver a compilar un procedimiento almacenado en SQL Server 2019 (15.x) mediante Transact-SQL.
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- sp_recompile
- WITH RECOMPILE clause
- recompiling stored procedures
- stored procedures [SQL Server], recompiling
ms.assetid: b90deb27-0099-4fe7-ba60-726af78f7c18
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93974ff7f7ef5d0dd28ae8ce6989da2f0b496548
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250252"
---
# <a name="recompile-a-stored-procedure"></a>Volver a compilar un procedimiento almacenado
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  En este tema se describe cómo volver a compilar un procedimiento almacenado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. Hay tres formas de hacerlo: la opción **WITH RECOMPILE** en la definición del procedimiento o cuando se llama al procedimiento, la sugerencia de consulta **RECOMPILE** en instrucciones individuales o usando el procedimiento almacenado del sistema **sp_recompile** . En este tema se describe el uso de la opción WITH RECOMPILE al crear una definición de procedimiento y ejecutar un procedimiento existente. También se describe el uso del procedimiento almacenado del sistema sp_recompile para volver a compilar un procedimiento existente.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para volver a compilar un procedimiento almacenado, usando:**  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Cuando un procedimiento se compila por primera vez o se vuelve a compilar, el plan de consulta del procedimiento se optimiza para el estado actual de la base de datos y sus objetos. Si una base de datos experimenta cambios significativos en sus datos o su estructura, al volver a compilar un procedimiento se actualiza y optimiza el plan de consulta del procedimiento para tener en cuenta esos cambios. Esto puede mejorar el rendimiento de procesamiento del procedimiento.  
  
-   Hay ocasiones en las que se debe forzar la nueva compilación del procedimiento y otras en que se realiza automáticamente. La nueva compilación automática tiene lugar siempre que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También se produce si una tabla subyacente a la que hace referencia el procedimiento ha experimentado cambios de diseño físico.  
  
-   Otro motivo para forzar la nueva compilación de un procedimiento es contrarrestar el comportamiento de "examen de parámetros" de la compilación de procedimientos. Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta procedimientos, los valores de parámetros usados por el procedimiento cuando se compila se incluyen como parte de la generación del plan de consulta. Si esos valores representan los valores típicos con los que se llama al procedimiento posteriormente, el procedimiento se beneficia del plan de consulta cada vez que se compila y se ejecuta. Si los valores de parámetro del procedimiento suelen ser atípicos, forzar una nueva compilación del procedimiento y un nuevo plan basándose en diferentes valores de parámetro puede mejorar el rendimiento.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incorpora nueva compilación de nivel de instrucciones de los procedimientos. Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a compilar procedimientos almacenados, solo se compila la instrucción que ha causado la nueva compilación, en lugar de todo el procedimiento.  
  
-   Si algunas consultas de un procedimiento suelen usar valores atípicos o temporales, se puede mejorar el rendimiento del procedimiento con la sugerencia de consulta RECOMPILE dentro de esas consultas. Puesto que solo se volverán a compilar las consultas que usan la sugerencia de consulta en lugar del procedimiento completo, se imita el comportamiento de nueva compilación de nivel de instrucciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Además de usar los valores de parámetro actuales del procedimiento, la sugerencia de consulta RECOMPILE también emplea los valores de cualquier variable local del procedimiento almacenado al compilar la instrucción. Para obtener más información, vea [Sugerencias de consulta (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Opción **WITH RECOMPILE**  
 Si se usa esta opción cuando se crea la definición del procedimiento, se necesita el permiso CREATE PROCEDURE en la base de datos y el permiso ALTER en el esquema en el que se crea el procedimiento.  
  
 Si se usa esta opción en una instrucción EXECUTE, se necesitan permisos EXECUTE para el procedimiento. No se necesitan permisos en la propia instrucción EXECUTE, pero sí se necesitan permisos de ejecución en el procedimiento al que se hace referencia en la instrucción EXECUTE. Para obtener más información, vea [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Sugerencia de consulta **RECOMPILE**  
 Esta característica se emplea cuando se crea el procedimiento y la sugerencia se incluye en las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] del procedimiento. Por tanto, necesita el permiso CREATE PROCEDURE en la base de datos y el permiso ALTER en el esquema en el que se va a crear el procedimiento.  
  
 Ejecutar el procedimiento almacenado del sistema **sp_recompile**  
 Necesita el permiso ALTER en el procedimiento especificado.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  

1. Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
1. En la barra Estándar, haga clic en **Nueva consulta**.  
  
1. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se crea la definición del procedimiento.  

   ```sql
   USE AdventureWorks2012;  
   GO  
   IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL   
       DROP PROCEDURE dbo.uspProductByVendor;  
   GO  
   CREATE PROCEDURE dbo.uspProductByVendor @Name varchar(30) = '%'  
   WITH RECOMPILE  
   AS  
       SET NOCOUNT ON;  
       SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'  
       FROM Purchasing.Vendor AS v   
       JOIN Purchasing.ProductVendor AS pv   
         ON v.BusinessEntityID = pv.BusinessEntityID   
       JOIN Production.Product AS p   
         ON pv.ProductID = p.ProductID  
       WHERE v.Name LIKE @Name;  
   ```  
  
### <a name="to-recompile-a-stored-procedure-by-using-the-with-recompile-option"></a>Para volver a compilar un procedimiento almacenado usando la opción WITH RECOMPILE   
  
Seleccione **Nueva consulta**. Luego, copie y pegue el ejemplo de código siguiente en la ventana de consulta y haga clic en **Ejecutar**. Esto ejecutará el procedimiento y volverá a compilar el plan de consulta del procedimiento.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE HumanResources.uspProductByVendor WITH RECOMPILE;  
GO
```  
  
### <a name="to-recompile-a-stored-procedure-by-using-sp_recompile"></a>Para volver a compilar un procedimiento almacenado mediante sp_recompile  

Seleccione **Nueva consulta**. Luego, copie y pegue el ejemplo siguiente en la ventana de consulta y haga clic en **Ejecutar**. Esto no ejecuta el procedimiento, sino que lo marca para que se vuelva a compilar de forma que su plan de consulta se actualice la próxima vez que se ejecute el procedimiento.  

```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'dbo.uspProductByVendor';   
GO
```  
  
## <a name="see-also"></a>Consulte también  
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modificar un procedimiento almacenado](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Cambiar el nombre de un procedimiento almacenado](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [Ver la definición de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Ver las dependencias de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
  
