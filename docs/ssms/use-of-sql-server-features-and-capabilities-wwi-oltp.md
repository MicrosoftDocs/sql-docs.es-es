---
description: Arguments for External Tools
title: Arguments for External Tools
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- arguments [SQL Server Management Studio]
- external tools [SQL Server Management Studio]
ms.assetid: 3991c13a-f23f-450b-a2ba-19391c399735
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4cf19970c7c742701248c362806bc823d38cf8a3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88314961"
---
# <a name="arguments-for-external-tools"></a>Arguments for External Tools
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Los argumentos son variables para las que el entorno de Studio proporciona valores cuando se inicia una herramienta desde el menú **Herramientas**. Es posible agregar herramientas externas, como el Bloc de notas, al menú **Herramientas** mediante el cuadro de diálogo **Herramientas externas** .  
  
En la tabla siguiente se enumeran los argumentos de las herramientas externas.  
  
|NOMBRE|Argumento|Descripción|  
|--------|------------|---------------|  
|**Ruta de acceso del elemento**|$(ItemPath)|El nombre completo del archivo de origen actual (definido como unidad + ruta de acceso + nombre de archivo); en blanco si hay una ventana de no origen activa.|  
|**Directorio del elemento**|$(ItemDir)|El directorio del origen actual (definido como unidad + ruta de acceso); en blanco si hay una ventana de no origen activa.|  
|**Nombre de archivo del elemento**|$(ItemFilename)|El nombre del archivo de origen actual (definido como nombre de archivo); en blanco si hay una ventana de no origen activa.|  
|**Extensión del elemento**|$(ItemExt)|La extensión del nombre del archivo de origen actual.|  
|**Línea actual***|$(CurLine)|La posición actual en la línea del cursor en el editor.|  
|**Columna actual***|$(CurCol)|La posición actual en la columna del cursor en el editor.|  
|**Texto actual***|$(CurText)|El texto actual (la palabra en la que se encuentra el cursor o una selección de una única línea, si la hay).|  
|**Ruta de acceso de destino**|$(TargetPath)|El nombre completo del archivo de destino (definido como unidad + ruta de acceso + nombre de archivo).|  
|**Directorio de destino**|$(TargetDir)|El directorio del archivo de destino.|  
|**Nombre de destino**|$(TargetName)|El nombre del archivo de destino.|  
|**Extensión de destino**|$(TargetExt)|La extensión del nombre del archivo de destino.|  
|**Directorio del proyecto**|$(ProjDir)|El directorio del proyecto actual (definido como unidad + ruta de acceso).|  
|**Nombre de archivo del proyecto**|$(ProjFileName)|El nombre de archivo del proyecto actual (definido como unidad + ruta de acceso + nombre de archivo).|  
|**Directorio de la solución**|$(SolutionDir)|El directorio de la solución actual (definido como unidad + ruta de acceso).|  
|**Nombre de archivo de la solución**|$(SolutionFileName)|El nombre de archivo de la solución actual (definido como unidad + ruta de acceso + nombre de archivo).|  
  
*La línea actual, la columna actual o el texto actual se basan en la posición del cursor en el editor de texto tal y como aparece en la barra de estado.  
  
## <a name="see-also"></a>Consulte también  
[Cuadro de diálogo Herramientas externas](../ssms/external-tools-dialog-box.md)  
[Elementos generales de la interfaz de usuario](../ssms/general-user-interface-elements.md)  
  
