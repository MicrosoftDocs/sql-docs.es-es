---
description: Actualizar desde la base de datos (DB2ToSQL)
title: Actualizar desde la base de datos (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 613a8368-b372-443f-8252-fb6dc31a003d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7c33ccf20e2b90fd967b5b15b491710d31ea83b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071941"
---
# <a name="refresh-from-database-db2tosql"></a>Actualizar desde la base de datos (DB2ToSQL)
El cuadro de diálogo **actualizar desde base de datos** permite seleccionar los objetos que se van a actualizar desde la base de datos DB2. Las filas del cuadro de diálogo están codificadas por colores en función del estado de los metadatos:  
  
-   Si los metadatos del objeto han cambiado localmente y en la base de datos de DB2, la fila es azul.  
  
-   Si los metadatos del objeto han cambiado en la base de datos DB2 pero no en SSMA, la fila es amarilla.  
  
-   Si los metadatos del objeto han cambiado localmente, pero no en la base de datos DB2, la fila es verde.  
  
-   Si el objeto es nuevo en la base de datos de DB2, la fila es de color rosa.  
  
Puede especificar la configuración de actualización de objetos predeterminada en el cuadro de diálogo **configuración del proyecto** . Para obtener más información, vea [configuración del proyecto&#40;Synchronization&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md).  
  
Para tener acceso al cuadro **de diálogo actualizar desde base** de datos, haga clic con el botón secundario en un objeto en el explorador de metadatos DB2 y haga clic en **actualizar desde base de** datos.  
  
## <a name="options"></a>Opciones  
**Contraer (-)**  
Contrae todos los grupos de objetos para ocultar objetos individuales.  
  
**Expandir (+)**  
Expanda todos los grupos de objetos para mostrar objetos individuales.  
  
**Ocultar/mostrar objetos iguales**  
Oculta los objetos de la lista si los metadatos del objeto son los mismos en la base de datos DB2 y en SSMA.  
  
**Actualizar desde base de datos (botón de flecha)**  
Use el botón de flecha para especificar que los metadatos de los objetos seleccionados se deben actualizar en SSMA.  
  
**No actualizar desde base de datos (botón X)**  
Use el botón X para especificar que los metadatos de los objetos seleccionados no se deben actualizar en SSMA.  
  
**Leyenda**  
Muestra un cuadro de diálogo de **leyenda** . La leyenda contiene la asignación entre los colores de fila y los Estados de los metadatos.  
  
Para mantener el cuadro de diálogo **leyenda** en la parte superior del cuadro de diálogo **actualizar desde la base de datos** , active la casilla **Mostrar en la parte superior** .  
  
