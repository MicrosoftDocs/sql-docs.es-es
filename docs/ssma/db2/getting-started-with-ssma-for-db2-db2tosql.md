---
description: Introducción con SSMA para DB2 (DB2ToSQL)
title: Introducción con SSMA para DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 48ca32fc-1830-4d1f-add7-480ba5ad02e8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1829101a62a3f2602e32b6e4d3535292a90a140d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069520"
---
# <a name="getting-started-with-ssma-for-db2-db2tosql"></a>Introducción con SSMA para DB2 (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para DB2 permite convertir rápidamente esquemas de base de datos DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas, cargar los esquemas resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y migrar datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
En este tema se presenta el proceso de instalación y, a continuación, se le ayuda a familiarizarse con la interfaz de usuario de SSMA.  
  
## <a name="installing-ssma"></a>Instalación de SSMA  
Para usar SSMA, primero debe instalar el programa cliente de SSMA en un equipo que pueda tener acceso a la base de datos DB2 de origen y a la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Proveedores de DB2 OLEDB en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Estos componentes admiten la migración de datos y la emulación de funciones del sistema DB2. Para obtener instrucciones de instalación, consulte [instalación de SSMA para DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md).  
  
Para iniciar SSMA, haga clic en **Inicio**, **Seleccione todos los programas**, **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant para DB2** y, a continuación, haga clic en **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant para DB2**.  
  
## <a name="ssma-for-db2-user-interface"></a>SSMA para la interfaz de usuario de DB2  
Una vez instalado SSMA, puede usar SSMA para migrar bases de datos DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ayuda a familiarizarse con la interfaz de usuario de SSMA antes de empezar. En el diagrama siguiente se muestra la interfaz de usuario de SSMA, incluidos los exploradores de metadatos, los metadatos, las barras de herramientas, el panel de resultados y el panel lista de errores:  
  
![Interfaz de usuario de SSMA](../../ssma/db2/media/ssma_db2_ui.png "Interfaz de usuario de SSMA")  
  
Para iniciar una migración, primero debe crear un nuevo proyecto. A continuación, se conecta a una base de datos DB2. Después de una conexión correcta, los esquemas DB2 aparecerán en el explorador de metadatos DB2. Después, puede hacer clic con el botón derecho en objetos en el explorador de metadatos DB2 para realizar tareas como crear informes que evalúen las conversiones en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También puede realizar estas tareas mediante las barras de herramientas y los menús.  
  
También debe conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Después de una conexión correcta, aparecerá una jerarquía de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos. Después de convertir los esquemas DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas, seleccione los esquemas convertidos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos y, a continuación, sincronice los esquemas con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Después de sincronizar los esquemas convertidos con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede volver al explorador de metadatos DB2 y migrar datos de esquemas DB2 a bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Para obtener más información acerca de estas tareas y cómo realizarlas, vea [migrar bases de datos de DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
En las secciones siguientes se describen las características de la interfaz de usuario de SSMA.  
  
### <a name="metadata-explorers"></a>Exploradores de metadatos  
SSMA contiene dos exploradores de metadatos para examinar y realizar acciones en DB2 y bases de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
#### <a name="db2-metadata-explorer"></a>Explorador de metadatos DB2  
El explorador de metadatos DB2 muestra información acerca de los esquemas DB2. Mediante el explorador de metadatos DB2, puede realizar las siguientes tareas:  
  
-   Examine los objetos de cada esquema.  
  
-   Seleccione objetos para la conversión y, a continuación, convierta los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis. Para obtener más información, vea [convertir esquemas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
-   Seleccione tablas para la migración de datos y, a continuación, migre los datos de esas tablas a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [migrar bases de datos DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>SQL Server explorador de metadatos  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El explorador de metadatos muestra información sobre una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA recupera los metadatos sobre esa instancia y los almacena en el archivo de proyecto.  
  
Puede usar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos para seleccionar objetos de base de datos DB2 convertidos y, a continuación, sincronizar esos objetos con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="metadata"></a>Metadatos  
A la derecha de cada explorador de metadatos hay pestañas que describen el objeto seleccionado. Por ejemplo, si selecciona una tabla en el explorador de metadatos DB2, se mostrarán seis pestañas: **tabla**, **SQL**, **asignación de tipos, Informe**, **propiedades** y **datos**. La pestaña **Informe** contiene información solo después de crear un informe que contenga el objeto seleccionado. Si selecciona una tabla en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos, aparecerán tres pestañas: **tabla**, **SQL** y **datos**.  
  
La mayoría de los valores de metadatos son de solo lectura. Sin embargo, puede modificar los metadatos siguientes:  
  
-   En el explorador de metadatos DB2, puede modificar los procedimientos y las asignaciones de tipos. Para convertir los procedimientos modificados y las asignaciones de tipos, realice los cambios antes de convertir los esquemas.  
  
-   En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos, puede modificar [!INCLUDE[tsql](../../includes/tsql-md.md)] para los procedimientos almacenados. Para ver estos cambios en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , realice estos cambios antes de cargar los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Los cambios realizados en un explorador de metadatos se reflejan en los metadatos del proyecto, no en las bases de datos de origen o de destino.  
  
### <a name="toolbars"></a>Barras de herramientas  
SSMA tiene dos barras de herramientas: una barra de herramientas de proyecto y una barra de herramientas de migración.  
  
#### <a name="the-project-toolbar"></a>Barra de herramientas del proyecto  
La barra de herramientas del proyecto contiene botones para trabajar con proyectos, conectarse a DB2 y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Estos botones se asemejan a los comandos del menú **archivo** .  
  
#### <a name="migration-toolbar"></a>Barra de herramientas de migración  
En la tabla siguiente se muestran los comandos de la barra de herramientas de migración:  
  
|Botón|Función|  
|------|--------|  
|**Crear informe**|Convierte los objetos DB2 seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis y, a continuación, crea un informe que muestra el éxito de la conversión.<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos DB2.|  
|**Convertir esquema**|Convierte los objetos DB2 seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos.<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos DB2.|  
|**Migrar datos**|Migra datos de la base de datos DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Antes de ejecutar este comando, debe convertir los esquemas DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas y, a continuación, cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Este comando está deshabilitado a menos que se seleccionen objetos en el explorador de metadatos DB2.|  
|**Detención**|Detiene el proceso actual.|  
  
### <a name="menus"></a>Menús  
En la tabla siguiente se muestran los menús de SSMA.  
  
|Menú|Descripción|  
|----|-----------|  
|**Archivo**|Contiene comandos para trabajar con proyectos, conectarse a DB2 y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Editar**|Contiene comandos para buscar y trabajar con texto en las páginas de detalles, como copiar [!INCLUDE[tsql](../../includes/tsql-md.md)] desde el panel de detalles de SQL. También contiene la opción **administrar marcadores** , donde podrá ver una lista de marcadores existentes. Puede usar los botones que se encuentran en el lado derecho del cuadro de diálogo para administrar los marcadores.|  
|**Ver**|Contiene el comando **sincronizar exploradores de metadatos** . Que sincroniza los objetos entre el explorador de metadatos DB2 y el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos. También contiene comandos para mostrar y ocultar los paneles de **salida** y **lista de errores** y los **diseños** de opciones para administrar los diseños.|  
|**Herramientas**|Contiene comandos para crear informes y migrar objetos y datos. También proporciona acceso a los cuadros de diálogo Configuración **global** y **configuración del proyecto** .|  
|**Ayuda**|Proporciona acceso a la ayuda de SSMA y al cuadro **de diálogo acerca de** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Panel de salida y panel de Lista de errores  
El menú **Ver** proporciona comandos para alternar la visibilidad del panel de resultados y el panel de lista de errores:  
  
-   En el panel de salida se muestran los mensajes de estado de SSMA durante la conversión de objetos, la sincronización de objetos y la migración de datos.  
  
-   En el panel de Lista de errores se muestran mensajes de error, de advertencia e informativos en una lista que se va a ordenar.  
  
## <a name="see-also"></a>Consulte también  
[Migración de datos de DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
[Referencia de la interfaz de usuario &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
