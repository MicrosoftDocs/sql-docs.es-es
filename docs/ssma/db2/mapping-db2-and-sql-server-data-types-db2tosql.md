---
description: Asignación de tipos de datos de DB2 y SQL Server (DB2ToSQL)
title: Asignación de tipos de datos de DB2 y SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e7e939a8-5e76-4509-beaf-5acd1cab505e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 32cf19b192fe67802ae44eaf9ce6e4ca891d02ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072369"
---
# <a name="mapping-db2-and-sql-server-data-types-db2tosql"></a>Asignación de tipos de datos de DB2 y SQL Server (DB2ToSQL)
Los tipos de base de datos DB2 son distintos de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipos de base de datos Al convertir objetos de base de datos DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos, debe especificar cómo se asignan los tipos de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede aceptar las asignaciones de tipos de datos predeterminadas o puede personalizar las asignaciones tal y como se muestra en las secciones siguientes.  
  
## <a name="default-mappings"></a>Asignaciones predeterminadas  
SSMA tiene un conjunto predeterminado de asignaciones de tipos de datos. Para ver la lista de asignaciones predeterminadas, vea [configuración del proyecto &#40;asignación de tipos&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
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
  
    O bien, para personalizar la asignación de tipos de datos en el nivel de base de datos, tabla, vista o procedimiento almacenado, seleccione la base de datos, la categoría de objeto o el objeto en el explorador de metadatos DB2:  
  
    1.  En el explorador de metadatos DB2, seleccione la carpeta o el objeto que desea personalizar.  
  
    2.  En el panel derecho, haga clic en la pestaña **asignación de tipos** .  
  
2.  Para agregar una nueva asignación, haga lo siguiente:  
  
    1.  Haga clic en **Agregar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos DB2 que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y la longitud máxima de los datos en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de los datos en el cuadro **reemplazar con** .  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Para modificar una asignación de tipo de datos, haga lo siguiente:  
  
    1.  Haga clic en **Editar**.  
  
    2.  En **tipo de origen**, seleccione el tipo de datos DB2 que se va a asignar.  
  
    3.  Si el tipo requiere una longitud, especifique la longitud mínima de los datos para la asignación en el cuadro **desde** y la longitud máxima de los datos en el cuadro **para** .  
  
        Esto le permite personalizar la asignación de datos para valores más pequeños y mayores del mismo tipo de datos.  
  
    4.  En **tipo de destino**, seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de destino.  
  
        Algunos tipos requieren una longitud de tipo de datos de destino. Si es necesario, escriba la nueva longitud de los datos en el cuadro **reemplazar con** y, a continuación, [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Para quitar una asignación de tipo de datos personalizada, haga lo siguiente:  
  
    1.  Seleccione la fila de la lista asignación de tipos que contiene la asignación de tipo de datos que desea quitar.  
  
    2.  Haga clic en **Quitar**.  
  
        No se pueden quitar las asignaciones heredadas. Sin embargo, las asignaciones heredadas se reemplazan por asignaciones personalizadas en un objeto o categoría de objeto específicos.  
  
## <a name="next-steps"></a>Pasos siguientes  
El siguiente paso del proceso de migración es el [Informe de evaluación &#40;DB2ToSQL&#41;](../../ssma/db2/assessment-report-db2tosql.md) o la [conversión de esquemas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md). Si crea un informe de evaluación, los objetos DB2 se convierten automáticamente durante la evaluación.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
