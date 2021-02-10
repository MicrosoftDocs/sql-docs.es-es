---
description: Actualizar desde la base de datos (MySQLToSQL)
title: Actualizar desde la base de datos (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 59a6db8f-2db6-4071-9005-928a7231de92
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 60a23edbcbad344ab3bf42286944f7c72acb8f92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015185"
---
# <a name="refresh-from-database-mysqltosql"></a>Actualizar desde la base de datos (MySQLToSQL)
El cuadro de diálogo **actualizar desde base de datos** permite seleccionar los objetos que se van a actualizar desde la base de datos MySQL. Las filas del cuadro de diálogo están codificadas por colores en función del estado de los metadatos:  
  
-   Si los metadatos del objeto han cambiado localmente y en la base de datos MySQL, la fila es azul.  
  
-   Si los metadatos del objeto han cambiado en la base de datos MySQL pero no en SSMA, la fila es amarilla.  
  
-   Si los metadatos del objeto han cambiado localmente, pero no en la base de datos MySQL, la fila es verde.  
  
-   Si el objeto es nuevo en la base de datos MySQL, la fila es de color rosa.  
  
Puede especificar la configuración de actualización de objetos predeterminada en el cuadro de diálogo **configuración del proyecto** . Para obtener más información, vea [configuración del proyecto &#40;Synchronization&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
Para tener acceso al cuadro **de diálogo actualizar desde base** de datos, haga clic con el botón secundario en un objeto en el explorador de metadatos MySQL y haga clic en **actualizar desde base de** datos.  
  
## <a name="options"></a>Opciones  
  
|||  
|-|-|  
|**Término**|**Definición**|  
|**Contraer (-)**|Contrae todos los grupos de objetos para ocultar objetos individuales.|  
|**Expandir (+)**|Expanda todos los grupos de objetos para mostrar objetos individuales.|  
|**Ocultar/mostrar objetos iguales**|Oculta los objetos de la lista si los metadatos del objeto son los mismos en la base de datos MySQL y en SSMA.|  
|**Actualizar desde base de datos (botón de flecha)**|Use el botón de flecha para especificar que los metadatos de los objetos seleccionados se deben actualizar en SSMA.|  
|**No actualizar desde base de datos (botón X)**|Use el botón X para especificar que los metadatos de los objetos seleccionados no se deben actualizar en SSMA.|  
|**Leyenda**|Muestra un cuadro de diálogo de **leyenda** . La leyenda contiene la asignación entre los colores de fila y los Estados de los metadatos.<br /><br />Para mantener el cuadro de diálogo **leyenda** en la parte superior del cuadro de diálogo **actualizar desde la base de datos** , active la casilla **Mostrar en la parte superior** .|  
  
