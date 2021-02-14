---
title: Asignar tipos de datos de Oracle y SQL Server (OracleToSQL) | Microsoft Docs
description: Obtenga información acerca de cómo personalizar SSMA para asignaciones de Oracle entre tipos de datos de Oracle y SQL Server o acepte el valor predeterminado.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8058123020571ad43e3142fccb920aa22a4b5af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080796"
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>Asignación de tipos de datos de Oracle y de SQL Server (OracleToSQL)
Los tipos de base de datos de Oracle se diferencian de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos Al convertir objetos de base de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datos de Oracle en objetos, debe especificar cómo se asignan los tipos de datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede aceptar las asignaciones de tipos de datos predeterminadas o puede personalizar las asignaciones tal y como se muestra en las secciones siguientes.  
  
## <a name="default-mappings"></a>Asignaciones predeterminadas  
SSMA tiene un conjunto predeterminado de asignaciones de tipos de datos. Para ver la lista de asignaciones predeterminadas, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
## <a name="type-mapping-inheritance"></a>Herencia de asignación de tipos  
Puede personalizar las asignaciones de tipos en el nivel de proyecto, el nivel de categoría de objeto (como todos los procedimientos almacenados) o el nivel de objeto. La configuración se hereda del nivel superior a menos que se invalide en un nivel inferior. Por ejemplo, si asigna **smallmoney** a **Money** en el nivel de proyecto, todos los objetos del proyecto usarán esta asignación a menos que personalice la asignación en el nivel de objeto o categoría.  
  
Al ver la pestaña **asignación de tipos** en SSMA, el fondo está codificado por colores para mostrar las asignaciones de tipo que se heredan. El fondo de una asignación de tipos es amarillo para cualquier asignación de tipo heredada y blanco para cualquier asignación que se especifique en el nivel actual.  
  
## <a name="customizing-data-type-mappings"></a>Personalizar asignaciones de tipos de datos  
En el procedimiento siguiente se muestra cómo asignar tipos de datos en el nivel de proyecto, base de datos o objeto:  
  
**Para asignar tipos de datos**  
  
1.  Para personalizar la asignación de tipos de datos para todo el proyecto, abra el cuadro de diálogo **configuración del proyecto** :  
  
    1.  En el menú **herramientas** , seleccione **configuración del proyecto**.  
  
    2.  En el panel izquierdo, seleccione **asignación de tipos**.  
  
        El gráfico de asignación de tipos y los botones aparecen en el panel derecho.  
  
    O bien, para personalizar la asignación de tipos de datos en el nivel de base de datos, tabla, vista o procedimiento almacenado, seleccione la base de datos, la categoría de objeto o el objeto en el explorador de metadatos de Oracle:  
  
    1.  En el explorador de metadatos de Oracle, seleccione la carpeta o el objeto que desea personalizar.  
  
    2.  En el panel derecho, haga clic en la pestaña **asignación de tipos** .  
  
2.  Para agregar una nueva asignación, haga lo siguiente:  
  
    1.  Haga clic en **Agregar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos de Oracle que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y la longitud máxima de los datos en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de los datos en el cuadro **reemplazar con** .  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Para modificar una asignación de tipo de datos, haga lo siguiente:  
  
    1.  Haga clic en **Editar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos de Oracle que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y la longitud máxima de los datos en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de los datos en el cuadro **reemplazar con** y, a continuación, [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Para quitar una asignación de tipo de datos personalizada, haga lo siguiente:  
  
    1.  Seleccione la fila de la lista asignación de tipos que contiene la asignación de tipo de datos que desea quitar.  
  
    2.  Haga clic en **Quitar**.  
  
        No se pueden quitar las asignaciones heredadas. Sin embargo, las asignaciones heredadas se reemplazan por asignaciones personalizadas en un objeto o categoría de objeto específicos.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso del proceso de migración consiste en [crear un informe de evaluación](assessing-oracle-schemas-for-conversion-oracletosql.md) o [convertir los objetos de base de datos de Oracle en SQL Server sintaxis](converting-oracle-schemas-oracletosql.md). Si crea un informe de evaluación, los objetos de Oracle se convierten automáticamente durante la evaluación.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
