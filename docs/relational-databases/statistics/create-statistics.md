---
title: Creación de estadísticas | Microsoft Docs
description: Obtenga más información sobre cómo crear estadísticas de optimización de consultas en las columnas de una tabla o vista indizada en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.stat.properties.f1
- sql13.swb.statistics.filter.f1
- sql13.swb.stat.columns.f1
- sql13.swb.statistics.propertis.f1
helpviewer_keywords:
- creating statistics
- statistics [SQL Server], creating
ms.assetid: 95a455fb-664d-4c95-851e-c6b62d7ebe04
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba784c0c2fc9733f482a5af09870b48951c6079d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250324"
---
# <a name="create-statistics"></a>Crear estadísticas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Puede crear estadísticas de optimización de consultas en una o más columnas de una tabla o vista indizada en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para la mayoría de las consultas, el optimizador de consultas ya genera las estadísticas necesarias para un plan de consulta de alta calidad; en algunos casos, para obtener los mejores resultados es necesario crear estadísticas adicionales.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear estadísticas con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Antes de crear estadísticas con la instrucción CREATE STATISTICS, compruebe que la opción AUTO_CREATE_STATISTICS está establecida en la base de datos. De este modo se asegurará de que el optimizador de consultas continúe creando rutinariamente estadísticas de una sola columna para las columnas del predicado de la consulta.  
  
-   Puede mostrar hasta 32 columnas por objeto de estadísticas.  
  
-   No se puede quitar, modificar ni cambiar la definición de una columna de la tabla que se define en un predicado de estadísticas filtrado.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere que el usuario sea el propietario de la tabla o vista indexada o un miembro de uno de los roles siguientes: rol fijo de servidor **sysadmin** , rol fijo de base de datos **db_owner** o rol fijo de base de datos **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-statistics"></a>Para crear estadísticas  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir la base de datos en la que desea crear una nueva estadística.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que desea crear una nueva estadística.  
  
4.  Haga clic con el botón derecho en la carpeta **Estadísticas** y, después, seleccione **Nueva estadística...** .  
  
     Las siguientes propiedades se muestran en la página **General** del cuadro de diálogo **Nuevas estadísticas de la tabla**_nombre\_tabla_.  
  
     **Nombre de tabla**  
     Muestra el nombre de la tabla descrita por las estadísticas.  
  
     **Nombre de las estadísticas**  
     Muestra el nombre del objeto de base de datos donde se almacenan las estadísticas.  
  
     **Columnas de estadísticas**  
     Esta cuadrícula muestra las columnas descritas por este conjunto de estadísticas. Todos los valores de la cuadrícula son de solo lectura.  
  
     **Nombre**  
     Muestra el nombre de la columna descrita por las estadísticas. Puede ser una sola columna o una combinación de columnas de una sola tabla.  
  
     **Tipo de datos**  
     Indica el tipo de datos de las columnas descritas por las estadísticas.  
  
     **Tamaño**  
     Muestra el tamaño del tipo de datos de cada columna.  
  
     **Identidad**  
     Cuando se activa, indica una columna de identidad.  
  
     **Permitir valores NULL**  
     Indica si la columna acepta valores NULL.  
  
     **Add (Agregar)**  
     Agregue columnas adicionales de la tabla a la cuadrícula de estadísticas.  
  
     **Remove**  
     Quita la columna seleccionada de la cuadrícula de estadísticas.  
  
     **Subir**  
     Desplaza la columna seleccionada a una ubicación anterior de la cuadrícula de estadísticas. La ubicación en la cuadrícula puede afectar considerablemente a la utilidad de las estadísticas.  
  
     **Bajar**  
     Desplaza la columna seleccionada a una ubicación posterior de la cuadrícula de estadísticas.  
  
     **Las estadísticas de estas columnas se actualizaron por última vez:**  
     Indica la antigüedad de las estadísticas. Las estadísticas actuales tienen mayor valor. Actualice las estadísticas después de realizar grandes cambios en los datos o después de agregar datos atípicos. Las estadísticas de las tablas que tienen una distribución coherente de datos deben actualizarse con menor frecuencia.  
  
     **Actualizar estadísticas de estas columnas**  
     Comprueba la actualización de las estadísticas cuando se cierra el cuadro de diálogo.  
  
     La siguiente propiedad se muestra en la página **Filtro** del cuadro de diálogo **Nuevas estadísticas de la tabla**_nombre\_tabla_.  
  
     **Expresión de filtro**  
     Define qué filas de datos se incluyen en las estadísticas filtradas. Por ejemplo: `Production.ProductSubcategoryID IN ( 1,2,3 )`  
  
5.  En el cuadro de diálogo **Nuevas estadísticas de la tabla**_nombre\_tabla_, en la página **General** haga clic en **Agregar**.  
  
     Las propiedades siguientes se muestran en el cuadro de diálogo **Seleccionar columnas** . Esta información es de solo lectura.  
  
     **Nombre**  
     Muestra el nombre de la columna descrita por las estadísticas. Puede ser una sola columna o una combinación de columnas de una sola tabla.  
  
     **Tipo de datos**  
     Indica el tipo de datos de las columnas descritas por las estadísticas.  
  
     **Tamaño**  
     Muestra el tamaño del tipo de datos de cada columna.  
  
     **Identidad**  
     Cuando se activa, indica una columna de identidad.  
  
     **Permitir valores NULL**  
     Indica si la columna acepta valores NULL.  
  
6.  En el cuadro de diálogo **Seleccionar columnas** , active la casilla o casillas de cada columna para la que desee crear una estadística y haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Nuevas estadísticas de la tabla**_nombre\_tabla_, haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-statistics"></a>Para crear estadísticas  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- Create new statistic object called ContactMail1  
    -- on the BusinessEntityID and EmailPromotion columns in the Person.Person table.   
  
    CREATE STATISTICS ContactMail1  
        ON Person.Person (BusinessEntityID, EmailPromotion);   
    GO  
    ```  
  
4.  La estadística creada anteriormente puede mejorar los resultados de la consulta siguiente.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT LastName, FirstName  
    FROM Person.Person  
    WHERE EmailPromotion = 2  
    ORDER BY LastName, FirstName;   
    GO  
    ```  
  
 Para obtener más información, vea [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
  
