---
title: Ventana Resultados espaciales
description: Aprenda a usar la ventana Resultados espaciales, en la se proporcionan herramientas de asignación visual para ver los datos espaciales. Para ver los resultados espaciales, los resultados de la consulta deben incluir una columna espacial con datos de geometría o de geografía.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c2d5a477-6496-4d01-adee-7322ebdfadf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b08fe5a2b40f07a6ffef93862512e5132dfbb6ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344157"
---
# <a name="spatial-results-window"></a>Ventana Resultados espaciales
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La ventana **Resultados espaciales** proporciona herramientas de asignación visual para ver los datos espaciales. Para ver los resultados espaciales, los resultados de la consulta deben incluir una columna espacial con datos de geometría o de geografía.  
  
> [!NOTE]  
>  La ventana **Resultados espaciales** solo está disponible si los resultados se devuelven a una cuadrícula en la ventana **Resultados** . Si especifica que los resultados se deben devolver como texto, esta ventana no está disponible.  
  
## <a name="options"></a>Opciones  
 **Seleccionar columna espacial**  
 Especifique la columna espacial que desea ver en las columnas espaciales de los resultados de la consulta. Solo puede seleccionarse una columna cada vez.  
  
 **Seleccionar columna de etiqueta**  
 Especifique la columna no espacial en las columnas devueltas en los resultados de la consulta para etiquetar los datos espaciales. Solo puede seleccionarse una columna cada vez.  
  
 Esta opción no está disponible si solo se devuelven instancias de punto en una consulta.  
  
 **Seleccionar proyección**  
 Muestre los datos geográficos en una de las cuatro proyecciones: Equirectangular, Mercator, Robinson o Bonne.  
  
 Esta opción no está disponible para los datos de geometría.  
  
 **Zoom**  
 Ajuste la presentación del mapa en una escala exponencial.  
  
 **Mostrar líneas de cuadrícula**  
 Activa o desactiva las líneas de cuadrícula de coordenadas.  
  
 En las formas de polígono, la etiqueta solo se muestra cuando la forma es lo suficientemente grande como para contener el texto del rótulo. Si desea mostrar las etiquetas para las formas pequeñas, ajuste el zoom.  
  
> [!NOTE]  
>  Las instancias de punto no se pueden etiquetar.  
  
## <a name="see-also"></a>Consulte también  
 [Ver datos espaciales en el Explorador de objetos](./view-spatial-data-in-object-explorer.md)   
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [Editor de consultas del motor de base de datos &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)   
 [Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)  
  
