---
description: Proyectos de SSMA (MySQLToSQL)
title: Trabajar con proyectos de SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Working with SSMA projects, create new project
- Working with SSMA projects, customize settings
- Working with SSMA projects, Open project
- Working with SSMA projects, Save project
ms.assetid: 9e4394e9-f177-41d9-839e-5d53a9c9b840
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92fb1bec694ca29ffe0591eb03012e1ed2ab3f2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064380"
---
# <a name="working-with-ssma-projects-mysqltosql"></a>Proyectos de SSMA (MySQLToSQL)
Para migrar las bases de datos de MySQL a SQL Server o SQL Azure, primero debe crear un proyecto de SSMA. El proyecto es un archivo que contiene la siguiente información:  
  
-   Metadatos sobre las bases de datos de MySQL que desea migrar a SQL Server o SQL Azure.  
  
-   Metadatos sobre la instancia de destino de SQL Server o SQL Azure que recibirán los objetos y los datos migrados.  
  
-   SQL Server o SQL Azure información de conexión.  
  
-   Configuración del proyecto.  
  
Al abrir un proyecto, se desconecta de MySQL y SQL Server o SQL Azure. Esto le permite trabajar sin conexión. Para obtener más información sobre cómo volver a conectarse a SQL Server, consulte [conectarse a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="reviewing-default-project-settings"></a>Revisar la configuración predeterminada del proyecto  
SSMA contiene varias opciones para convertir y cargar bases de datos, migrar datos y sincronizar SSMA con MySQL y SQL Server o SQL Azure. La configuración predeterminada es adecuada para muchos usuarios. Sin embargo, antes de crear un nuevo proyecto de SSMA, debe revisar la configuración. Si es necesario, puede cambiar la configuración predeterminada que se usará para todos los proyectos nuevos.  
  
##### <a name="to-review-default-project-settings"></a>Para revisar la configuración predeterminada del proyecto  
  
1.  Seleccione **configuración predeterminada del proyecto** en el menú **herramientas** .  
  
2.  Seleccione el tipo de proyecto en el menú desplegable de la **versión de destino** de la migración para el que se va a ver o cambiar la configuración y, a continuación, haga clic en la pestaña **General** .  
  
3.  En el panel izquierdo, haga clic en **conversión**.  
  
4.  En el panel derecho, revise y cambie la configuración según sea necesario. Para obtener más información sobre esta configuración, vea [configuración del proyecto &#40;conversión&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md) .  
  
5.  Repita los pasos 1-3 para las páginas de asignación de migración, sincronización, SQL Azure, GUI y tipo.  
  
-   Para obtener información sobre la configuración de la migración, vea [configuración del proyecto &#40;migración&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md).  
  
-   Para obtener información sobre la configuración de la sincronización en SQL Server, vea [configuración del proyecto &#40;sincronización&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md).  
  
-   Para obtener información sobre la configuración de la GUI, vea [configuración del proyecto (GUI) (SSMA Common)](../sybase/project-settings-gui-sybasetosql.md).  
  
-   Para obtener información sobre la configuración de asignación de tipos de datos, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
-   Para obtener información sobre la configuración de SQL Azure, vea [configuración del proyecto &#40;Azure SQL Database&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md).  
  
> [!NOTE]  
> La configuración de SQL Azure solo se mostrará cuando seleccione **migración a SQL Azure** al crear un proyecto.  
  
## <a name="creating-new-projects"></a>Crear nuevos proyectos  
Para migrar datos de bases de datos MySQL a SQL Server o SQL Azure, debe crear un proyecto.  
  
##### <a name="to-create-a-new-project"></a>Para crear un nuevo proyecto  
  
1.  Seleccione **nuevo proyecto** en el menú **archivo** . Aparecerá el cuadro de diálogo **Nuevo proyecto** . En el menú **Archivo**, seleccione **Nuevo proyecto**. Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
2.  En el cuadro **nombre** , escriba un nombre para el proyecto.  
  
3.  En el cuadro **Ubicación** , escriba o seleccione una carpeta para el proyecto.  
  
4.  En el menú desplegable migración, seleccione la versión de destino **que se** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa para la migración. Las opciones disponibles son:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   Azure SQL Database  
  
Y, a continuación, haga clic en **Aceptar** .  
  
SSMA crea el archivo de proyecto.  
  
## <a name="customizing-project-settings"></a>Personalización de la configuración del proyecto  
Además de definir la configuración predeterminada del proyecto que se aplica a todos los proyectos de SSMA nuevos, también puede personalizar la configuración de cada proyecto. Para obtener más información, vea [establecer opciones de proyecto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).  
  
Al personalizar las asignaciones de tipos de datos entre las bases de datos de origen y de destino, puede definir asignaciones en el nivel de proyecto, base de datos u objeto. Para obtener más información, consulte [asignación de tipos de datos de MySQL y SQL Server &#40;&#41;MySQLToSQL ](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).  
  
## <a name="saving-projects"></a>Guardar proyectos  
La característica guardar proyectos permite al usuario guardar la configuración del proyecto y, opcionalmente, los metadatos de la base de datos en el archivo del proyecto SSMA.  
  
##### <a name="to-save-a-project"></a>Para guardar un proyecto  
  
-   En el menú **archivo** , seleccione **Guardar** proyecto.  
  
Si las bases de datos del proyecto han cambiado o no se han convertido, SSMA le pedirá que cargue y guarde los metadatos. Cargar y guardar metadatos le permite trabajar sin conexión. También le permite enviar un archivo de proyecto completo a otras personas, como personal de soporte técnico. Si se le pide que guarde los metadatos, haga lo siguiente:  
  
1.  Para cada base de datos que muestra el estado **falta de metadatos**, active la casilla situada junto al nombre de la base de datos. El almacenamiento de metadatos puede tardar varios minutos. Si no desea guardar los metadatos en este momento, no active las casillas de verificación.  
  
2.  Haga clic en **Save**(Guardar).  
  
SSMA analizará los esquemas de MySQL y guardará los metadatos en el archivo de proyecto.  
  
## <a name="opening-projects"></a>Abrir proyectos  
Al abrir un proyecto, se desconecta de MySQL y de SQL Server o SQL Azure. Esto le permite trabajar sin conexión. Para actualizar los metadatos, cargue los objetos de base de datos en SQL Server o SQL Azure. Para migrar datos, debe volver a conectarse a SQL Server o SQL Azure.  
  
##### <a name="to-open-a-project"></a>Para abrir un proyecto  
  
1.  Realice uno de los siguientes procedimientos:  
  
    1.  En el menú **archivo** , seleccione **proyectos recientes**.  
  
    2.  Seleccione el proyecto que desea abrir.  
  
    3.  En el menú **archivo** , seleccione **Abrir proyecto**, busque el archivo de proyecto. m2ssproj, seleccione el archivo y, a continuación, haga clic en **abrir**.  
  
2.  Para volver a conectarse a MySQL, en el menú **archivo** , seleccione **volver a conectar a MySQL**.  
  
3.  Para volver a conectarse a SQL Server, en el menú **archivo** , seleccione **volver a conectar a SQL Server**.  
  
4.  Para volver a conectarse a SQL Azure, en el menú **archivo** , seleccione **volver a conectar a SQL Azure.**  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [conectarse a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Conexión a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
[Conexión a Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
