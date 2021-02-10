---
description: Trabajar con proyectos de SSMA (DB2ToSQL)
title: Trabajar con proyectos de SSMA (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 07abef8a-28e8-4a66-927c-c9a5b8c938ef
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 67c772195ea4b2a2ab29c08219f77d04a5d6833c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069320"
---
# <a name="working-with-ssma-projects-db2tosql"></a>Trabajar con proyectos de SSMA (DB2ToSQL)
Para migrar bases de datos DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , primero debe crear un proyecto SSMA. El proyecto es un archivo que contiene la siguiente información:  
  
-   Metadatos acerca de las bases de datos DB2 a las que desea migrar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Metadatos sobre la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que recibirá los objetos y los datos migrados.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] información de conexión.  
  
-   Configuración del proyecto.  
  
Al abrir un proyecto, se desconecta de DB2 y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esto le permite trabajar sin conexión. Para obtener información sobre cómo volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [conectarse a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Revisar la configuración predeterminada del proyecto  
SSMA contiene varios valores para convertir y cargar objetos de base de datos, migrar datos y sincronizar SSMA con DB2 y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La configuración predeterminada es adecuada para muchos usuarios. Sin embargo, antes de crear un nuevo proyecto de SSMA, debe revisar la configuración. Si lo desea, puede cambiar la configuración predeterminada que se usará para todos los proyectos nuevos.  
  
**Para revisar la configuración predeterminada del proyecto**  
  
1.  En el menú **herramientas** , haga clic en **configuración predeterminada del proyecto**.  
  
2.  Seleccione el tipo de proyecto en el menú desplegable de la **versión de destino** de la migración para el que se deben ver o cambiar las opciones de configuración y luego haga clic en la pestaña **General** .  
  
3.  En el panel izquierdo, haga clic en **conversión**.  
  
4.  En el panel derecho, revise y cambie la configuración según sea necesario. Para obtener más información sobre esta configuración, vea [configuración del proyecto &#40;conversión&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
5.  Repita los pasos 1-3 para las páginas migración, sincronización, carga de objetos del sistema, GUI y asignación de tipos.  
  
    -   Para obtener información sobre la configuración de la migración, vea [configuración del proyecto &#40;migración&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md).  
  
    -   Para obtener información sobre la configuración de objetos del sistema, vea [configuración del proyecto&#40;cargar objetos del sistema&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-loading-system-objects-db2tosql.md).  
  
    -   Para obtener información sobre la configuración de la sincronización a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [configuración del proyecto&#40;sincronización&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md).  
  
    -   Para obtener información sobre la configuración de la GUI, vea [configuración del proyecto &#40;gui&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md).  
  
    -   Para obtener información sobre la configuración de asignación de tipos de datos, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
## <a name="creating-new-projects"></a>Crear nuevos proyectos  
Para migrar datos de bases de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , primero debe crear un proyecto de.  
  
**Para crear un proyecto**  
  
1.  En el menú **Archivo**, haga clic en **Nuevo proyecto**.  
  
    Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
2.  En el cuadro **nombre** , escriba un nombre para el proyecto.  
  
3.  En el cuadro **Ubicación** , escriba o seleccione una carpeta para el proyecto y, a continuación, haga clic en **Aceptar**.  
  
4.  En el menú desplegable migración, seleccione la versión de destino **que se** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa para la migración. Las opciones disponibles son:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL Database  
  
## <a name="customizing-project-settings"></a>Personalización de la configuración del proyecto  
Además de definir la configuración predeterminada del proyecto que se aplica a todos los proyectos de SSMA nuevos, puede personalizar la configuración de cada proyecto. Para obtener más información, vea [establecer opciones de proyecto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md) y secciones relacionadas.  
  
Al personalizar las asignaciones de tipos de datos entre las bases de datos de origen y de destino, puede definir asignaciones en el nivel de proyecto, base de datos u objeto. Para obtener más información, consulte [asignar tipos de datos de DB2 y SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
## <a name="saving-projects"></a>Guardar proyectos  
Cuando se guarda un proyecto, SSMA conserva la configuración del proyecto y, opcionalmente, los metadatos de la base de datos en el archivo del proyecto.  
  
**Para guardar un proyecto**  
  
-   En el menú **archivo** , haga clic en **Guardar proyecto**.  
  
    Si los esquemas del proyecto han cambiado o no se han convertido, SSMA le pedirá que cargue y guarde los metadatos. Cargar y guardar metadatos le permitirá trabajar sin conexión. También le permite enviar un archivo de proyecto completo a otras personas, como personal de soporte técnico. Si se le pide que guarde los metadatos, haga lo siguiente:  
  
    1.  Para cada esquema que muestre el estado **falta de metadatos**, active la casilla situada junto al nombre de la base de datos.  
  
        El almacenamiento de metadatos puede tardar varios minutos. Si aún no desea guardar los metadatos, no active las casillas de verificación.  
  
    2.  Haga clic en el botón **Save** (Guardar).  
  
        SSMA analizará los esquemas DB2 y guardará los metadatos en el archivo de proyecto.  
  
## <a name="opening-projects"></a>Abrir proyectos  
Al abrir un proyecto, se desconecta de DB2 y de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esto le permite trabajar sin conexión. Para actualizar los metadatos, cargue los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para migrar datos, debe volver a conectarse a DB2 y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Para abrir un proyecto**  
  
1.  Realice uno de los siguientes procedimientos:  
  
    -   En el menú **archivo** , seleccione **proyectos recientes** y, a continuación, haga clic en el proyecto que desea abrir.  
  
    -   En el menú **archivo** , seleccione **Abrir proyecto**, busque el archivo de proyecto. o2ssproj, seleccione el archivo y, a continuación, haga clic en **abrir**.  
  
2.  Para volver a conectarse a DB2, en el menú **archivo** , haga clic en **volver a conectar a DB2**.  
  
3.  Para volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en el menú **archivo** , haga clic en **volver a conectar a SQL Server**.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [conectarse a la base de datos DB2](./connecting-to-db2-database-db2tosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
[Conectar con la base de datos DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
[Conexión a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
