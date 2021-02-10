---
description: Exportación de un inventario de acceso (AccessToSQL)
title: Exportación de un inventario de acceso (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, exporting metadata
- exporting
- exporting Access metadata
- exporting, Access metadata
- exporting, querying exported metadata
- inventories of Access databases
- querying exported metadata
ms.assetid: 7e1941fb-3d14-4265-aff6-c77a4026d0ed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 10bff5f86bec26994c143195fcbb97aa30ba709e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044775"
---
# <a name="exporting-an-access-inventory-accesstosql"></a>Exportación de un inventario de acceso (AccessToSQL)
Si tiene varias bases de datos de Access y no está seguro de cuáles se van a migrar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede exportar un inventario de todas las bases de datos de Access de un proyecto. Después, puede revisar y consultar los metadatos de inventario para determinar qué bases de datos y objetos de las bases de datos se van a migrar. Este inventario le permite encontrar rápidamente respuestas a preguntas, como las siguientes:  
  
-   ¿Cuáles son las bases de datos más grandes?  
  
-   ¿Quién es el propietario de la mayoría de las bases de datos?  
  
-   ¿Qué bases de datos contienen las mismas tablas?  
  
-   ¿Qué bases de datos no se han modificado en los últimos seis meses?  
  
-   ¿Qué bases de datos contienen información privada?  
  
Al final de este tema se proporcionan ejemplos de consultas que se usan para responder a estas preguntas.  
  
## <a name="exported-metadata"></a>Metadatos exportados  
SSMA exporta metadatos acerca de las bases de datos, las tablas, las columnas, los índices, las claves externas, las consultas, los informes, los formularios, las macros y los módulos de Access. Los metadatos de cada una de estas categorías de elementos se exportan a una tabla independiente. Para los esquemas de estas tablas, consulte [acceso a los esquemas de inventario](access-inventory-schemas-accesstosql.md).  
  
## <a name="exporting-inventory-data"></a>Exportar datos de inventario  
Para exportar un inventario de acceso, primero debe abrir o crear un proyecto de SSMA y, a continuación, agregar la base de datos de Access que desea analizar. Después de agregar las bases de datos a un proyecto de SSMA, exporte los metadatos de esas bases de datos a una base de datos y un esquema especificados [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si es necesario, SSMA crea tablas para almacenar los metadatos. SSMA agrega los metadatos sobre las bases de datos de Access a la base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
> Una base de datos de Access se puede dividir en varios archivos: una base de datos back-end que contiene tablas y bases de datos front-end que contienen consultas, formularios, informes, macros, módulos y accesos directos. Si desea migrar una base de datos dividida a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , agregue la base de datos de front-end a SSMA.  
  
Las instrucciones siguientes describen cómo crear un proyecto, agregar bases de datos al proyecto, conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, exportar datos de inventario.  
  
**Para crear un proyecto**  
  
1.  Abra SSMA para Access.  
  
2.  En el menú **Archivo**, seleccione **Nuevo proyecto**.  
  
    Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
3.  En el cuadro **nombre** , escriba un nombre para el proyecto.  
  
4.  En el cuadro **Ubicación** , escriba o seleccione una carpeta para el proyecto.  
  
5.  En el cuadro combinado **migrar a** , seleccione la versión de destino a la que desea migrar y, a continuación, haga clic en **Aceptar**.  
  
Para obtener más información sobre la creación de proyectos, vea [crear y administrar proyectos](creating-and-managing-projects-accesstosql.md).  
  
**Para buscar y agregar bases de datos**  
  
1.  En el menú **archivo** , haga clic en **Buscar bases de datos**.  
  
2.  En el asistente buscar bases de datos, escriba la unidad, la ruta de acceso del archivo o la ruta de acceso UNC en la que desea realizar la búsqueda. O bien, haga clic en **examinar** para seleccionar la unidad o carpeta de red.  
  
3.  Haga clic en **Agregar** para agregar la ubicación al cuadro de lista.  
  
    Repita los dos pasos anteriores para agregar ubicaciones de búsqueda adicionales.  
  
4.  Opcionalmente, agregue criterios de búsqueda para refinar la lista de bases de datos que se devuelven.  
  
    > [!IMPORTANT]  
    > El cuadro **de texto todo o parte del nombre de archivo** no admite caracteres comodín.  
  
5.  Haz clic en **Examen**.  
  
    Aparece la página digitalizar. Esto muestra las bases de datos que se han encontrado y el progreso de la búsqueda. Para detener la búsqueda, haga clic en **detener**.  
  
6.  En la página seleccionar archivos, seleccione cada una de las bases de datos que desea agregar al proyecto.  
  
    Puede usar los botones **seleccionar todo** y **Borrar todo** de la parte superior de la lista para seleccionar o borrar todas las bases de datos. También puede mantener presionada la tecla CTRL para seleccionar varias filas o mantener presionada la tecla Mayús para seleccionar un intervalo de filas.  
  
7.  Haga clic en **Siguiente**.  
  
8.  En la página comprobar, haga clic en **Finalizar**.  
  
Para obtener más información sobre cómo agregar bases de datos a proyectos, vea [Agregar y quitar archivos de base de datos de Access](adding-and-removing-access-database-files-accesstosql.md).  
  
**Para conectarse a SQL Server**  
  
1.  En el menú **archivo** , seleccione **conectarse a SQL Server**.  
  
2.  En el cuadro de diálogo conexión, escriba o seleccione el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Si se está conectando a la instancia predeterminada en el equipo local, puede especificar **localhost** o un punto (**.**).  
  
    -   Si se va a conectar a la instancia predeterminada en otro equipo, escriba el nombre del equipo.  
  
    -   Si se va a conectar a una instancia con nombre, escriba el nombre del equipo, una barra diagonal inversa y el nombre de la instancia. Por ejemplo: MyServer\MyInstance.  
  
3.  En el cuadro **base** de datos, escriba el nombre de la base de datos de destino para los metadatos exportados.  
  
4.  Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurada para aceptar conexiones en un puerto no predeterminado, escriba el número de puerto que se utiliza para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las conexiones en el cuadro **Puerto del servidor** . Para la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el número de puerto predeterminado es 1433. En el caso de las instancias con nombre, SSMA intenta obtener el número de puerto del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio browser.  
  
5.  En el menú desplegable **autenticación** , seleccione el tipo de autenticación que se utilizará para la conexión. Para usar la cuenta de Windows actual, seleccione **autenticación de Windows**. Para usar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión, seleccione **SQL Server autenticación** y proporcione un nombre de usuario y una contraseña.  
  
Para obtener más información sobre cómo conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [conectarse a SQL Server &#40;AccessToSQL&#41;](../../ssma/access/connecting-to-sql-server-accesstosql.md).  
  
**Para exportar información de inventario**  
  
1.  En el explorador de metadatos de Access, expanda **Access-metabase**.  
  
2.  Active la casilla situada junto a **bases de datos**.  
  
    Para omitir las bases de datos individuales o los objetos de base de datos, expanda la carpeta **bases** de datos y, a continuación, desactive la casilla situada junto a la base de datos u objeto de base de datos.  
  
3.  Haga clic con el botón derecho en **bases de datos** y seleccione **exportar esquema**.  
  
4.  En el cuadro de diálogo **seleccionar esquema para exportar** , seleccione el esquema de destino para los metadatos exportados y, a continuación, haga clic en **Aceptar**.  
  
Cada vez que se exportan metadatos, SSMA anexa los datos al inventario. Los datos existentes en el inventario no se actualizan ni se eliminan.  
  
## <a name="querying-the-exported-metadata"></a>Consultar los metadatos exportados  
Después de exportar los metadatos acerca de las bases de datos de Access, puede consultar los metadatos. Las instrucciones siguientes describen cómo usar la ventana del editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ejecutar consultas.  
  
**Para consultar metadatos**  
  
1.  En el menú **Inicio** , seleccione **todos los programas**, **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005** o Microsoft **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008** o **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012** y, a continuación, haga clic en **SQL Server Management Studio**.  
  
2.  En el cuadro de diálogo **conectar al servidor** , Compruebe la configuración y, a continuación, haga clic en **conectar**.  
  
3.  En la barra de herramientas Management Studio, haga clic en **nueva consulta** para abrir el editor de consultas.  
  
4.  En la ventana Editor de consultas, escriba una consulta. En la sección siguiente se muestran algunos ejemplos.  
  
5.  Presione la tecla F5 para ejecutar la consulta.  
  
## <a name="query-examples"></a>Ejemplos de consultas  
Antes de ejecutar cualquiera de las siguientes consultas, debe ejecutar una consulta USE *database_name* para asegurarse de que las consultas se ejecutan en la base de datos que contiene los metadatos exportados. Por ejemplo, si exportó los metadatos a una base de datos denominada MyAccessMetadata, debe agregar lo siguiente al principio del [!INCLUDE[tsql](../../includes/tsql-md.md)] código:  
  
```  
USE MyAccessMetadata;  
GO  
```  
En los siguientes ejemplos se usa el esquema **DBO** . Si exportó los metadatos a otro esquema, asegúrese de cambiar el esquema cuando ejecute estas consultas.  
  
### <a name="what-tables-and-columns-are-in-these-databases"></a>¿Qué tablas y columnas hay en estas bases de datos?  
La siguiente consulta combina las tablas que contienen metadatos de columna, tabla y base de datos y, a continuación, devuelve los nombres de todas las bases de datos, tablas y columnas, ordenadas por nombre de columna:  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D.DatabaseId  
ORDER BY ColumnName;  
```  
  
### <a name="what-are-the-largest-databases"></a>¿Cuáles son las bases de datos más grandes?  
La consulta siguiente devuelve el nombre de la base de datos, el tamaño del archivo y el número de tablas de cada base de datos de Access, ordenadas por tamaño de archivo:  
  
```  
SELECT DatabaseName, FileSize, TablesCount  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileSize DESC;  
```  
  
### <a name="who-is-the-owner-of-most-of-the-databases"></a>¿Quién es el propietario de la mayoría de las bases de datos?  
La consulta siguiente devuelve el nombre de la base de datos y el propietario de cada base de datos de Access, ordenados por propietario.  
  
```  
SELECT DatabaseName, FileOwner  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileOwner;  
```  
  
### <a name="which-databases-contain-the-same-tables"></a>¿Qué bases de datos contienen las mismas tablas?  
La siguiente consulta utiliza una subconsulta para buscar todos los nombres de tabla que aparecen más de una vez en la lista de tablas y, a continuación, usa esta lista de tablas para obtener el nombre de la base de datos. Los resultados se devuelven como el nombre de la base de datos y, a continuación, el nombre de la tabla y se ordenan por nombre de tabla.  
  
```  
SELECT DatabaseName, TableName   
FROM dbo.SSMA_Access_InventoryTables T  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON D.DatabaseId = T.DatabaseId  
WHERE TableName IN (  
SELECT TableName   
FROM dbo.SSMA_Access_InventoryTables  
GROUP BY TableName   
HAVING count(*)>1  
)  
ORDER BY TableName;  
```  
  
### <a name="which-databases-were-not-modified-in-the-last-six-months"></a>¿Qué bases de datos no se modificaron en los últimos seis meses?  
En la consulta siguiente se obtiene la fecha actual, se obtiene el valor del mes durante seis meses y, a continuación, se devuelve una lista de las bases de datos con una fecha modificada de hace más de seis meses.  
  
```  
SELECT DatabaseName, DateModified  
FROM dbo.SSMA_Access_InventoryDatabases  
WHERE DATEDIFF(month, DateModified, GETDATE()) > 6  
ORDER BY DateModified;  
```  
  
### <a name="which-databases-contain-private-information"></a>¿Qué bases de datos contienen información privada?  
Las bases de datos de Access pueden contener información confidencial o personal. Es posible que desee trasladar estas bases de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para aprovechar las ventajas de sus características de seguridad. Si sabe que las columnas que contienen datos confidenciales tienen un nombre específico o que contienen caracteres específicos, puede usar una consulta para buscar todas las columnas que contengan esa información. Por ejemplo, puede buscar todas las columnas que incluyan la cadena "Salary".  A continuación, la consulta devuelve el nombre de la base de datos, el nombre de la tabla y el nombre de la columna.  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D. DatabaseId  
WHERE ColumnName LIKE '%salary%';  
```  
Si no conoce el nombre de la columna, puede escribir una consulta para que devuelva todas las columnas. Para ello, quite la cláusula WHERE de la consulta anterior.  
  
## <a name="see-also"></a>Consulte también  
[Preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md)  
  
