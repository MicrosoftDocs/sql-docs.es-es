---
title: Introducción con SSMA for SAP ASE (SybaseToSQL) | Microsoft Docs
description: Obtenga información sobre el proceso de instalación de SQL Server Migration Assistant (SSMA) for SAP ASE y familiarícese con la interfaz de usuario de SSMA.
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c4098516-f0fc-4690-97bb-3766dfd43156
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2ed1698af612a3bd960b7c16011dc4f95e33f61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078866"
---
# <a name="getting-started-with-ssma-for-sap-ase-sybasetosql"></a>Introducción con SSMA para SAP ASE (SybaseToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para SAP ASE le permite convertir rápidamente esquemas de base de datos SAP Adaptive Server Enterprise (ASE) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas de o Azure SQL Database, cargar los esquemas resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database y migrar datos de SAP ase a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a Azure SQL Database.  
  
En este tema se presenta el proceso de instalación y, a continuación, se le ayuda a familiarizarse con la interfaz de usuario de SSMA.  
  
## <a name="installing-and-licensing-ssma"></a>Instalación y licencias de SSMA  
Para usar SSMA, primero debe instalar el programa cliente de SSMA en un equipo que pueda tener acceso tanto a la instancia de origen de SAP ASE como a la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Para usar la migración de datos del lado servidor, debe instalar el paquete de extensiones y al menos uno de los proveedores de SAP ASE (OLE DB o ADO.NET) en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Estos componentes admiten la migración de datos y la emulación de las funciones del sistema de SAP ASE. Para obtener instrucciones de instalación, consulte [instalación de SSMA para SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md).  
  
Para iniciar SSMA, haga clic en **Inicio**, **Seleccione todos los programas**, **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant para Sybase** y, a continuación, seleccione **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant de Sybase**. La primera vez que inicie SSMA, aparecerá un cuadro de diálogo de licencia. Debe conceder una licencia de SSMA mediante un Windows Live ID antes de poder usar SSMA. Las instrucciones de licencia se incluyen con las instrucciones de instalación del tema [Installing SSMA for Sybase Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md) .  
  
## <a name="ssma-for-sap-ase-user-interface"></a>SSMA para la interfaz de usuario de SAP ASE  
Una vez instalado SSMA y con licencia, puede usar SSMA para migrar bases de datos de SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Ayuda a familiarizarse con la interfaz de usuario de SSMA antes de empezar. En el diagrama siguiente se muestra la interfaz de usuario de SSMA, incluidos los exploradores de metadatos, los metadatos, las barras de herramientas, el panel de resultados y el panel lista de errores:  
  
![SSMA para la interfaz de usuario de SAP ASE](../../ssma/sybase/media/ssmaforsybaseuserinterface.jpg "SSMA para la interfaz de usuario de SAP ASE")  
  
Para iniciar una migración, primero debe crear un nuevo proyecto. A continuación, se conecta a SAP ASE. Después de una conexión correcta, aparecerá una jerarquía de bases de datos de SAP ASE en el explorador de metadatos de Sybase. Después, puede hacer clic con el botón derecho en objetos en el explorador de metadatos de Sybase para realizar tareas como crear informes que evalúen las conversiones en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. También puede realizar estas tareas a través de las barras de herramientas y los menús.  
  
También debe conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Después de una conexión correcta, una jerarquía de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de Azure SQL Database aparecerá en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos. Después de convertir los esquemas de SAP ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas de o Azure SQL Database, seleccione los esquemas convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos y, a continuación, cargue los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
Después de cargar los esquemas convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database, puede volver al explorador de metadatos de Sybase y migrar datos de bases de datos de SAP ase a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos SQL de o de Azure.  
  
Para obtener más información sobre estas tareas y cómo realizarlas, consulte [migración de bases de datos de SAP ase a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md).  
  
En las secciones siguientes se describen las características de la interfaz de usuario de SSMA.  
  
### <a name="metadata-explorers"></a>Exploradores de metadatos  
SSMA contiene dos exploradores de metadatos para examinar y realizar acciones en SAP ASE y en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database.  
  
#### <a name="sybase-metadata-explorer"></a>Explorador de metadatos de Sybase  
Sybase Metadata Explorer muestra información sobre las bases de datos en la instancia de origen de SAP ASE.  
  
Mediante el explorador de metadatos de Sybase, puede realizar las siguientes tareas:  
  
-   Examinar las tablas de cada base de datos.  
  
-   Seleccione objetos para la conversión y, a continuación, convierta los objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database sintaxis. Para más información, consulte [conversión de objetos de base de datos de SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
-   Seleccione objetos para la migración de datos y, a continuación, migre los datos de esos objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Para más información, consulte [migración de datos de SAP ase en SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
#### <a name="sql-server-or-sql-azure-metadata-explorer"></a>SQL Server o SQL Azure explorador de metadatos  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos muestra información sobre una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database, SSMA recupera metadatos sobre esa instancia y los almacena en el archivo de proyecto.  
  
Puede usar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos para seleccionar objetos de base de datos de SAP ase convertidos y, a continuación, cargar (sincronizar) esos objetos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
Para obtener más información, vea [cargar objetos de base de datos convertidos en SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
### <a name="metadata"></a>Metadatos  
A la derecha de cada explorador de metadatos hay pestañas que describen el objeto seleccionado. Por ejemplo, si selecciona una tabla en el explorador de metadatos de Sybase, aparecen seis pestañas: **tabla**, **SQL**, **asignación de tipos**, **datos**, **propiedades** e **informes**. La pestaña **Informe** contiene información solo después de crear un informe que contenga el objeto seleccionado. Si selecciona una tabla en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure el explorador de metadatos, aparecen tres pestañas: **tabla**, **SQL** y **datos**.  
  
La mayoría de los valores de metadatos son de solo lectura. Sin embargo, puede modificar los metadatos siguientes:  
  
-   En el explorador de metadatos de Sybase, puede modificar los procedimientos y las asignaciones de tipos. Realice estos cambios antes de convertir los esquemas.  
  
-   En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos, puede modificar [!INCLUDE[tsql](../../includes/tsql-md.md)] para los procedimientos almacenados. Realice estos cambios antes de cargar los esquemas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Los cambios realizados en un explorador de metadatos se reflejan en los metadatos del proyecto, no en las bases de datos de origen o de destino.  
  
### <a name="toolbars"></a>Barras de herramientas  
SSMA tiene dos barras de herramientas: una barra de herramientas de proyecto y una barra de herramientas de migración.  
  
#### <a name="the-project-toolbar"></a>Barra de herramientas del proyecto  
La barra de herramientas del proyecto contiene botones para trabajar con proyectos, conectarse a SAP ASE y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Estos botones se asemejan a los comandos del menú **archivo** .  
  
#### <a name="the-migration-toolbar"></a>Barra de herramientas de migración  
La barra de herramientas de migración contiene los siguientes comandos:  
  
|Botón|Función|  
|----------|------------|  
|**Crear informe**|Convierte los objetos de SAP ASE seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis y, a continuación, crea un informe que muestra el éxito de la conversión.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Sybase.|  
|**Convertir esquema**|Convierte los objetos de SAP ASE seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o Azure SQL Database.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Sybase.|  
|**Migrar datos**|Migra datos de la base de datos de SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Antes de ejecutar este comando, debe convertir los esquemas de SAP ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas de o Azure SQL Database y, a continuación, cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.<br /><br />Este comando solo está disponible cuando se seleccionan objetos en el explorador de metadatos de Sybase.|  
|**Detención**|Detiene el proceso actual, como convertir objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database sintaxis.|  
  
### <a name="menus"></a>Menús  
SSMA contiene los siguientes menús:  
  
|Menú|Descripción|  
|--------|---------------|  
|**Archivo**|Contiene comandos para trabajar con proyectos, conectarse a SAP ASE y conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.|  
|**Editar**|Contiene comandos para buscar y trabajar con texto en las páginas de detalles, como copiar [!INCLUDE[tsql](../../includes/tsql-md.md)] desde el panel de detalles de SQL. También contiene la opción **administrar marcadores** , donde puede ver una lista de marcadores existentes. Puede usar los botones que se encuentran en el lado derecho del cuadro de diálogo para administrar los marcadores.|  
|**Ver**|Contiene el comando **sincronizar exploradores de metadatos** . Esto sincroniza los objetos entre el explorador de metadatos de Sybase y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure el explorador de metadatos. También contiene comandos para mostrar y ocultar los paneles de **salida** y **lista de errores** , así como los **diseños** de opciones para administrar los diseños.|  
|**Herramientas**|Contiene comandos para crear informes, exportar datos y migrar objetos y datos. También proporciona acceso a los cuadros de diálogo Configuración **global** y **configuración del proyecto** .|  
|**Evaluador**|Contiene comandos para crear casos de prueba, ver resultados de pruebas y comandos para la administración de copia de seguridad de base de datos.|  
|**Ayuda**|Proporciona acceso a la ayuda de SSMA y al cuadro **de diálogo acerca de** .|  
  
### <a name="output-pane-and-error-list-pane"></a>Panel de salida y panel de Lista de errores  
El menú **Ver** proporciona comandos para alternar la visibilidad del panel de resultados y el panel de lista de errores:  
  
-   En el panel de salida se muestran los mensajes de estado de SSMA durante la conversión de objetos, la sincronización de objetos y la migración de datos.  
  
-   En el panel de Lista de errores se muestran mensajes de error, de advertencia e informativos en una lista que se puede ordenar.  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de SAP ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Referencia de la interfaz de usuario &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
