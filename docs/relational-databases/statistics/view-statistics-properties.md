---
title: Ver propiedades de estadísticas | Microsoft Docs
description: Obtenga más información sobre cómo mostrar estadísticas de optimización de consultas actuales para una tabla o vista indizada en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.statistics.details.f1
helpviewer_keywords:
- viewing statistics properties
- statistics [SQL Server], viewing properties
ms.assetid: 0eaa2101-006e-4015-9979-3468b50e0aaa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 440b2e3dd7bcf7d1c7934a32ff95f9cd2299602d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250233"
---
# <a name="view-statistics-properties"></a>Ver propiedades de estadísticas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Puede mostrar las estadísticas de optimización de consultas actuales para una tabla o vista indizada en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Los objetos de estadísticas incluyen un encabezado con metadatos sobre las estadísticas, un histograma con la distribución de valores de la primera columna de clave del objeto de estadísticas y un vector de la densidad para medir la correlación entre las columnas. Para obtener más información sobre histogramas y vectores de densidad, vea [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para ver propiedades de estadísticas, mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para ver el objeto de estadísticas, el usuario debe ser propietario de la tabla o miembro del rol fijo de servidor **sysadmin** , del rol fijo de base de datos **db_owner** o del rol fijo de base de datos **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-statistics-properties"></a>Para ver las propiedades de estadísticas  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir la base de datos en la que desea crear una nueva estadística.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que quiere ver las propiedades de las estadísticas.  
  
4.  Haga clic en el signo más para expandir la carpeta **Estadísticas** .  
  
5.  Haga clic con el botón derecho en el objeto Estadísticas cuyas propiedades quiere ver y seleccione **Propiedades**.  
  
6.  En el cuadro de diálogo **Propiedades de estadísticas -** _nombre de estadísticas_, en el panel **Seleccionar una página**, seleccione **Detalles**.  
  
     Las propiedades siguientes se muestran en la página **Detalles** del cuadro de diálogo **Propiedades de estadísticas -** _nombre de estadísticas_.  
  
     **Nombre de tabla**  
     Muestra el nombre de la tabla descrita por las estadísticas.  
  
     **Nombre de las estadísticas**  
     Muestra el nombre del objeto de base de datos donde se almacenan las estadísticas.  
  
     **Estadísticas para INDEXnombre_de_estadísticas**  
     Este cuadro de texto muestra las propiedades devueltas del objeto de estadísticas. Estas propiedades se dividen en tres secciones: Encabezado de estadísticas, Vector de densidad e Histograma.  
  
     La siguiente información se describen las columnas devueltas en el conjunto de resultados para el encabezado de estadísticas.  
  
     **Nombre**  
     Nombre del objeto de estadísticas.  
  
     **Updated**  
     Fecha y hora de la última actualización de las estadísticas.  
  
     **Filas**  
     Número total de filas que tenía la tabla o vista indizada la última vez que se actualizaron las estadísticas. Si las estadísticas se filtran o corresponden a un índice filtrado, el número de filas puede ser inferior al número de filas de la tabla.  
  
     **Rows Sampled**  
     Número total de filas muestreadas para cálculos de estadísticas. Si Rows Sampled < Rows, el histograma y los resultados de la densidad que se muestren serán estimaciones extraídas de las filas muestreadas.  
  
     **Pasos**  
     Número de pasos del histograma. Cada paso abarca un intervalo de valores de columna seguido de un valor de columna límite superior. Los pasos del histograma se definen en la primera columna de clave de las estadísticas. El número máximo de pasos es 200.  
  
     **Densidad**  
     Se calcula como 1 / *valores distintos* en todos los valores de la primera columna de clave del objeto de estadísticas, excepto en los valores límite del histograma. El optimizador de consultas no usa este valor de densidad y solo se muestra por motivos de compatibilidad con versiones anteriores a SQL Server 2008.  
  
     **Promedio de longitud de clave**  
     Número promedio de bytes por cada uno de los valores de las columnas de clave del objeto de estadísticas.  
  
     **String Index**  
     Sí indica que el objeto de estadísticas contiene estadísticas de resumen de las cadenas para mejorar los cálculos de cardinalidad de los predicados de consulta que utilizan el operador LIKE; por ejemplo, `WHERE ProductName LIKE '%Bike'`. Las estadísticas de resumen de cadenas se almacenan de forma independiente del histograma y se crean en la primera columna de clave del objeto de estadísticas cuando es de tipo **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** o **ntext**.  
  
     **Expresión de filtro**  
     Predicado del subconjunto de filas de la tabla incluido en el objeto de estadísticas. NULL = Estadísticas no filtradas.  
  
     **Filas sin filtrar**  
     Número total de filas de la tabla antes de aplicar la expresión de filtro. Si Expresión de filtro es NULL, las Filas sin filtrar son iguales a Filas.  
  
     La siguiente información describe las columnas devueltas en el conjunto de resultados del vector de densidad.  
  
     **Toda la densidad**  
     La densidad es 1 / *valores distintos*. Los resultados muestran la densidad de cada prefijo de columnas del objeto de estadísticas (una fila por cada densidad). Un valor distinto es una lista Distinct de los valores de columna de cada fila y prefijo de columna. Por ejemplo, si el objeto de estadísticas contiene las columnas de clave (A, B, C), los resultados indican la densidad de las listas de valores distintos de cada uno de estos prefijos de columna: (A), (A,B) y (A, B, C). Si se usa el prefijo (A, B, C), cada una de estas listas es una lista de valores distintos: (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Si se usa el prefijo (A, B) los valores de la misma columna tendrán estas listas de valores distintos: (3, 5), (4, 4) y (4, 5).  
  
     **Promedio de longitud**  
     Promedio de longitud, en bytes, para almacenar una lista de los valores de columna del prefijo de columna. Por ejemplo, si cada valor de la lista (3, 5, 6) necesita 4 bytes, la longitud es 12 bytes.  
  
     **Columnas**  
     Nombres de las columnas en el prefijo para las que se muestran Toda la densidad y Promedio de longitud.  
  
     La siguiente información describe las columnas devueltas en el conjunto de resultados del histograma.  
  
     **RANGE_HI_KEY**  
     Valor de columna límite superior de un paso del histograma. El valor de columna también se denomina valor de clave.  
  
     **RANGE_ROWS**  
     Número calculado de filas cuyo valor de columna está comprendido en un paso del histograma, sin incluir el límite superior.  
  
     **EQ_ROWS**  
     Número calculado de filas cuyo valor de columna es igual al límite superior del paso del histograma.  
  
     **DISTINCT_RANGE_ROWS**  
     Número calculado de filas que tienen un valor de columna distinto en un paso del histograma, sin incluir el límite superior.  
  
     **AVG_RANGE_ROWS**  
     Número medio de filas que tienen valores de columna duplicados en un paso del histograma, sin incluir el límite superior (RANGE_ROWS/DISTINCT_RANGE_ROWS para DISTINCT_RANGE_ROWS > 0).  
  
7.  Haga clic en **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-view-statistics-properties"></a>Para ver las propiedades de estadísticas  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- The following example displays all statistics information for the AK_Address_rowguid index of the Person.Address table.   
    DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);   
    GO  
    ```  
  
 Para obtener más información, vea [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
#### <a name="to-find-all-of-the-statistics-on-a-table-or-view"></a>Para buscar todas las estadísticas de una tabla o vista  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    /*Gets the following information: name and ID of the statistics, whether the statistics were created automatically or by the user, whether the statistics were created with the NORECOMPUTE option, and whether the statistics have a filter and, if so, what that filter is.  
    */  
    SELECT name AS statistics_name  
        ,stats_id  
        ,auto_created  
        ,user_created  
        ,no_recompute  
        ,has_filter  
        ,filter_definition  
    -- using the sys.stats catalog view  
    FROM sys.stats  
    -- for the Sales.SpecialOffer table  
    WHERE object_id = OBJECT_ID('Sales.SpecialOffer');  
    GO  
    ```  
  
 Para obtener más información, vea [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).  
  
  
