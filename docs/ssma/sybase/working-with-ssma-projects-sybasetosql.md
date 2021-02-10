---
description: Proyectos de SSMA (SybaseToSQL)
title: Trabajar con proyectos de SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 11091d95-c488-48c3-891a-743cac94ac93
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b0276efc988eca1674410c31a2f83379ee1e875b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064997"
---
# <a name="working-with-ssma-projects-sybasetosql"></a>Proyectos de SSMA (SybaseToSQL)
Para migrar bases de datos de Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, primero debe crear un proyecto de SSMA. El proyecto es un archivo que contiene metadatos sobre las bases de datos de ASE a las que desea migrar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, metadatos sobre la instancia de destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure que recibirán los objetos y los datos migrados, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure información de conexión y configuración del proyecto.  
  
Al abrir un proyecto, se desconecta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Esto le permite trabajar sin conexión. Puede volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Para obtener más información, consulte [conexión a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  /  [conexión a Azure SQL Database &#40;&#41;SybaseToSQL ](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Revisar la configuración predeterminada del proyecto  
SSMA contiene varias opciones para convertir y cargar objetos de base de datos, migrar datos y sincronizar SSMA con ASE y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] or SQL Azure. La configuración predeterminada para estas opciones es adecuada para muchos usuarios. Sin embargo, antes de crear un nuevo proyecto de SSMA, debe revisar las opciones y, si lo desea, cambiar los valores predeterminados que se usarán para todos los proyectos nuevos.  
  
**Para revisar la configuración predeterminada del proyecto**  
  
1.  En el menú **herramientas** , seleccione **configuración predeterminada del proyecto**.  
  
2.  Seleccione el tipo de proyecto en el menú desplegable de la **versión de destino** de la migración para el que se deben ver o cambiar las opciones de configuración y luego haga clic en la pestaña **General** .  
  
3.  En el panel izquierdo, haga clic en **conversión**.  
  
4.  En el panel derecho, revise las opciones y cambie las opciones según sea necesario. Para obtener más información sobre estas opciones, vea [configuración del proyecto &#40;conversión&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md).  
  
5.  Repita los pasos 1-3 para las páginas de migración, SQL Azure, carga de objetos, GUI y asignación de tipos.  
  
    -   Para obtener información sobre las opciones de migración, vea [configuración del proyecto &#40;migración&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md).  
  
    -   Para obtener información sobre las opciones para cargar objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [configuración del proyecto &#40;sincronización&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md).  
  
    -   Para obtener más información sobre las opciones de la GUI, vea [configuración del proyecto &#40;gui&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md).  
  
    -   Para obtener más información sobre la configuración de asignación de tipos de datos, haga clic en [configuración del proyecto &#40;asignación de tipos&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md).  
  
    -   Para obtener más información sobre las opciones de SQL Azure, vea [configuración del proyecto &#40;Azure SQL Database &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md).  
  
    > [!NOTE]  
    > La configuración de SQL Azure solo se mostrará cuando seleccione **migración a SQL Azure** al crear un proyecto.  
  
## <a name="creating-new-projects"></a>Crear nuevos proyectos  
Para migrar datos de bases de datos de ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, primero debe crear un proyecto de.  
  
**Para crear un proyecto**  
  
1.  En el menú **Archivo**, seleccione **Nuevo proyecto**.  
  
    Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
2.  En el cuadro **nombre** , escriba un nombre para el proyecto.  
  
3.  En el cuadro **Ubicación** , escriba o seleccione una carpeta para el proyecto.  
  
4.  En el menú desplegable migración, seleccione la versión de destino **que se** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa para la migración. Las opciones disponibles son:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL Database  
  
Y, a continuación, haga clic en **Aceptar**.  
  
## <a name="customizing-project-settings"></a>Personalización de la configuración del proyecto  
Además de definir la configuración predeterminada del proyecto que se aplica a todos los proyectos de SSMA nuevos, puede personalizar la configuración de cada proyecto. Para obtener más información, vea [establecer opciones de proyecto &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).  
  
Al personalizar las asignaciones de tipos de datos entre las bases de datos de origen y de destino, puede definir asignaciones en el nivel de proyecto, base de datos u objeto. Para obtener más información acerca de la asignación de tipos, consulte [asignación de tipos de datos de Sybase ase y SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
## <a name="saving-projects"></a>Guardar proyectos  
Cuando se guarda un proyecto, SSMA conserva la configuración del proyecto y, opcionalmente, los metadatos de la base de datos en el archivo del proyecto.  
  
**Para guardar un proyecto**  
  
-   En el menú **archivo** , seleccione **Guardar proyecto**.  
  
    Si las bases de datos del proyecto han cambiado o no se han convertido, SSMA le pedirá que guarde los metadatos en el proyecto. Guardar metadatos le permitirá trabajar sin conexión y enviar un archivo de proyecto completo a otras personas, incluido el personal de soporte técnico. Si se le pide que guarde los metadatos, haga lo siguiente:  
  
    1.  Para cada base de datos que muestra el estado **falta de metadatos**, active la casilla situada junto al nombre de la base de datos.  
  
        El almacenamiento de metadatos puede tardar varios minutos. Si no desea guardar los metadatos en este momento, no active las casillas de verificación.  
  
    2.  Haga clic en el botón **Save** (Guardar).  
  
        SSMA analizará los esquemas de Sybase ASE y guardará los metadatos en el archivo de proyecto.  
  
## <a name="opening-projects"></a>Abrir proyectos  
Al abrir un proyecto, se desconecta de ASE y de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Esto le permite trabajar sin conexión. Para actualizar los metadatos, cargue los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Para migrar datos, debe volver a conectarse a ASE y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
**Para abrir un proyecto**  
  
1.  Realice uno de los siguientes procedimientos:  
  
    -   En el menú **archivo** , seleccione **proyectos recientes** y, a continuación, seleccione el proyecto que desea abrir.  
  
    -   En el menú **archivo** , seleccione **Abrir proyecto**, busque el archivo de proyecto. s2ssproj, seleccione el archivo y, a continuación, haga clic en **abrir**.  
  
2.  Para volver a conectar con ASE, en el menú **archivo** , seleccione **volver a conectar a Sybase**.  
  
3.  Para volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, en el menú **archivo** , seleccione **volver a conectar con SQL Server**  /  **volver a conectar a SQL Azure**.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [conectarse a Sybase ase](connecting-to-sybase-ase-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de Sybase ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Conexión a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)  
[Conexión a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  
[Conexión a Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)  
  
