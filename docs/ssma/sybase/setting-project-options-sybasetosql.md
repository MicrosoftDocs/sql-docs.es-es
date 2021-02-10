---
description: Configuración de opciones de proyecto (SybaseToSQL)
title: Establecer opciones de proyecto (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Project Options Setting
ms.assetid: 97b70fc8-1f68-4f15-8e22-db5b784ea4ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: fa8e45af93faa4ef175d129487a1d655a6d02ead
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030763"
---
# <a name="setting-project-options-sybasetosql"></a>Configuración de opciones de proyecto (SybaseToSQL)
Para cada proyecto de SSMA, puede establecer opciones de nivel de proyecto. Estas opciones especifican la conversión de objetos, la carga de objetos, SQL Azure, la interfaz de usuario y la configuración de migración de datos. Antes de convertir objetos a o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure o migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, compruebe que las opciones de configuración son adecuadas para el proyecto.  
  
SSMA le permite configurar opciones predeterminadas para todos los proyectos. Estas opciones se aplican a cualquier proyecto nuevo que cree. Después, puede personalizar las opciones de cada proyecto.  
  
## <a name="configuration-options-and-modes"></a>Opciones de configuración y modos  
SSMA tiene cinco conjuntos de opciones de configuración del proyecto:  
  
1.  Información del proyecto  
  
2.  General (conversión, migración y recopilación de datos)  
  
3.  Synchronization  
  
4.  GUI  
  
5.  Asignación de tipos  
  
También tiene cuatro modos para configurar estas opciones:  
  
1.  Valor predeterminado  
  
2.  Optimistic  
  
3.  Completo  
  
4.  Personalizada  
  
El modo predeterminado es el recomendado para la mayoría de los usuarios. El modo optimista mantiene más la sintaxis actual de Sybase Adaptive Server Enterprise (ASE) y es más fácil de leer. Sin embargo, mantener la sintaxis actual podría no ser preciso. Si la sintaxis de ASE se debe convertir a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] una sintaxis equivalente o SQL Azure, el modo completo realiza una conversión completa, pero es posible que el código resultante sea más difícil de leer. En el modo personalizado, se establecen las opciones.  
  
La configuración se describe en la sección referencia de la interfaz de usuario de esta documentación. Para obtener más información sobre la configuración y cómo se aplica la configuración en cada modo, vea los temas siguientes:  
  
-   [Configuración del proyecto &#40;conversión&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;migración&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;sincronización&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;GUI&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;asignación de tipos&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;Azure SQL Database &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)  
  
## <a name="setting-project-options"></a>Configuración de opciones de proyecto  
En SSMA, puede configurar los valores predeterminados para todos los proyectos. Estas opciones se guardan en el archivo de configuración de SSMA y se aplican a cualquier proyecto nuevo que cree.  
  
**Para establecer las opciones predeterminadas del proyecto**  
  
1.  En el menú **herramientas** , seleccione **configuración predeterminada del proyecto**.  
  
2.  En el cuadro de diálogo **configuración predeterminada del proyecto** , use uno de los procedimientos siguientes:  
  
    -   Seleccione el tipo de proyecto de migración para el que es necesario ver o cambiar la configuración en el menú desplegable de la **versión de destino** de la migración, haga clic en general en la parte inferior del panel izquierdo y, a continuación, seleccione conversión o migración o SQL Azure.  
  
    -   Para seleccionar un modo predefinido, en el cuadro desplegable **modo** , seleccione **predeterminado**, **optimista** o **completo**.  
  
    -   Para especificar la configuración personalizada, simplemente seleccione o escriba los nuevos valores o valores.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
También puede personalizar la configuración del proyecto actual. Esta configuración se guarda en el archivo de proyecto actual.  
  
**Para personalizar la configuración del proyecto actual**  
  
1.  En el menú **herramientas** , seleccione **configuración del proyecto**.  
  
2.  En el cuadro de diálogo **configuración del proyecto** , use uno de los procedimientos siguientes:  
  
    -   Para seleccionar un modo predefinido, en el cuadro desplegable **modo** , seleccione **predeterminado**, **optimista** o **completo**.  
  
    -   Para especificar un modo personalizado, en el cuadro desplegable **modo** , seleccione **personalizado**, seleccione una opción en el panel izquierdo, haga clic en la configuración o el valor en el panel derecho y, a continuación, seleccione o especifique la nueva configuración o valor.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso de la migración depende de las necesidades del proyecto:  
  
-   Si desea personalizar la asignación de los tipos de datos de origen y de destino, consulte [asignación de tipos de datos de Sybase ase y SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
-   De lo contrario, puede convertir las definiciones de objetos de base de datos de Sybase en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure definiciones de objeto. Para obtener más información, consulte [conversión de objetos de base de datos de Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de Sybase ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
