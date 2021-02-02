---
description: Modificar un esquema de partición
title: Modificación de un esquema de partición | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 515de63f-dfc5-434d-9adb-f3b5992f745a
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1cba23d0d043004f1f5a71d8e4af4844470ff163
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049111"
---
# <a name="modify-a-partition-scheme"></a>Modificar un esquema de partición
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Para modificar un esquema de partición en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , puede diseñar un grupo de archivos que contenga la siguiente partición que se agregará a la tabla con particiones mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para hacerlo debe asignar la propiedad NEXT USED a un grupo de archivos. Puede asignar la propiedad NEXT USED a un grupo de archivos vacío o a uno que ya contenga una partición. Es decir, un grupo de archivos puede tener más de una partición.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear una tabla o un índice con particiones, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Los grupos de archivos afectados por ALTER PARTITION SCHEME deben estar en línea.  

> [!NOTE]
> En Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] solamente se admiten grupos de archivos principales.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los siguientes permisos pueden utilizarse para ejecutar ALTER PARTITION SCHEME:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se ha creado el esquema de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se ha creado el esquema de partición.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para modificar un esquema de partición:**  
  
 Esta acción específica no se puede realizar mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para modificar un esquema de partición, primero debe eliminar el esquema y después crear uno nuevo con las propiedades deseadas mediante el Asistente para la creación de particiones. Para obtener más información, vea [Create Partitioned Tables and Indexes](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)[Using SQL Server Management Studio](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md#SSMSProcedure) en **Crear tablas e índices con particiones**.  
  
#### <a name="to-delete-a-partition-scheme"></a>Para eliminar un esquema de partición  
  
1.  Haga clic en el signo más para expandir la base de datos donde desea eliminar el esquema de partición.  
  
2.  Haga clic en el signo más para expandir la carpeta **Almacenamiento**.  
  
3.  Haga clic en el signo más para expandir la carpeta **Esquemas de partición** .  
  
4.  Haga clic con el botón derecho en el esquema de partición que quiere eliminar y seleccione **Eliminar**.  
  
5.  En el cuadro de diálogo **Eliminar objeto** , asegúrese de que está seleccionado el esquema de partición correcta y después haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-modify-a-partition-scheme"></a>Para modificar un esquema de partición  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- add five new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test5fg;  
    GO  
    -- if the "myRangePF1" partition function and the "myRangePS1" partition scheme exist,  
    -- drop them from the AdventureWorks2012 database  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
    DROP PARTITION FUNCTION myRangePF1;  
    GO  
    IF EXISTS (SELECT * FROM sys.partition_schemes  
        WHERE name = 'myRangePS1')  
    DROP PARTITION SCHEME myRangePS1;  
    GO  
    -- create the new partition function "myRangePF1" with four partition groups  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    -- create the new partition scheme "myRangePS1"that will use   
    -- the "myRangePF1" partition function with five file groups.  
    -- The last filegroup, "test5fg," will be kept empty but marked  
    -- as the next used filegroup in the partition scheme.  
    CREATE PARTITION SCHEME myRangePS1  
    AS PARTITION myRangePF1  
    TO (test1fg, test2fg, test3fg, test4fg, test5fg);  
    GO  
    --Split "myRangePS1" between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    GO  
    -- Allow the "myRangePS1" partition scheme to use the filegroup "test5fg"  
    -- for the partition with boundary_values of 100 and 500  
    ALTER PARTITION SCHEME myRangePS1  
    NEXT USED test5fg;  
    GO  
    ```  
  
 Para obtener más información, vea [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md).  
  
  
