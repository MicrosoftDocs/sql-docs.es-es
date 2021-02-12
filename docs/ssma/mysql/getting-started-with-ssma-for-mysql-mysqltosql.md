---
title: Introducción con SSMA para MySQL (MySQLToSQL) | Microsoft Docs
description: Obtenga información sobre el proceso de instalación de SQL Server Migration Assistant (SSMA) para MySQL y familiarícese con la interfaz de usuario de SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Getting started, MySQL metadata explorer
- Getting started, SQL Server or SQL Azure metadata explorer
- Getting started,Installing and licensing
ms.assetid: 8ebfa061-be6f-4a07-923f-8dc832a82f70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9847b7a709fe64f3fcb7fca92ce1f8a4003843e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070260"
---
# <a name="getting-started-with-ssma-for-mysql-mysqltosql"></a>Introducción a SSMA para MySQL (MySQLToSQL)
SQL Server Migration Assistant (SSMA) para MySQL permite convertir rápidamente esquemas de base de datos MySQL en esquemas de SQL Server o Azure SQL Database, cargar los esquemas resultantes en SQL Server o Azure SQL Database y migrar datos de MySQL a SQL Server o Azure SQL Database.  
  
En este tema se presenta el proceso de instalación y, a continuación, se le ayuda a familiarizarse con la interfaz de usuario de SSMA.  
  
## <a name="installing-ssma"></a>Instalación de SSMA  
Para usar SSMA, primero debe instalar el programa cliente de SSMA en un equipo que pueda tener acceso a la base de datos MySQL de origen y a la instancia de destino de SQL Server o Azure SQL Database. A continuación, instale los proveedores de MySQL (controlador de ODBC 5,1 de MySQL (de confianza) en el equipo que ejecuta el programa cliente de SSMA. Para obtener instrucciones de instalación, consulte [instalación de SSMA para MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
  
Para iniciar SSMA, haga clic en **Inicio**, seleccione **todos los programas**, **SQL Server Migration Assistant para MySQL** y, a continuación, haga clic en **SQL Server Migration Assistant para MySQL**.  
  
## <a name="ssma-for-mysql-user-interface"></a>SSMA para la interfaz de usuario de MySQL  
Una vez instalado SSMA y con licencia, puede usar SSMA para migrar bases de datos de MySQL a SQL Server o Azure SQL Database. Ayuda a familiarizarse con la interfaz de usuario de SSMA antes de empezar. En el diagrama siguiente se muestra la interfaz de usuario de SSMA, incluidos los exploradores de metadatos, los metadatos, las barras de herramientas, el panel de resultados y el panel lista de errores:  
  
![SSMA para la interfaz de usuario de gráfica de MySQL](../../ssma/mysql/media/ssmaformysqlgui.gif "SSMA para la interfaz de usuario de gráfica de MySQL")  
  
Para iniciar una migración, debe:  
  
1.  Cree un nuevo proyecto.  
  
2.  Conectarse a una base de datos MySQL.  
  
3.  Después de una conexión correcta, los esquemas de MySQL aparecerán en el explorador de metadatos de MySQL. Haga clic con el botón derecho en objetos en el explorador de metadatos de MySQL para realizar tareas como crear informes que evalúen las conversiones a SQL Server/Azure SQL Database.  
  
También puede realizar estas tareas mediante las barras de herramientas y los menús.  
  
También debe conectarse a una instancia de SQL Server. Después de una conexión correcta, aparecerá una jerarquía de SQL Server bases de datos en SQL Server explorador de metadatos. Después de convertir los esquemas de MySQL en esquemas de SQL Server, seleccione esos esquemas convertidos en SQL Server explorador de metadatos y, a continuación, sincronice los esquemas con SQL Server.  
  
Debe conectarse a Azure SQL Database si ha seleccionado Azure SQL Database en el cuadro de diálogo migrar a la lista desplegable en nuevo proyecto. Después de una conexión correcta, aparecerá una jerarquía de Azure SQL Database bases de datos en Azure SQL Database explorador de metadatos. Después de convertir los esquemas de MySQL en esquemas de Azure SQL Database, seleccione esos esquemas convertidos en Azure SQL Database explorador de metadatos y, a continuación, sincronice los esquemas con Azure SQL Database.  
  
Después de sincronizar los esquemas convertidos con SQL Server o Azure SQL Database, puede volver al explorador de metadatos de MySQL y migrar los datos de los esquemas de MySQL a las bases de datos de SQL Server o Azure SQL Database.  
  
Para obtener más información acerca de estas tareas y cómo realizarlas, vea [migrar bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md).  
  
En las secciones siguientes se describen las características de la interfaz de usuario de SSMA.  
  
### <a name="metadata-explorers"></a>Exploradores de metadatos  
SSMA contiene dos exploradores de metadatos para examinar y realizar acciones en las bases de datos MySQL y SQL Server.  
  
### <a name="mysql-metadata-explorer"></a>Explorador de metadatos de MySQL  
El explorador de metadatos de MySQL muestra información sobre los esquemas de MySQL. Mediante el explorador de metadatos de MySQL, puede realizar las siguientes tareas:  
  
-   Examine los objetos de cada esquema.  
  
-   Seleccione objetos para la conversión y, a continuación, convierta los objetos a SQL Server sintaxis. Para obtener más información, vea [convertir las bases de datos MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
-   Seleccione tablas para la migración de datos y, a continuación, migre los datos de esas tablas a SQL Server. Para obtener más información, vea [migrar datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server o Azure SQL Database explorador de metadatos  
SQL Server o Azure SQL Database el explorador de metadatos muestra información sobre una instancia de SQL Server o Azure SQL Database. Cuando se conecta a una instancia de SQL Server o Azure SQL Database, SSMA recupera los metadatos sobre esa instancia y los almacena en el archivo de proyecto.  
  
Puede usar este explorador de metadatos para seleccionar objetos de base de datos MySQL convertidos y, a continuación, sincronizar esos objetos con la instancia de SQL Server o Azure SQL Database.  
  
Para obtener más información, consulte [Synchronization (MySQL to SQL Server/Azure SQL Database)](./loading-converted-database-objects-into-sql-server-mysqltosql.md) .  
  
### <a name="metadata"></a>Metadatos  
A la derecha de cada explorador de metadatos hay pestañas que describen el objeto seleccionado. Por ejemplo, si selecciona una tabla en el explorador de metadatos de MySQL, se mostrarán nueve pestañas: **tabla**, **SQL**, **asignación de tipos**, **datos**, **configuración**, asignación de **juego de caracteres**, **modos SQL**, **propiedades** e **Informe**. La pestaña **Informe** contiene información solo después de crear un informe que contenga el objeto seleccionado. Si selecciona una tabla en SQL Server explorador de metadatos, aparecerán tres pestañas: **tabla**, **SQL** y **datos**.  
  
La mayoría de los valores de metadatos son de solo lectura. Sin embargo, puede modificar los metadatos siguientes:  
  
-   En el explorador de metadatos de MySQL, puede modificar las asignaciones de tipos, la asignación de juegos de caracteres y los modos SQL. Para convertir las asignaciones de tipos alteradas o los modos SQL o la asignación de juegos de caracteres, realice los cambios antes de convertir los esquemas.  
  
-   En SQL Server explorador de metadatos, puede modificar las propiedades de la tabla y el índice en la pestaña tabla. Para ver estos cambios en SQL Server, realice estos cambios antes de cargar los esquemas en SQL Server.  
  
Los cambios realizados en un explorador de metadatos se reflejan en los metadatos del proyecto, no en las bases de datos de origen o de destino.  
  
### <a name="toolbars"></a>Barras de herramientas  
SSMA tiene dos barras de herramientas: una barra de herramientas de proyecto y una barra de herramientas de migración.  
  
### <a name="the-project-toolbar"></a>Barra de herramientas del proyecto  
La barra de herramientas del proyecto contiene botones para trabajar con proyectos, conectarse a MySQL y conectarse a SQL Server o Azure SQL Database. Estos botones se asemejan a los comandos del menú **archivo** .  
  
### <a name="migration-toolbar"></a>Barra de herramientas de migración  
En la tabla siguiente se muestran los comandos de la barra de herramientas de migración:  
  
|||  
|-|-|  
|**Button**|**Function**|  
|**Crear informe**|Convierte los objetos de MySQL seleccionados en SQL Server o Azure SQL Database objetos y, a continuación, crea un informe que muestra el éxito de la conversión.<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos de MySQL.|  
|**Convertir esquema**|Convierte los objetos de MySQL seleccionados en objetos SQL Server o Azure SQL Database.<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos de MySQL.|  
|**Migrar datos**|Migra datos de la base de datos MySQL a SQL Server o Azure SQL Database. Antes de ejecutar este comando, debe convertir los esquemas de MySQL en SQL Server o Azure SQL Database esquemas y, a continuación, cargar los objetos en SQL Server o Azure SQL Database.<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos de MySQL.|  
|**Detención**|Detiene el proceso actual.|  
  
### <a name="menus"></a>Menús  
En la tabla siguiente se muestran los menús de SSMA.  
  
|||  
|-|-|  
|**Menú**|**Descripción**|  
|**Archivo**|Contiene comandos para trabajar con proyectos, conectarse a MySQL y conectarse a SQL Server o Azure SQL Database.|  
|**Editar**|Contiene comandos para buscar y trabajar con texto en las páginas de detalles. Para abrir el cuadro de diálogo **administrar marcadores** , en el menú Edición, haga clic en administrar marcadores. En el cuadro de diálogo verá una lista de marcadores existentes. Puede usar los botones que se encuentran en el lado derecho del cuadro de diálogo para administrar los marcadores.|  
|**Ver**|Contiene el comando **sincronizar exploradores de metadatos** . Que sincroniza los objetos entre el explorador de metadatos de MySQL y SQL Server o Azure SQL Database explorador de metadatos. También contiene comandos para mostrar y ocultar los paneles de **salida** y **lista de errores** y los **diseños** de opciones que se van a administrar con los diseños.|  
|**Herramientas**|Contiene comandos para crear informes, convertir esquemas, actualizar desde la base de datos, migrar objetos y datos y guardar como script. También proporciona acceso a los cuadros de diálogo **configuración global, configuración de proyecto predeterminada** y **configuración del proyecto** .|  
|**Ayuda**|Proporciona acceso a la ayuda de SSMA y al cuadro **de diálogo acerca de** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Panel de salida y panel de Lista de errores  
El menú **Ver** proporciona comandos para alternar la visibilidad del panel de resultados y el panel de lista de errores:  
  
-   En el panel de salida se muestran los mensajes de estado de SSMA durante la conversión de objetos, la sincronización de objetos y la migración de datos.  
  
-   En el panel de Lista de errores se muestran mensajes de error, de advertencia e informativos en una lista que se va a ordenar.  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
[Migración de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
