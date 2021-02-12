---
description: Asignación de tipos de datos de MySQL y de SQL Server (MySQLToSQL)
title: Asignación de tipos de datos de MySQL y SQL Server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, customize data type mapping
- Mapping, Type mapping
ms.assetid: 14f98054-13b4-4231-a6b0-2452f3b9941d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f7024f944030714e7aa2431815e326b1be5be701
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077217"
---
# <a name="mapping-mysql-and-sql-server-data-types-mysqltosql"></a>Asignación de tipos de datos de MySQL y de SQL Server (MySQLToSQL)
Los tipos de base de datos MySQL se diferencian de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de Azure SQL Database. Al convertir objetos de base de datos MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o SQL Azure, debe especificar cómo se asignan los tipos de datos de MySQL a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede aceptar las asignaciones de tipos de datos predeterminadas o puede personalizar las asignaciones tal y como se muestra en los procedimientos siguientes.  
  
## <a name="default-mappings"></a>Asignaciones predeterminadas  
SSMA tiene un conjunto predeterminado de asignaciones de tipos de datos. Para ver la lista de asignaciones predeterminadas, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
## <a name="type-mapping-inheritance"></a>Herencia de asignación de tipos  
Puede personalizar las asignaciones de tipos en el nivel de proyecto, el nivel de categoría de objeto (como todos los procedimientos almacenados) o el nivel de objeto. La configuración se hereda del nivel superior a menos que se invalide en un nivel inferior. Por ejemplo, si asigna **smallint** a **int** en el nivel de proyecto, todos los objetos del proyecto usarán esta asignación a menos que personalice la asignación en el nivel de objeto o categoría.  
  
Al ver la pestaña **asignación de tipos** en SSMA, el fondo está codificado por colores para mostrar las asignaciones de tipo que se heredan. El fondo de una asignación de tipos es amarillo para cualquier asignación de tipo heredada y blanco para cualquier asignación que se especifique en el nivel actual.  
  
## <a name="customizing-data-type-mappings"></a>Personalizar asignaciones de tipos de datos  
  
-   **Para asignar tipos de datos:**  
  
    En los procedimientos siguientes se muestra cómo asignar tipos de datos en el nivel de proyecto, base de datos u objeto de base de datos:  
  
    1.  Para personalizar la asignación de tipos de datos para todo el proyecto, abra el cuadro de diálogo **configuración del proyecto** . En el menú herramientas, seleccione **configuración del proyecto**.  
  
        En el panel izquierdo, seleccione **asignación de tipos**. El gráfico de asignación de tipos y los botones aparecen en el panel derecho.  
  
    2.  Para personalizar las asignaciones de tipos de datos en el nivel de base de datos o de tabla, seleccione la base de datos o la tabla en el explorador de metadatos de MySQL. En el explorador de metadatos de MySQL, seleccione la carpeta o el objeto que desea personalizar.  
  
        En el panel derecho, haga clic en **asignación de tipos**.  
  
-   **Para agregar una nueva asignación, haga lo siguiente:**  
  
    1.  En el panel asignación de tipos, haga clic en **Agregar** .  
  
    2.  En el cuadro de diálogo nueva asignación de tipos, en **tipo de origen**, seleccione el tipo de datos MySQL que desea asignar.  
  
    3.  Si el tipo requiere una longitud, especifique las longitudes de datos mínima y máxima para la asignación; para ello, active las casillas **desde** y **hasta** y, a continuación, escriba los valores.  
  
    4.  Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos. En **tipo de destino**, seleccione el tipo de datos SQL Server o SQL Azure de destino.  
  
        1.  Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de datos en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
        2.  Algunos tipos requieren una **escala** y **precisión** del tipo de datos de destino. Si es necesario, escriba la nueva precisión y la escala en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
-   **Para editar una asignación de tipos, haga lo siguiente:**  
  
    1.  En el panel asignación de tipos, haga clic en **Editar**.  
  
    2.  En el cuadro de diálogo lista de asignación de tipos, en **tipo de origen**, seleccione el tipo de datos MySQL que desea asignar.  
  
    3.  Si el tipo requiere una longitud, especifique las longitudes de datos mínima y máxima para la asignación; para ello, active las casillas **desde** y **hasta** y, a continuación, escriba los valores.  
  
    Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos. En **tipo de destino**, seleccione el tipo de datos SQL Server o SQL Azure de destino.  
  
    -  Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de datos en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
    -  Algunos tipos requieren una **escala** y **precisión** del tipo de datos de destino. Si es necesario, escriba la nueva precisión y la escala en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
-   **Para quitar una asignación de tipo de datos, haga lo siguiente:**  
  
    1.  En el panel asignación de tipos, seleccione la fila de la lista asignación de tipos que contiene la asignación de tipo de datos que desea quitar.  
  
    2.  Haga clic en **Quitar**.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [crear un informe de evaluación](assessing-mysql-databases-for-conversion-mysqltosql.md) o [convertir los objetos de base de datos MySQL en SQL Server o SQL Azure sintaxis](converting-mysql-databases-mysqltosql.md). Si crea un informe, los objetos de MySQL se convierten automáticamente durante la evaluación.  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
