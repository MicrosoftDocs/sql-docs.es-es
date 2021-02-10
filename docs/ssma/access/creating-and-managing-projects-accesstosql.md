---
description: Crear y administrar proyectos (AccessToSQL)
title: Crear y administrar proyectos (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- creating projects
- new projects
- opening projects
- projects
- projects, creating and managing
- saving metadata
- saving projects
ms.assetid: f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ba2e567c5675e602cc80382d42adfcbb89bce90a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045093"
---
# <a name="creating-and-managing-projects-accesstosql"></a>Crear y administrar proyectos (AccessToSQL)
Para migrar bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, primero debe crear un proyecto de SSMA. El proyecto es un archivo que contiene metadatos sobre las bases de datos de Access a las que desea migrar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, metadatos sobre la instancia de destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure que recibirán los objetos y los datos migrados, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] información de conexión y la configuración del proyecto.  
  
## <a name="reviewing-default-project-settings"></a>Revisar la configuración predeterminada del proyecto  
SSMA contiene varias opciones para convertir y sincronizar objetos de base de datos y para convertir datos. La configuración predeterminada para estas opciones es adecuada para muchos usuarios. Sin embargo, antes de crear un nuevo proyecto de SSMA, debe revisar las opciones y, si lo desea, cambiar la configuración predeterminada que se utilizará para todos los proyectos nuevos.  
  
**Para revisar la configuración predeterminada del proyecto**  
  
1.  En el menú **herramientas** , seleccione **configuración predeterminada del proyecto**.  
  
2.  Seleccione el tipo de proyecto en el menú desplegable de la **versión de destino** de la migración para el que se va a ver o cambiar la configuración y, a continuación, haga clic en la pestaña **General** .  
  
3.  En el panel izquierdo, haga clic en **conversión**.  
  
4.  En el panel derecho, revise las opciones. Para obtener más información sobre estas opciones, vea [configuración del proyecto (conversión)](./project-settings-conversion-accesstosql.md).  
  
5.  Cambie las opciones según sea necesario.  
  
6.  Repita los pasos anteriores para las páginas **migración**, **GUI** y **asignación de tipos** .  
  
    -   Para obtener información sobre las opciones de migración, vea [configuración del proyecto (migración)](./project-settings-migration-accesstosql.md).  
  
    -   Para obtener información sobre las opciones de la interfaz de usuario, vea [configuración del proyecto (GUI)](../sybase/project-settings-gui-sybasetosql.md).  
  
    -   Para obtener más información sobre la configuración de asignación de tipos de datos, vea [configuración del proyecto (asignación de tipos)](./project-settings-type-mapping-accesstosql.md).  
  
    -   Para obtener información sobre la configuración de SQL Azure, vea [configuración del proyecto (SQL Azure)](./project-settings-azure-sql-db-accesstosql.md).  
  
**Nota:** SQL Azure configuración solo estará disponible cuando seleccione migración a SQL Azure al crear un proyecto.  
  
## <a name="creating-new-projects"></a>Crear nuevos proyectos  
SSMA comienza sin cargar un proyecto predeterminado. Para migrar datos de bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, debe crear un proyecto de.  
  
**Para crear un nuevo proyecto**  
  
1.  En el menú **Archivo**, seleccione **Nuevo proyecto**.  
  
    Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
2.  En el cuadro **nombre** , escriba un nombre para el proyecto.  
  
3.  En el cuadro **Ubicación** , escriba o seleccione una carpeta para el proyecto.  
  
4.  En el menú desplegable migración, seleccione uno de SQL Server 2005/SQL Server 2008/SQL Server 2012/SQL Server 2014/SQL Server 2016/Azure SQL Database y, a continuación, haga clic en **Aceptar**.  
  
SSMA crea el archivo de proyecto. Ahora puede realizar el siguiente paso para [Agregar una o varias bases de datos de Access](adding-and-removing-access-database-files-accesstosql.md).  
  
## <a name="customizing-project-settings"></a>Personalización de la configuración del proyecto  
Además de definir la configuración predeterminada del proyecto, que se aplica a todos los proyectos de SSMA nuevos, también puede personalizar la configuración de cada proyecto. Para obtener más información, consulte [configuración de las opciones de conversión y migración](setting-conversion-and-migration-options-accesstosql.md).  
  
Al personalizar las asignaciones de tipos de datos entre las bases de datos de origen y de destino, puede definir asignaciones en el nivel de proyecto, base de datos u objeto. Para obtener más información sobre la asignación de tipos, vea [asignar tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md).  
  
## <a name="saving-projects"></a>Guardar proyectos  
Cuando se guarda un proyecto, SSMA conserva los valores del proyecto y, opcionalmente, los metadatos de la base de datos en el archivo del proyecto.  
  
**Para guardar un proyecto**  
  
-   En el menú **archivo** , seleccione **Guardar proyecto**.  
  
    Si las bases de datos del proyecto han cambiado o no se han convertido, SSMA le pedirá que guarde los metadatos en el proyecto. Guardar metadatos permite trabajar sin conexión. También le permite enviar un archivo de proyecto completo a otras personas, incluido el personal de soporte técnico. Si se le pide que guarde los metadatos, haga lo siguiente:  
  
    1.  Para cada base de datos que muestra el estado **falta de metadatos**, active la casilla situada junto al nombre de la base de datos.  
  
        El almacenamiento de metadatos puede tardar varios minutos. Si no desea guardar los metadatos en este momento, no active las casillas de verificación.  
  
    2.  Haga clic en **Save**(Guardar).  
  
        SSMA analizará los esquemas de acceso y guardará los metadatos en el archivo de proyecto.  
  
## <a name="opening-projects"></a>Abrir proyectos  
Al abrir un proyecto, se desconecta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Esto le permite trabajar sin conexión. Para actualizar los objetos de base de datos de carga de metadatos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Para migrar datos, debe volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
**Para abrir un proyecto**  
  
1.  Realice uno de los siguientes procedimientos:  
  
    -   En el menú **archivo** , seleccione **proyectos recientes** y, a continuación, seleccione el proyecto que desea abrir.  
  
    -   En el menú **archivo** , seleccione **Abrir proyecto**, busque el archivo de proyecto. a2ssproj, seleccione el archivo y, a continuación, haga clic en **abrir**.  
  
2.  Para volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en el menú **archivo** , seleccione **volver a conectar a SQL Server**.  
  
3.  Para volver a conectarse a SQL Azure, en el menú **archivo** , seleccione **volver a conectar a SQL Azure.**  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [Agregar una o varias bases de datos de Access](adding-and-removing-access-database-files-accesstosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Agregar y quitar archivos de base de datos de Access](adding-and-removing-access-database-files-accesstosql.md)  
