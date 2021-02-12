---
description: Informe de evaluación (AccessToSQL)
title: Informe de evaluación (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment Report dialog box
- Conversion Report dialog box
ms.assetid: ba6f53aa-0049-4c49-8bb8-607a8bfaa737
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7c7e8b8e282a59144628f70e0b2efc06a1f4b59f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076396"
---
# <a name="assessment-report-accesstosql"></a>Informe de evaluación (AccessToSQL)
La ventana Informe de evaluación muestra los resultados de la conversión de objetos de base de datos a [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis y también puede ayudarle a calcular la complejidad y el costo de los proyectos de migración.  
  
Para crear un informe de evaluación, seleccione los objetos que desea convertir en el explorador de metadatos de origen, haga clic con el botón derecho en **bases** de datos y, a continuación, seleccione **crear informe**. También puede mostrar este informe automáticamente después de convertir los esquemas. Sin embargo, el nombre del informe será informe de conversión. Para obtener más información, vea [configuración del proyecto (GUI) (SSMA Common)](../sybase/project-settings-gui-sybasetosql.md).  
  
## <a name="options"></a>Opciones  
**Panel Explorador**  
Contiene una jerarquía de objetos en el informe de evaluación. Expanda carpetas para ver objetos y subcomponentes individuales. Al hacer clic en una categoría o un objeto, las estadísticas de conversión de esa categoría u objeto aparecen en el panel de detalles.  
  
**Panel de detalles**  
Muestra estadísticas de conversión o mensajes de error y advertencia para el objeto seleccionado. Por ejemplo, si se selecciona la carpeta tablas, el panel detalles mostrará los números de claves externas, índices, claves principales y tablas que se han convertido.  
  
**Panel Mensajes**  
Muestra los errores, las advertencias y los mensajes de información que se generaron cuando se creó el informe de evaluación. Los mensajes se agrupan por número.  
  
Para ver los detalles del mensaje, haga clic en **errores**, **advertencias** o **mensajes** y, a continuación, expanda un mensaje. SSMA mostrará la lista de objetos que tienen este error. Haga clic en un objeto para mostrar todos los detalles de conversión del objeto.  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario (acceso)](./user-interface-reference-accesstosql.md)  
