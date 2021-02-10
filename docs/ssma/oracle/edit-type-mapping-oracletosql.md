---
description: Editar asignación de tipo (OracleToSQL)
title: Editar la asignación de tipos (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7078b4ed-c779-4bf3-8db8-f9dcb3edd50f
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 79da4985cef777225cbf805c103c782b512e2d5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058675"
---
# <a name="edit-type-mapping-oracletosql"></a>Editar asignación de tipo (OracleToSQL)
El cuadro de diálogo **Editar asignación de tipos** permite especificar cómo se asignan los tipos entre los objetos de base de datos de origen y de destino.  
  
Puede tener acceso a este cuadro de diálogo en varios lugares:  
  
-   Al seleccionar una base de datos de origen o un objeto de base de datos, la pestaña **asignación de tipos** aparece a la derecha del explorador de metadatos. Haga clic en **Agregar** para agregar una nueva asignación de tipos o haga clic en **Editar** para cambiar una asignación de tipos existente.  
  
-   En el menú **herramientas** , seleccione **configuración del proyecto** o **configuración predeterminada del proyecto**. En el cuadro de diálogo resultante, seleccione **asignación de tipos**. Haga clic en **Agregar** para agregar una nueva asignación de tipos o haga clic en **Editar** para cambiar una asignación de tipos existente.  
  
Las asignaciones de tipos específicas de tabla invalidan las asignaciones de tipo de proyecto y base de datos. Las asignaciones específicas de la base de datos invalidan las asignaciones del proyecto.  
  
## <a name="options"></a>Opciones  
**Tipo de origen**  
Seleccione el tipo de datos de origen que se va a asignar a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos.  
  
Si el tipo de datos es de longitud variable, los campos siguientes aparecerán en **tipo de origen**:  
  
**From**  
Especifique la longitud mínima de esta asignación. Por ejemplo, para el tipo de datos **nchar** , puede especificar 10 para especificar que esta asignación es para un intervalo que empieza en **NCHAR (10)**.  
  
**To**  
Especifique la longitud máxima de esta asignación. Por ejemplo, para el tipo de datos **nchar** , puede especificar 20 para especificar que esta asignación es para un intervalo que termina en **NCHAR (20)**.  
  
**Tipo de destino**  
Seleccione el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos al que se asigna el tipo de datos de origen. Cuando SSMA crea la tabla o el procedimiento almacenado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el tipo de datos de origen cambiará a este tipo de datos.  
  
Si el tipo de datos es de longitud variable, el siguiente campo aparecerá en **tipo de destino**:  
  
**Replace with**  
Especifique la longitud de destino de esta asignación. Por ejemplo, para el tipo de datos **nvarchar** , puede especificar 20 para especificar que el tipo de datos de origen especificado debe estar asignado a **nvarchar (20)**.  
  
