---
title: Comparación y sincronización de datos de tablas con datos de una base de datos de referencia
description: Aprenda a comparar datos de dos bases de datos diferentes. Vea cómo sincronizar los datos y cómo ver el script que se usa para el proceso de sincronización.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 96d743b0-b69a-45bb-ae0e-62103dca76e2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/27/2020
ms.openlocfilehash: 40d09ff78845707729a94359c30d07c73df1bb77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079626"
---
# <a name="compare-and-synchronize-data-in-one-or-more-tables-with-data-in-a-reference-database"></a>Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia

Puede comparar los datos de una base de datos de *origen* y una base de datos de *destino* y especificar qué tablas se deben comparar. La revisión de estos datos puede ayudarle a adoptar una decisión sobre los cambios que se deben sincronizar. A continuación, puede actualizar el destino para sincronizar las bases de datos o exportar el script de actualización al editor de Transact\-SQL o a un archivo.  
  
Por ejemplo, puede sincronizar las bases de datos para actualizar un servidor de almacenamiento provisional con una copia de los datos de producción. También puede sincronizar una o varias tablas para poblarlas con datos de referencia de otra. Además, puede comparar datos antes y después de ejecutar pruebas como forma adicional de comprobación.  
  
Puede comparar datos de dos bases de datos, pero no puede especificar un archivo de proyecto de base de datos o .dacpac para compararlos porque no contiene datos.  
  
Esta sección contiene las siguientes áreas de información:  
  
-   [Cómo: Comparar y sincronizar los datos de dos bases de datos](../ssdt/how-to-compare-and-synchronize-the-data-of-two-databases.md)  
  
-   [Cómo: Consultar las diferencias de los datos](../ssdt/how-to-view-data-differences.md)  
  
## <a name="requirements"></a>Requisitos  
Cuando se comparan datos de una tabla o vista, la tabla o vista en la base de datos de origen debe compartir varios atributos con una tabla o vista en la base de datos de destino. Las tablas y vistas que no cumplen los siguientes criterios no se comparan y no aparecen en la segunda página del asistente **Nueva comparación de datos**:  
  
-   Las tablas deben tener nombres de columnas coincidentes que tengan tipos de datos compatibles.  
  
    Los nombres de tablas, vistas y propietarios distinguen entre mayúsculas y minúsculas.  
  
-   Las tablas deben tener la misma clave principal, índice único o restricción UNIQUE.  
  
-   Las vistas deben tener los mismos índices clúster únicos.  
  
-   Puede comparar una tabla con una vista solo si tienen el mismo nombre.  
  
Cada objeto tiene una clave o un índice que determina los objetos a los que corresponde. Cada tabla o vista puede tener más de una clave principal, de un índice único o de una restricción UNIQUE. Por tanto, puede especificar qué clave, índice o restricción utilizar.  
  
## <a name="common-tasks"></a>Tareas comunes  
En esta sección, puede encontrar descripciones de las tareas comunes que admiten este escenario.  
  
**Establezca las opciones para controlar cómo se comparan los datos**: cuando se comparan datos, puede omitir con seguridad columnas de identidad, deshabilitar desencadenadores y deshabilitar claves externas. También puede quitar claves principales, índices y restricciones UNIQUE del script de actualización.  
  
**Compare los datos de las tablas y actualice opcionalmente el destino para que coincida con el origen:** después de especificar las bases de datos de origen y destino y ejecutar la comparación, vea los resultados en la ventana **Comparación de datos**. Observe no solo los detalles de las diferencias sino también el script de actualización que usa para sincronizar los datos. Después de identificar diferencias entre las dos bases de datos, especifique una acción para cada diferencia. A continuación, actualice el destino o exporte el script de actualización al editor de Transact\-SQL o a un archivo. Puede que desee exportar el script para que usted u otra persona pueda revisarlo antes de aplicar los cambios.  
  
## <a name="understanding-comparison-results"></a><a name="UnderstandingDataCompareResults"></a>Reconocimiento de los resultados de la comparación  
La tabla siguiente describe las cinco columnas de la ventana **Comparación de datos**.  
  
|Columna|Notas|  
|----------|---------|  
|Object|Muestra el nombre de la tabla o vista y una casilla que indica si el destino se debe sincronizar al escribir actualizaciones o exportar el script de actualización. La casilla no está disponible para tablas o vistas que no contienen datos.|  
|Varios registros|Muestra el número de registros en el destino con la misma clave pero no los mismos datos que en el origen. Los paréntesis incluyen el número de registros que se marcan para actualizar cuando se escriben actualizaciones o se exporta el script de actualización.|  
|Solo en el origen|Muestra el número de registros en el origen que no aparecen en el destino. Los paréntesis incluyen el número de registros que se marcan para agregarlos cuando se escriben actualizaciones o se exporta el script de actualización.|  
|Sólo en destino|Muestra el número de registros en el destino que no aparecen en el origen. Los paréntesis incluyen el número de los registros que se marcan para su eliminación cuando se escriben actualizaciones o se exporta el script de actualización.|  
|Registros idénticos|Muestra el número de registros en el destino con la misma clave y los mismos datos que en el origen. Estos registros no se actualizan al escribir actualizaciones o exportar el script de actualización.|  
  
### <a name="table-and-view-details"></a>Detalles de las vistas y tablas  
Al hacer clic en cualquier tabla o vista en la ventana **Comparación de datos**, el panel de detalles muestra todas las filas que la tabla o vista contiene. Cada pestaña en el panel de detalles muestra una categoría distinta (Diferentes registros, Solo en origen, Solo en destino, Registros idénticos). Para cada fila, puede activar o desactivar la casilla correspondiente para indicar si desea incluir ese cambio en el script de actualización.  
  
## <a name="see-also"></a>Consulte también  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
[Cómo: Usar Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
