---
description: Mover un índice existente a un grupo de archivos diferente
title: Movimiento de un índice existente a un grupo de archivos diferente | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- moving tables
- switching filegroups for index
- moving indexes
- indexes [SQL Server], moving
- filegroups [SQL Server], switching
ms.assetid: 167ebe77-487d-4ca8-9452-4b2c7d5cb96e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4ccacb2b46b609b61485821f250644596f0e3955
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048888"
---
# <a name="move-an-existing-index-to-a-different-filegroup"></a>Mover un índice existente a un grupo de archivos diferente
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo mover un índice existente de su grupo de archivos actual a otro distinto en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para mover un índice existente a un grupo de archivos diferente, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si una tabla base tiene un índice clúster, al mover el índice clúster a un nuevo grupo de archivos se mueve también la tabla a ese grupo de archivos.  
  
-   No puede mover los índices creados mediante una restricción UNIQUE o PRIMARY KEY con [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Para mover estos índices, use la instrucción [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) con la opción (DROP_EXISTING=ON) en [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup-using-table-designer"></a>Para mover un índice existente a un grupo de archivos diferente usando el Diseñador de tablas  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla que contiene el índice que desea mover.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic con el botón derecho en la tabla que contiene el índice que quiere mover y seleccione **Diseño**.  
  
4.  En el menú **Diseñador de tablas** , haga clic en **Índices o claves**.  
  
5.  Seleccione el índice que desea mover.  
  
6.  En la cuadrícula principal, expanda **Especificación de espacio de datos**.  
  
7.  Seleccione **Nombre de esquema de partición o grupo de archivos** y seleccione en la lista el esquema de grupo de archivos o de partición al que desea mover el índice.  
  
8.  Haga clic en **Cerrar**.  
  
9. En el menú **Archivo** , seleccione **Guardar**_nombre_tabla_.  

#### <a name="to-move-an-existing-index-to-a-different-filegroup-in-object-explorer"></a>Para mover un índice existente a un grupo de archivos distinto en el Explorador de objetos  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla que contiene el índice que desea mover.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla que contiene el índice que desea mover.  
  
4.  Haga clic en el signo más para expandir la carpeta **Índices** .  
  
5.  Haga clic con el botón derecho en el índice que quiere mover y seleccione **Propiedades**.  
  
6.  En **Seleccionar una página**, seleccione **Almacenamiento**.  
  
7.  Seleccione el grupo de archivos al que desee mover el índice.  
  
     Si la tabla o el índice tienen particiones, seleccione el esquema de particiones al que desee mover el índice. Para obtener más información acerca de los índices con particiones, vea [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
     Si lo que desea mover es un índice clúster, puede utilizar el procesamiento en línea. El procesamiento en línea permite que varios usuarios obtengan acceso al mismo tiempo a los datos subyacentes, así como a índices no clúster durante la operación de índice. Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
     En equipos multiprocesador que usan [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], puede configurar el número de procesadores que desea usar para ejecutar la instrucción de índice; para ello, especifique un valor máximo de grado de paralelismo. La característica Operaciones indizadas en paralelo no está disponible en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea Características compatibles con las ediciones de SQL Server 2016. Para obtener más información sobre las operaciones indexadas en paralelo, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
8.  Haga clic en **Aceptar**.  
  
 La información siguiente está disponible en la página **Almacenamiento** del cuadro de diálogo **Propiedades del índice -** _nombre de índice_:  
  
 **Grupo de archivos**  
 Almacena el índice en el grupo de archivos especificado. En la lista solo se muestran los grupos de archivos (fila) estándar. La selección de lista predeterminada es el grupo de archivos PRIMARY de la base de datos.  
  
 **Grupo de archivos de flujo de archivos**  
 Especifica el grupo de archivos para los datos FILESTREAM. En esta lista solo se muestran los grupos de archivos FILESTREAM. La selección de lista predeterminada es el grupo de archivos PRIMARY FILESTREAM.  
  
 **Esquema de partición**  
 Almacena el índice en un esquema de partición. Al hacer clic en **Esquema de partición** , se habilita la cuadrícula que se muestra a continuación. La selección de lista predeterminada es el esquema de partición utilizado para almacenar los datos de la tabla. Si se selecciona un esquema de partición distinto de la lista, se actualizará la información en la cuadrícula.  
  
 La opción de esquema de partición no estará disponible si no hay ningún esquema de partición en la base de datos.  
  
 **Esquema de partición Filestream**  
 Especifica el esquema de partición de los datos FILESTREAM. El esquema de partición debe ser simétrico al esquema especificado en la opción **Esquema de partición** .  
  
 Si no tiene particiones, el campo está en blanco.  
  
 **Parámetro del esquema de partición**  
 Muestra el nombre de la columna que participa en el esquema de partición.  
  
 **Columna de la tabla**  
 Seleccione la tabla o vista que se asignará al esquema de partición.  
  
 **Tipo de datos de la columna**  
 Muestra información de tipo de datos de la columna.  
  
> [!NOTE]  
>  Si la columna de tabla es una columna calculada, **Tipo de datos de la columna** mostrará "columna calculada".  
  
 **Permitir procesamiento en línea de instrucciones DML al mover el índice**  
 Permite a los usuarios obtener acceso a los datos de la tabla subyacente o del índice clúster, así como a todos los índices no clúster asociados durante las operaciones de índice.  
  
> [!NOTE]  
>  Esta opción no estará disponible para los índices XML ni cuando el índice sea un índice clúster deshabilitado.  
  
 **Establecer grado máximo de paralelismo**  
 Limita el número de procesadores que se van a utilizar durante la ejecución de planes paralelos. El valor predeterminado es 0, que utiliza el número real de CPU disponibles. Si el valor se establece en 1, se suprime la generación de planes paralelos; si el valor se establece en un número mayor que 1, se restringe el número máximo de procesadores que se utilizan en la ejecución de una única consulta. Esta opción solo está disponible si el cuadro de diálogo está en los estados **Volver a generar** o **Volver a crear** .  
  
> [!NOTE]  
>  Si especifica un valor superior al número de CPU disponibles, se utilizará el número real de CPU disponibles.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-move-an-existing-index-to-a-different-filegroup"></a>Para mover un índice existente a un grupo de archivos diferente  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Creates the TransactionsFG1 filegroup on the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP TransactionsFG1;  
    GO  
    /* Adds the TransactionsFG1dat3 file to the TransactionsFG1 filegroup. Please note that you will have to change the filename parameter in this statement to execute it without errors.  
    */  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = TransactionsFG1dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\DATA\TransactionsFG1dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP TransactionsFG1;  
    GO  
    /*Creates the IX_Employee_OrganizationLevel_OrganizationNode index  
      on the TransactionsPS1 filegroup and drops the original IX_Employee_OrganizationLevel_OrganizationNode index.  
    */  
    CREATE NONCLUSTERED INDEX IX_Employee_OrganizationLevel_OrganizationNode  
        ON HumanResources.Employee (OrganizationLevel, OrganizationNode)  
        WITH (DROP_EXISTING = ON)  
        ON TransactionsFG1;  
    GO  
    ```  
  
 Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
