---
description: Especificar el factor de relleno para un índice
title: Especificar el factor de relleno para un índice | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- fill factor [SQL Server]
- page splits [SQL Server]
ms.assetid: 237a577e-b42b-4adb-90cf-aa7fb174f3ab
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: e1e0a2b406d6817c43a5a4a51bea90b556c5cea6
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048780"
---
# <a name="specify-fill-factor-for-an-index"></a>Especificar el factor de relleno para un índice
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Este tema describe qué es el factor de relleno y cómo especificar un valor de factor de relleno en un índice de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 La opción de factor de relleno permite optimizar el almacenamiento y rendimiento de los datos de índice. Cuando se crea o se vuelve a generar un índice, el valor de factor de relleno determina el porcentaje de espacio en cada página de nivel de hoja que se tiene que rellenar con datos, por lo que se reserva el resto de cada página como espacio disponible para seguir creciendo. Por ejemplo, si se especifica un valor de factor de relleno de 80, significa que el 20 por ciento de cada página de nivel de hoja se dejará vacío para proporcionar espacio para la expansión del índice a medida que se agreguen datos a la tabla subyacente. El espacio vacío se reserva entre las filas de índice en lugar de al final del índice.  
  
 El valor de factor de relleno es un porcentaje entre 1 y 100, y el valor predeterminado en el servidor es 0, lo que significa que las páginas de nivel de hoja se rellenan al máximo de su capacidad.  
  
> [!NOTE]  
>  Los valores del factor de relleno 0 y 100 son idénticos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Consideraciones de rendimiento](#Performance)  
  
     [Seguridad](#Security)  
  
-   **Para especificar un factor de relleno en un índice, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="performance-considerations"></a><a name="Performance"></a> Consideraciones de rendimiento  
  
#### <a name="page-splits"></a>Divisiones de páginas  
 Si el valor de factor de relleno se elige correctamente, se pueden reducir las posibles divisiones de página al proporcionarse espacio suficiente para la expansión del índice a medida que se agregan datos a la tabla subyacente. Cuando se agrega una fila nueva a una página de índice llena, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] mueve aproximadamente la mitad de las filas a una página nueva con el fin de hacer espacio para la nueva fila. Esta reorganización se denomina división de página. Una división de página genera espacio para los nuevos registros, pero puede tardar en realizarse y es una operación que consume muchos recursos. Además, puede causar fragmentación que, a su vez, causa más operaciones de E/S. Cuando se llevan a cabo divisiones de páginas con frecuencia, el índice se puede volver a generar utilizando un valor de factor de relleno nuevo o existente para redistribuir los datos. Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Aunque si el valor del factor de relleno es bajo y distinto de cero, se puede reducir el requisito de dividir páginas a medida que crezca el índice, éste necesitará más espacio de almacenamiento y puede reducir el rendimiento de la lectura. Incluso en el caso de una aplicación pensada para realizar muchas operaciones de inserción y actualización, el número de lecturas de base de datos suele superar a las escrituras en un factor de 5 a 10. Por lo tanto, al especificar un factor de relleno distinto del predeterminado, se puede disminuir el rendimiento de las lecturas de base de datos en una cantidad inversamente proporcional al valor del factor de relleno. Por ejemplo, un valor de factor de relleno igual a 50 puede hacer que el rendimiento de las lecturas de base de datos se reduzca a la mitad. El rendimiento de las lecturas se reduce porque el índice contiene más páginas, con lo que se aumentan las operaciones de E/S de disco necesarias para recuperar los datos.  
  
#### <a name="adding-data-to-the-end-of-the-table"></a>Agregar datos al final de la tabla  
 Un factor de relleno distinto de cero o de cien puede ser conveniente para el rendimiento si los datos nuevos se distribuyen uniformemente en toda la tabla. Sin embargo, si todos los datos se agregan al final de la tabla, el espacio vacío en las páginas de índice no se rellenará. Por ejemplo, si la columna de clave de índice es una columna IDENTITY, la clave de las nuevas filas siempre aumenta y las filas de índice se agregan lógicamente al final del índice. Si las filas existentes se van a actualizar con datos que hacen aumentar el tamaño de las filas, use un factor de relleno menor que 100. Los bytes adicionales en cada página ayudarán a reducir las divisiones de página ocasionadas por la longitud extra de las filas.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-specify-a-fill-factor-by-using-table-designer"></a>Para especificar un factor de relleno con el Diseñador de tablas  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que quiera especificar el factor de relleno de un índice.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic con el botón derecho en la tabla en la que quiere especificar el factor de relleno de un índice y seleccione **Diseño**.  
  
4.  En el menú **Diseñador de tablas** , haga clic en **Índices o claves**.  
  
5.  Seleccione el índice con el factor de relleno que desea especificar.  
  
6.  Expanda **Especificación de relleno**, seleccione la fila **Factor de relleno** y escriba el factor de relleno que desee en la fila.  
  
7.  Haga clic en **Cerrar**.  
  
8.  En el menú **Archivo** , seleccione **Guardar**_nombre_tabla_.  
  
#### <a name="to-specify-a-fill-factor-in-an-index-by-using-object-explorer"></a>Para especificar un factor de relleno en un índice mediante el Explorador de objetos  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que quiera especificar el factor de relleno de un índice.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que quiere especificar el factor de relleno de un índice.  
  
4.  Haga clic en el signo más para expandir la carpeta **Índices** .  
  
5.  Haga clic con el botón derecho en el índice con el factor de relleno que quiere especificar y seleccione **Propiedades**.  
  
6.  Debajo de **Seleccionar una página**, seleccione **Opciones**.  
  
7.  En la fila **Factor de relleno** , escriba el factor de relleno que desee.  
  
8.  Haga clic en **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-specify-a-fill-factor-in-an-existing-index"></a>Para especificar un factor de relleno en un índice existente  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En el ejemplo se vuelve a generar un índice existente y se aplica el factor de relleno especificado durante la operación de nueva generación.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Rebuilds the IX_Employee_OrganizationLevel_OrganizationNode index   
    -- with a fill factor of 80 on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD WITH (FILLFACTOR = 80);   
    GO  
    ```  
  
#### <a name="another-way-to-specify-a-fill-factor-in-an-index"></a>Otra manera de especificar un factor de relleno de un índice  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    /* Drops and re-creates the IX_Employee_OrganizationLevel_OrganizationNode index on the HumanResources.Employee table with a fill factor of 80.  
    */  
  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)   
    WITH (DROP_EXISTING = ON, FILLFACTOR = 80);   
    GO  
    ```  
  
 Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
