---
description: Configuración de opciones de proyecto (MySQLToSQL)
title: Establecer opciones de proyecto (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Setting project options,configuration options
ms.assetid: 08820d88-e157-4d49-9401-38580dd7ec2d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75c4f6ca899135141aa68ec147889fcce8cfa549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074516"
---
# <a name="setting-project-options-mysqltosql"></a>Configuración de opciones de proyecto (MySQLToSQL)
Para cada proyecto de SSMA, puede establecer opciones de nivel de proyecto. Estas opciones especifican cómo se convierten los objetos, cómo se migran los datos y cómo se asignan los tipos de datos de origen a los tipos de datos de destino.  Antes de convertir los objetos en SQL Server o SQL Azure o migrar datos a SQL Server o SQL Azure, compruebe que las opciones de configuración son adecuadas para el proyecto.  
  
SSMA le permite configurar las opciones predeterminadas para todos los proyectos. Estas opciones se aplican a cualquier proyecto nuevo que cree. Después, puede personalizar las opciones de cada proyecto.  
  
## <a name="configuration-options-and-modes"></a>Opciones de configuración y modos  
SSMA tiene cinco conjuntos de opciones de configuración del proyecto:  
  
-   Información del proyecto  
  
-   General (conversión, migración y SQL Azure)  
  
-   Synchronization  
  
-   GUI  
  
-   Asignación de tipos  
  
La configuración del proyecto se puede configurar de cuatro maneras:  
  
-   Valor predeterminado  
  
-   Optimistic  
  
-   Completo  
  
-   Personalizada  
  
El modo predeterminado es el recomendado para la mayoría de los usuarios. El modo optimista mantiene más la sintaxis actual de MySQL y es más fácil de leer. Sin embargo, mantener la sintaxis actual podría no ser preciso. Si la sintaxis de MySQL se debe convertir en una sintaxis equivalente SQL Server o SQL Azure, el modo completo realiza la conversión más completa. Sin embargo, el código resultante puede ser más difícil de leer. En el modo personalizado, puede establecer las opciones.  
  
Para obtener más información sobre la configuración y cómo se aplica la configuración en cada modo, vea los temas siguientes:  
  
-   [Configuración del proyecto &#40;conversión&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
-   [Configuración del proyecto &#40;migración&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)  
  
-   [Configuración del proyecto (GUI) (SSMA Common)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [Configuración del proyecto &#40;asignación de tipos&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)  
  
-   [Configuración del proyecto &#40;sincronización&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
-   [Configuración del proyecto &#40;Azure SQL Database&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)  
  
## <a name="setting-project-options"></a>Configuración de opciones de proyecto  
En SSMA, puede configurar los valores predeterminados para todos los proyectos. Estas opciones se guardan en el archivo de configuración de SSMA y se aplican a cualquier proyecto nuevo que cree.  
  
**Para establecer las opciones predeterminadas del proyecto**  
  
1.  En el menú **herramientas** , haga clic en **configuración predeterminada del proyecto**.  
  
2.  En el cuadro de diálogo **configuración predeterminada del proyecto** , use uno de los procedimientos siguientes:  
  
    1.  Seleccione el tipo de proyecto de migración para el que es necesario ver o cambiar la configuración del menú desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione **conversión o migración o SQL Azure** opción.  
  
    2.  Para seleccionar un modo predefinido, seleccione **predeterminado**, **optimista** o **completo** en el cuadro desplegable **modo** .  
  
    3.  Para especificar la configuración personalizada, seleccione o especifique la nueva configuración o los valores.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
También puede personalizar la configuración del proyecto actual. La configuración se guarda en el archivo del proyecto actual.  
  
**Para personalizar la configuración del proyecto actual**  
  
1.  En el menú **herramientas** , haga clic en **ProjectSettings**.  
  
2.  En el cuadro de diálogo **ProjectSettings** , use uno de los procedimientos siguientes:  
  
    1.  Para seleccionar un modo predefinido, seleccione **predeterminado**, **optimista** o **completo** en el cuadro desplegable **modo** .  
  
    2.  Para especificar un modo personalizado, seleccione **personalizado** en el cuadro desplegable **modo** . Y, a continuación, seleccione la configuración de proyecto adecuada.  
  
3.  Haga clic en **Aceptar** para guardar la configuración.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso de la migración depende de las necesidades del proyecto:  
  
-   Para personalizar la asignación de los tipos de datos de origen y de destino, consulte [asignación de tipos de datos de SQL Server y MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   De lo contrario, puede convertir las definiciones de objetos de base de datos MySQL en SQL Server o SQL Azure definiciones de objeto. Para obtener más información, vea [convertir las bases de datos MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Asignación de tipos de datos de MySQL y SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
