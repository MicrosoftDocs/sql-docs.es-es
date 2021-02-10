---
title: Introducción a SQL Server Migration Assistant para el acceso | Microsoft Docs
description: Introducción al uso de SSMA para convertir objetos de base de datos de Access en SQL Server o Azure SQL Database objetos, cargar los objetos resultantes y migrar datos.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- error list pane
- getting started
- menus
- metadata explorers
- output pane
- toolbars
- user interface
- user interface overview
ms.assetid: 462a731f-08f1-44e1-9eeb-4deac6d2f6c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e05bc860ec1cc9f3703a1d1c17441c4e1ad3de15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044665"
---
# <a name="getting-started-with-sql-server-migration-assistant-for-access-accesstosql"></a>Introducción a SQL Server Migration Assistant para Access (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para Access permite convertir rápidamente objetos de base de datos de Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos de o Azure SQL Database, cargar los objetos resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database y migrar datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a Azure SQL Database. Si es necesario, también puede vincular tablas de acceso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database tablas para poder seguir usando las aplicaciones front-end de Access existentes con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
En este tema se presenta el proceso de instalación y le ayuda a familiarizarse con la interfaz de usuario de SSMA.  
  
## <a name="installing-ssma"></a>Instalación de SSMA  
Para usar SSMA, primero debe instalar el programa cliente de SSMA en un equipo que pueda tener acceso a las bases de datos que desea migrar y a la instancia de destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Para obtener instrucciones de instalación, consulte [installing SQL Server Migration Assistant for Access &#40;AccessToSQL&#41;](../../ssma/access/installing-sql-server-migration-assistant-for-access-accesstosql.md).  
  
Para iniciar SSMA, haga clic en **Inicio**, seleccione **todos los programas**, **SQL Server Migration Assistant para Access** y, a continuación, seleccione **SQL Server Migration Assistant para el acceso**.  
  
## <a name="using-ssma"></a>Usar SSMA  
Después de instalar SSMA, le ayudará a familiarizarse con la interfaz de usuario de SSMA antes de usar la herramienta para migrar bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. En el diagrama siguiente se muestra la interfaz de usuario de SSMA, incluidos los exploradores de metadatos, los metadatos, las barras de herramientas, el panel de salida y el panel lista de errores:  
  
![SSMA para la interfaz de usuario de gráfica de Access](../../ssma/access/media/ssmaforaccessgui.gif "SSMA para la interfaz de usuario de gráfica de Access")  
  
Para iniciar una migración, cree un nuevo proyecto y, a continuación, agregue bases de datos de Access al explorador de metadatos. Después, puede hacer clic con el botón derecho en objetos en el explorador de metadatos de acceso para realizar tareas como las siguientes:
- Exportar un inventario de objetos de base de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.
- Crear informes que evalúen las conversiones en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.
- Convertir esquemas de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database esquemas.

También puede realizar estas tareas mediante las barras de herramientas y los menús.  
  
También debe conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Después de una conexión correcta, aparece una jerarquía de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos. Después de convertir los esquemas de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los esquemas, puede seleccionar los esquemas convertidos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos y, a continuación, cargar los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Si ha seleccionado Azure SQL Database en el cuadro de diálogo migrar a la lista desplegable de nuevo proyecto, debe conectarse a Azure SQL Database. Después de una conexión correcta, se muestra una jerarquía de Azure SQL Database bases de datos en Azure SQL Database explorador de metadatos. Después de convertir los esquemas de acceso a Azure SQL Database esquemas, puede seleccionar esos esquemas convertidos en Azure SQL Database explorador de metadatos y, a continuación, cargar los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Después de cargar los esquemas convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database, puede volver a tener acceso al explorador de metadatos y migrar los datos de las bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las bases de datos de o Azure SQL Database. Si es necesario, también puede vincular tablas de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas de o Azure SQL Database.  
  
Para obtener más información acerca de estas tareas y cómo realizarlas, vea los temas siguientes:  
  
-   [Preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md)  
  
-   [Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
-   [Vincular aplicaciones de Access a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)  
  
En las secciones siguientes se describen las características de la interfaz de usuario de SSMA.  
  
### <a name="metadata-explorers"></a>Exploradores de metadatos  
SSMA contiene dos exploradores de metadatos que puede usar para examinar y realizar acciones en las bases de datos de Access y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
#### <a name="access-metadata-explorer"></a>Acceder al Explorador de metadatos  
El explorador de metadatos de acceso muestra información sobre las bases de datos de Access que se han agregado al proyecto. Al agregar una base de datos de Access, SSMA recupera metadatos acerca de esa base de datos, que son los metadatos que están disponibles en el explorador de metadatos de Access.  
  
Puede usar el explorador de metadatos de Access para realizar las siguientes tareas:  
  
-   Examinar las tablas en cada base de datos de Access.  
  
-   Seleccionar objetos para la conversión y convertir los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis. Para obtener más información, vea [convertir objetos de base de datos de Access](converting-access-database-objects-accesstosql.md).  
  
-   Seleccione objetos para la migración de datos y migre los datos de dichos objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [migrar datos de Access a SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
-   Vincular y desvincular el acceso y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las tablas.  
  
#### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server o Azure SQL Database explorador de metadatos  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database explorador de metadatos muestra información sobre una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database, SSMA recupera metadatos sobre esa instancia y los almacena en el archivo de proyecto.  
  
Puede usar el explorador de metadatos SQL Server o Azure SQL Database para seleccionar objetos de base de datos de Access convertidos y cargar (sincronizar) esos objetos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
Para obtener más información, vea [cargar objetos de base de datos convertidos en SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md).  
  
### <a name="metadata"></a>Metadatos  
A la derecha de cada explorador de metadatos hay pestañas que describen el objeto seleccionado. Por ejemplo, si selecciona una tabla en el explorador de metadatos de Access, aparecen cuatro pestañas: **tabla**, **asignación de tipos**, **propiedades** y **datos**. Si selecciona una tabla en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos, aparecen tres pestañas: **tabla**, **SQL** y **datos**.  
  
La mayoría de los valores de metadatos son de solo lectura. Sin embargo, puede modificar los metadatos siguientes:  
  
-   En el explorador de metadatos de Access, puede modificar las asignaciones de tipos. Asegúrese de realizar estos cambios antes de crear informes o convertir esquemas.  
  
-   En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos, puede modificar las propiedades de la tabla y el índice en la pestaña **tabla** . Realice estos cambios antes de cargar los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [convertir objetos de base de datos de Access](converting-access-database-objects-accesstosql.md).  
  
### <a name="toolbars"></a>Barras de herramientas  
SSMA tiene dos barras de herramientas: una barra de herramientas de proyecto y una barra de herramientas de migración.  
  
#### <a name="the-project-toolbar"></a>Barra de herramientas del proyecto  
La barra de herramientas del proyecto contiene botones para trabajar con proyectos, agregar archivos de base de datos de Access y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Estos botones se asemejan a los comandos del menú **archivo** .  
  
#### <a name="the-migration-toolbar"></a>Barra de herramientas de migración  
La barra de herramientas de migración contiene los siguientes comandos:  
  
|Botón|Función|  
|----------|------------|  
|**Convertir, cargar y migrar**|Convierte las bases de Azure SQL Database datos de Access, carga los objetos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database y los migra en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un solo paso.|  
|**Crear informe**|Convierte el esquema de acceso seleccionado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database sintaxis y, a continuación, crea un informe que muestra el éxito de la conversión.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Access.|  
|**Convertir esquema**|Convierte el esquema de acceso seleccionado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database esquemas.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Access.|  
|**Migrar datos**|Migra datos de la base de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Antes de ejecutar este comando, debe convertir los esquemas de acceso en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database esquemas y, a continuación, cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Access.|  
|**Detención**|Detiene el proceso actual, como convertir objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database sintaxis.|  
  
### <a name="menus"></a>Menús  
SSMA contiene los siguientes menús:  
  
|Menú|Descripción|  
|--------|---------------|  
|**Archivo**|Contiene comandos para el Asistente para migración, trabajar con proyectos, agregar y quitar archivos de base de datos de Access y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.|  
|**Editar**|Contiene comandos para buscar y trabajar con texto en las páginas de detalles, como copiar [!INCLUDE[tsql](../../includes/tsql-md.md)] desde el panel de detalles de SQL. Para abrir el cuadro de diálogo **administrar marcadores** , en el menú Edición, haga clic en administrar marcadores. En el cuadro de diálogo, verá una lista de marcadores existentes. Puede usar los botones que se encuentran en el lado derecho del cuadro de diálogo para administrar los marcadores.|  
|**Ver**|Contiene el comando **sincronizar exploradores de metadatos** . Esto sincroniza los objetos entre el explorador de metadatos de Access y el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos de Azure SQL Database. También contiene comandos para mostrar y ocultar los paneles de **salida** y **lista de errores** y los **diseños** de opciones que se van a administrar con los diseños.|  
|**Herramientas**|Contiene comandos para crear informes, exportar datos, migrar objetos y datos, vincular tablas y proporciona acceso a los cuadros de diálogo Configuración global y proyecto.|  
|**Ayuda**|Proporciona acceso a la ayuda de SSMA y al cuadro **de diálogo acerca de** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Panel de salida y panel de Lista de errores  
El menú **Ver** proporciona comandos para alternar la visibilidad del panel de resultados y el panel de lista de errores:  
  
-   En el panel de salida se muestran los mensajes de estado de SSMA durante la conversión de objetos, la sincronización de objetos y la migración de datos.  
  
-   En el panel de Lista de errores se muestran mensajes de error, de advertencia e informativos en una lista que se puede ordenar.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
