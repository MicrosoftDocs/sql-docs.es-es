---
description: Asignación de tipos de datos de Sybase ASE y de SQL Server (SybaseToSQL)
title: Asignación de tipos de datos de Sybase ASE y SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping Sybase ASE Schemas to SQL Server Schemas
- Type Mapping Settings
ms.assetid: 784365d3-df4e-47ab-8ee0-d8392b52f510
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a8b2ba53ef5f3dcb895d217e2acef3acc15e8784
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017675"
---
# <a name="mapping-sybase-ase-and-sql-server-data-types-sybasetosql"></a>Asignación de tipos de datos de Sybase ASE y de SQL Server (SybaseToSQL)
Los tipos de base de datos de Sybase Adaptive Server Enterprise (ASE) se diferencian de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de Azure SQL Database. Al convertir objetos de base de datos de ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos de o SQL Azure, debe especificar cómo se asignan los tipos de datos de ase a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede aceptar las asignaciones de tipos de datos predeterminadas o puede personalizar las asignaciones tal y como se muestra en las secciones siguientes.  
  
## <a name="default-mappings"></a>Asignaciones predeterminadas  
SSMA tiene un conjunto predeterminado de asignaciones de tipos de datos. Para ver la lista de asignaciones predeterminadas, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md).  
  
## <a name="type-mapping-inheritance"></a>Herencia de asignación de tipos  
Puede personalizar las asignaciones de tipos en el nivel de proyecto, el nivel de categoría de objeto (como todos los procedimientos almacenados) o el nivel de objeto. La configuración se hereda del nivel superior a menos que se invalide en un nivel inferior. Por ejemplo, si asigna **smallmoney** a **Money** en el nivel de proyectos, todos los objetos del proyecto usarán esta asignación a menos que personalice la asignación en el nivel de categoría de objeto o de objeto.  
  
Al ver la pestaña **asignación de tipos** en SSMA, el fondo está codificado por colores para mostrar las asignaciones de tipo que se heredan. El fondo de una asignación de tipos es amarillo para cualquier asignación de tipo heredada y blanco para cualquier asignación especificada en el nivel actual.  
  
## <a name="customizing-data-type-mappings"></a>Personalizar asignaciones de tipos de datos  
En el procedimiento siguiente se muestra cómo asignar tipos de datos en el nivel de proyecto, base de datos u objeto.  
  
**Para asignar tipos de datos**  
  
1.  Para personalizar la asignación de tipos de datos para todo el proyecto, abra el cuadro de diálogo **configuración del proyecto** :  
  
    1.  En el menú **herramientas** , seleccione **configuración del proyecto**.  
  
    2.  En el panel izquierdo, seleccione **asignación de tipos**.  
  
        El gráfico de asignación de tipos y los botones aparecen en el panel derecho.  
  
    O bien, para personalizar la asignación de tipos de datos en el nivel de base de datos, tabla, vista o procedimiento almacenado, seleccione la base de datos, la categoría de objeto o el objeto en el explorador de metadatos de Sybase:  
  
    1.  En el explorador de metadatos de Sybase, seleccione la carpeta o el objeto que desea personalizar.  
  
    2.  En el panel derecho, haga clic en la pestaña **asignación de tipos** .  
  
2.  Para agregar una nueva asignación, haga lo siguiente:  
  
    1.  Haga clic en **Agregar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos ase que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y especifique la longitud máxima de los datos para la asignación en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos destino o SQL Azure.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de los datos en el cuadro **reemplazar con** .  
  
    5.  Haga clic en **OK**.  
  
3.  Para editar una asignación de tipo de datos, haga lo siguiente:  
  
    1.  Haga clic en **Editar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos ase que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y especifique la longitud máxima de los datos para la asignación en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos destino o SQL Azure.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de datos en el cuadro **reemplazar con** y, a continuación, haga clic en **Aceptar**.  
  
4.  Para quitar una asignación de tipo de datos personalizada, haga lo siguiente:  
  
    1.  Seleccione la fila de la lista asignación de tipos que contiene la asignación de tipo de datos que desea quitar.  
  
    2.  Haga clic en **Quitar**.  
  
        No se pueden quitar las asignaciones heredadas. Sin embargo, las asignaciones heredadas se reemplazan por asignaciones personalizadas en un objeto o categoría de objeto específicos.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso del proceso de migración consiste en [crear un informe de evaluación](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md) o [convertir los objetos de base de datos de Sybase ase en SQL Server o SQL Azure sintaxis](converting-sybase-ase-database-objects-sybasetosql.md). Si crea un informe de evaluación, los objetos de Sybase ASE se convierten automáticamente durante la evaluación.  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de Sybase ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
