---
description: Editar asignación de tipo (MySQLToSQL)
title: Editar la asignación de tipos (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 184f7ab2-725f-491e-a15b-b889f2fb6a68
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b1ddf264835cf0a9a008335787353d7a1051431f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016605"
---
# <a name="edit-type-mapping-mysqltosql"></a>Editar asignación de tipo (MySQLToSQL)
El cuadro de diálogo **Editar asignación de tipos** permite especificar cómo se asignan los tipos entre los objetos de base de datos de origen y de destino.  
  
Puede tener acceso a este cuadro de diálogo en varios lugares:  
  
-   Al seleccionar una base de datos de origen o un objeto de base de datos, la pestaña **asignación de tipos** aparece a la derecha del explorador de metadatos. Haga clic en **Agregar** para agregar una nueva asignación de tipos o haga clic en **Editar** para cambiar una asignación de tipos existente.  
  
-   En el menú **herramientas** , seleccione **configuración del proyecto** o **configuración predeterminada del proyecto**. En el cuadro de diálogo resultante, seleccione **asignación de tipos**. Haga clic en **Agregar** para agregar una nueva asignación de tipos o haga clic en **Editar** para cambiar una asignación de tipos existente.  
  
-   Las asignaciones de tipos específicas de tabla invalidan las asignaciones de tipo de proyecto y base de datos. Las asignaciones específicas de la base de datos invalidan las asignaciones del proyecto.  
  
## <a name="options"></a>Opciones  
  
##### <a name="source-type"></a>Tipo de origen  
Seleccione el tipo de datos de origen que se va a asignar a un tipo de datos SQL Server.  
  
Si el tipo de datos es de longitud variable, los campos siguientes aparecerán en **sourceType**:  
  
##### <a name="from"></a>De  
Especifique la longitud mínima de esta asignación. Por ejemplo, para el tipo de datos **nchar** , puede especificar 10 para especificar que esta asignación es para un intervalo que empieza en **NCHAR (10).**  
  
##### <a name="to"></a>En  
Especifique la longitud máxima de esta asignación. Por ejemplo, para el tipo de datos **nchar** , puede especificar 20 para especificar que esta asignación es para un intervalo que termina en **NCHAR (20).**  
  
##### <a name="target-type"></a>Tipo de destino  
Seleccione el tipo de datos SQL Server al que se asigna el tipo de datos de origen. Cuando SSMA crea la tabla o en el SQL Server, el tipo de datos de origen cambiará a este tipo de datos.  
  
Si el tipo de datos es de longitud variable, el siguiente campo aparecerá en **tipo de destino**:  
  
##### <a name="replace-with"></a>Reemplazar por  
Especifique la longitud de destino de esta asignación. Por ejemplo, para el tipo de datos **nvarchar** , puede especificar 20 para especificar que el tipo de datos de origen especificado debe estar asignado a **nvarchar (20).**  
  
