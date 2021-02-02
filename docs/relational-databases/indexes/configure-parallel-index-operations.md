---
description: Configurar operaciones de índice en paralelo
title: Configuración de operaciones de índice en paralelo | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index parallel operations [SQL Server]
- processors [SQL Server], parallel index operations
- max degree of parallelism option
- MAXDOP index option, parallel index operations
- parallel index operations [SQL Server]
ms.assetid: 8ec8c71e-5fc1-443a-92da-136ee3fc7f88
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41f1cb29f7bd1671166c7eaa0504a7c22c796a30
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048994"
---
# <a name="configure-parallel-index-operations"></a>Configurar operaciones de índice en paralelo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

En este tema se define el grado máximo de paralelismo y se explica cómo modificar este valor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

En los sistemas multiprocesador que ejecutan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise o superior, las instrucciones de índice pueden usar varios procesadores (CPU) para realizar las operaciones de examen, ordenación e indización asociadas a la instrucción de índice, al igual que hacen otras consultas. El número de CPU que se usan para ejecutar una sola instrucción de índice viene determinado por la opción de configuración del servidor [Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md), la carga de trabajo actual y las estadísticas de índices. La opción max degree of parallelism determina el número máximo de procesadores que se utilizarán en la ejecución de planes paralelos. Si [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] detecta que el sistema está ocupado, el grado de paralelismo de la operación de índice se reduce automáticamente antes de comenzar la ejecución de la instrucción. [!INCLUDE[ssDE](../../includes/ssde-md.md)] también puede reducir el grado de paralelismo si la columna de clave inicial de un índice sin particiones tiene un número limitado de valores distintos o la frecuencia de cada valor distinto varía significativamente. Para obtener más información, consulte [Guía de arquitectura de procesamiento de consulta](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing). 
  
> [!NOTE]  
> Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para establecer el grado máximo de paralelismo, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El número de procesadores que utiliza el optimizador de consultas suele proporcionar un rendimiento óptimo. No obstante, las operaciones como la creación, reconstrucción o eliminación de índices de gran tamaño utilizan una gran cantidad de recursos y pueden provocar la falta de recursos para otras aplicaciones y operaciones de base de datos durante la operación de índice. Cuando se produce este problema, se puede configurar manualmente el número máximo de procesadores que se emplean para ejecutar la instrucción de índice limitando el número de procesadores que se usarán para la operación de índice.  
  
-   La opción de índice MAXDOP reemplaza la opción de configuración max degree of parallelism solo para la consulta que especifica esta opción. En la tabla siguiente se muestran los valores enteros válidos que se pueden especificar con la opción de configuración max degree of parallelism y la opción de índice MAXDOP.  
  
    |Value|Descripción|  
    |-----------|-----------------|  
    |0|Especifica que el servidor determina el número de CPU que se usan, según la carga de trabajo del sistema actual. Éste es el valor predeterminado y recomendado.|  
    |1|Suprime la generación de planes paralelos. La operación se ejecutará en serie.|  
    |2-64|Limita el número de procesadores al valor especificado. Puede que se utilicen menos procesadores, dependiendo de la carga de trabajo actual. Si especifica un valor superior al número de CPU disponibles, se utilizará el número real de CPU disponibles.|  
  
-   La ejecución de índices en paralelo y la opción de índice MAXDOP se aplican a las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes:  
  
    -   [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)  
  
    -   [ALTER INDEX (…) REBUILD](../../t-sql/statements/alter-index-transact-sql.md)  
  
    -   [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) (Solo se aplica a los índices agrupados).  
  
    -   [ALTER TABLE ADD (índice) CONSTRAINT](../../t-sql/statements/alter-table-table-constraint-transact-sql.md) 
  
    -   [ALTER TABLE DROP (índice agrupado) CONSTRAINT](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
  
-   La opción de índice MAXDOP no se puede especificar en la instrucción `ALTER INDEX (...) REORGANIZE`.  
  
-   Los requisitos de memoria de las operaciones de índices con particiones que requieren ordenación pueden ser mayores si el Optimizador de consultas aplica grados de paralelismo a la operación de generación. Cuanto mayores sean los grados de paralelismo, mayor será el requisito de memoria. Para obtener más información, vea [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
###  <a name="permissions"></a><a name="Security"></a> <a name="Permissions"></a> Permisos  
 Debe tener un permiso de `ALTER` sobre la tabla o vista.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-set-max-degree-of-parallelism-on-an-index"></a>Para establecer el grado máximo de paralelismo en un índice  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que desea establecer el grado máximo de paralelismo para un índice.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que desea establecer el grado máximo de paralelismo para un índice.  
  
4.  Expanda la carpeta **Índices** .  
  
5.  Haga clic con el botón derecho en el índice para el que quiere establecer el grado máximo de paralelismo y seleccione **Propiedades**.  
  
6.  Debajo de **Seleccionar una página**, seleccione **Opciones**.  
  
7.  Seleccione **Grado máximo de paralelismo** y, a continuación, escriba un valor entre 1 y 64.  
  
8.  Haga clic en **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-set-max-degree-of-parallelism-on-an-existing-index"></a>Para establecer el grado máximo de paralelismo en un índice existente  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    /*Alters the IX_ProductVendor_VendorID index on the Purchasing.ProductVendor table so that, if the server has eight or more processors, the Database Engine will limit the execution of the index operation to eight or fewer processors.  
    */  
    ALTER INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor  
    REBUILD WITH (MAXDOP=8);   
    GO  
    ```  
  
 Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
#### <a name="set-max-degree-of-parallelism-on-a-new-index"></a>Establecer el grado máximo de paralelismo en un nuevo índice  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    CREATE INDEX IX_ProductVendor_NewVendorID   
    ON Purchasing.ProductVendor (BusinessEntityID)  
    WITH (MAXDOP=8);  
    GO  
    ```  
 
## <a name="see-also"></a>Consulte también
[Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing)   (Guía de arquitectura de procesamiento de consultas)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)     
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)      
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)      
[ALTER TABLE table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)       
[ALTER TABLE index_option &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-index-option-transact-sql.md)    
