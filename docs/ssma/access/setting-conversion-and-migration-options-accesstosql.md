---
description: Configuración de las opciones de conversión y migración (AccessToSQL)
title: Configuración de las opciones de conversión y migración (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, setting options
- migration options
- modes
- options, conversion settings
- project settings
- schemas
ms.assetid: 0a7304df-2f35-4453-96ef-7ac83dea1167
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 154eaa42bf08f622f3e08359f025284752134bc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066325"
---
# <a name="setting-conversion-and-migration-options-accesstosql"></a>Configuración de las opciones de conversión y migración (AccessToSQL)
Para cada proyecto de SSMA, puede establecer opciones de nivel de proyecto. Estas opciones especifican cómo se convierten los objetos, cómo se migran los datos y cómo se asignan los tipos de datos de origen a los tipos de datos de destino. Antes de convertir objetos a o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure o migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, compruebe que las opciones de configuración son adecuadas para el proyecto.  
  
## <a name="configuration-options-and-modes"></a>Opciones de configuración y modos  
SSMA tiene cuatro conjuntos de opciones de configuración y cuatro modos para configurar estas opciones: default, optimistic, Full y Custom. El modo predeterminado es el recomendado para la mayoría de los usuarios. Use el modo optimista para las conversiones simples. Use el modo completo si desea ver todos los mensajes. En el modo personalizado, se establecen las opciones.  
  
La configuración se describe en la sección "referencia de la interfaz de usuario" de esta documentación. Para obtener más información sobre la configuración y cómo se aplica la configuración en cada modo, vea los temas siguientes:  
  
-   [Configuración del proyecto (conversión)](./project-settings-conversion-accesstosql.md)  
  
-   [Configuración del proyecto (migración)](./project-settings-migration-accesstosql.md)  
  
-   [Configuración del proyecto (GUI)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [Configuración del proyecto (asignación de tipo)](./project-settings-type-mapping-accesstosql.md)  
  
-   [Configuración del proyecto (SQL Azure)](./project-settings-azure-sql-db-accesstosql.md)  
  
## <a name="setting-project-options"></a>Configuración de opciones de proyecto  
En SSMA, puede configurar los valores predeterminados para todos los proyectos. Estas opciones se guardan en el archivo de configuración de SSMA y se aplican a cualquier proyecto nuevo que cree.  
  
**Para establecer las opciones predeterminadas del proyecto**  
  
1.  En el menú **herramientas** , seleccione **configuración predeterminada del proyecto**.  
  
2.  En el cuadro de diálogo **configuración predeterminada del proyecto** , realice una de las acciones siguientes:  
  
    -   Seleccione el tipo de proyecto de migración para el que es necesario ver o cambiar la configuración en la lista desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione **conversión o migración o SQL Azure**.  
  
        > [!NOTE]  
        > SQL Azure opción solo está disponible en la pestaña **General** si el tipo de proyecto creado es SQL Azure.  
  
    -   Para seleccionar un modo predefinido, seleccione **predeterminado**, **optimista** o **completo** en el cuadro desplegable **modo** .  
  
    -   Para especificar un modo personalizado, seleccione **personalizado** en el cuadro **modo** , seleccione una opción en el panel izquierdo, haga clic en la configuración o el valor en el panel derecho y, a continuación, seleccione o especifique el nuevo valor o valor.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
También puede personalizar la configuración del proyecto actual. Esta configuración se guarda en el archivo de proyecto actual.  
  
**Para personalizar la configuración del proyecto actual**  
  
1.  En el menú **herramientas** , seleccione **configuración del proyecto**.  
  
2.  En el cuadro de diálogo **configuración del proyecto** , realice una de las acciones siguientes:  
  
    -   Para seleccionar un modo predefinido, seleccione **predeterminado**, **optimista** o **completo** en el cuadro desplegable **modo** .  
  
    -   Para especificar un modo personalizado, seleccione **personalizado** en el cuadro **modo** , seleccione una opción en el panel izquierdo, haga clic en la configuración o el valor en el panel derecho y, a continuación, seleccione o especifique el nuevo valor o valor.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso de la migración depende de las necesidades del proyecto:  
  
-   Para personalizar la asignación de los tipos de datos de origen y de destino, vea [asignar tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md) .  
  
-   Para personalizar la asignación de bases de datos de origen y de destino, consulte [asignación de bases de datos de origen y de destino](mapping-source-and-target-databases-accesstosql.md) .  
  
-   De lo contrario, puede convertir las definiciones de objetos de base de datos de Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure definiciones de objeto. Para obtener más información, vea [convertir objetos de base de datos de Access](converting-access-database-objects-accesstosql.md) .  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
