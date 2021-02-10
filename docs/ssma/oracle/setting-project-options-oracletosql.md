---
description: Configuración de opciones de proyecto (OracleToSQL)
title: Establecer opciones de proyecto (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Configuration Options and Modes
ms.assetid: a324d07d-cfdf-43bd-98a0-acf332c5a4db
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7de2ab3447a35f9d55489673adaa532866a4f89f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067680"
---
# <a name="setting-project-options-oracletosql"></a>Configuración de opciones de proyecto (OracleToSQL)
Puede establecer opciones de nivel de proyecto para cada proyecto de SSMA. Estas opciones especifican la conversión de objetos, la carga de objetos, la interfaz de usuario y la configuración de migración de datos. Antes de convertir objetos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , compruebe que las opciones de configuración son adecuadas para el proyecto.  
  
SSMA le permite configurar opciones predeterminadas para todos los proyectos. Estas opciones se aplican a cualquier proyecto nuevo que cree. Después, puede personalizar las opciones de cada proyecto.  
  
## <a name="configuration-options-and-modes"></a>Opciones de configuración y modos  
SSMA tiene cinco conjuntos de opciones de configuración del proyecto:  
  
-   Información del proyecto  
  
-   General (conversión, migración y carga de objetos)  
  
-   Synchronization  
  
-   GUI  
  
-   Asignación de tipos  
  
También tiene cuatro modos para configurar estas opciones:  
  
-   Valor predeterminado  
  
-   Optimistic  
  
-   Completo  
  
-   Personalizada  
  
El modo predeterminado es el recomendado para la mayoría de los usuarios. El modo optimista mantiene más la sintaxis de Oracle actual y es más fácil de leer. Sin embargo, mantener la sintaxis actual podría no ser preciso. Si la sintaxis de Oracle se debe convertir en una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis equivalente, el modo completo realiza la conversión más completa, pero es posible que el código resultante sea más difícil de leer. En el modo personalizado, se establecen las opciones.  
  
Para obtener más información sobre la configuración y cómo se aplica la configuración en cada modo, vea los temas siguientes:  
  
-   [Configuración del proyecto &#40;conversión&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md)  
  
-   [Configuración del proyecto &#40;migración&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-migration-oracletosql.md)  
  
-   [Configuración del proyecto&#40;sincronización&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-synchronization-oracletosql.md)  
  
-   [Configuración del proyecto &#40;GUI&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-gui-oracletosql.md)  
  
-   [Configuración del proyecto &#40;asignación de tipos&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md)  
  
## <a name="setting-project-options"></a>Configuración de opciones de proyecto  
En SSMA, puede configurar los valores predeterminados para todos los proyectos. Estas opciones se guardan en el archivo de configuración de SSMA y se aplican a cualquier proyecto nuevo que cree.  
  
**Para establecer las opciones predeterminadas del proyecto**  
  
1.  En el menú **herramientas** , haga clic en **configuración predeterminada del proyecto**.  
  
2.  En el cuadro de diálogo **configuración predeterminada del proyecto** , use uno de los procedimientos siguientes:  
  
    -   Seleccione el tipo de proyecto de migración para el que se deben ver o cambiar las opciones de configuración en el menú desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione conversión o migración.  
  
    -   Para seleccionar un modo predefinido, en el cuadro desplegable **modo** , seleccione **predeterminado**, **optimista** o **completo**.  
  
    -   Para especificar la configuración personalizada, seleccione o especifique la nueva configuración o los valores.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
También puede personalizar la configuración del proyecto actual. Esta configuración se guarda en el archivo de proyecto actual.  
  
**Para personalizar la configuración del proyecto actual**  
  
1.  En el menú **herramientas** , haga clic en **configuración del proyecto**.  
  
2.  En el cuadro de diálogo **configuración del proyecto** , use uno de los procedimientos siguientes:  
  
    -   Para seleccionar un modo predefinido, en el cuadro desplegable **modo** , seleccione **predeterminado**, **optimista** o **completo**.  
  
    -   Para especificar un modo personalizado, en el cuadro **modo** , seleccione **personalizada** y, a continuación, seleccione la configuración de proyecto adecuada.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso de la migración depende de las necesidades del proyecto:  
  
-   Para personalizar la asignación de los tipos de datos de origen y de destino, vea [asignar tipos de datos de Oracle y SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
-   De lo contrario, puede convertir las definiciones de objetos de base de datos de Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiciones de objeto. Para obtener más información, vea [convertir esquemas de Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Asignar tipos de datos de Oracle y SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md)  
  
