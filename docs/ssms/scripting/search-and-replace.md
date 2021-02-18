---
title: Buscar y reemplazar
description: Obtenga información sobre las cuatro maneras diferentes de buscar y reemplazar texto, cada una con su propia versión del cuadro de diálogo Buscar y reemplazar. La configuración de las opciones de Buscar y reemplazar afecta a todas estas formas de búsqueda.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- match case [SQL Server]
- undo operations
- searches [SQL Server Management Studio]
- replacing text
- quick search and replaces [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], undo
- Query Editor [SQL Server Management Studio], searching
- regular expressions [SQL Server Management Studio]
- finding text [SQL Server Management Studio]
- text [SQL Server], search and replace operations
- finding [SQL Server Management Studio]
- locating text
- Query Editor [SQL Server Management Studio], replacing text
- Find and Replace dialog box
- wildcard options [SQL Server Management Studio]
- match whole word [SQL Server]
- searches [SQL Server Management Studio], replacing
ms.assetid: 3641c7b3-3e3e-4ddd-af82-c15b50004f94
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b723be5ceacacb587156ddad40837472ca696af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354573"
---
# <a name="search-and-replace"></a>Buscar y reemplazar
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Existen varias maneras de buscar y reemplazar texto. En el menú **Editar**, **Buscar y reemplazar** ofrece cuatro opciones: **Búsqueda rápida**, **Reemplazo rápido**, **Buscar en archivos** o **Reemplazar en archivos**. Cada una abre versiones del cuadro de diálogo **Buscar y reemplazar** . También es posible buscar sin un cuadro de diálogo mediante los métodos abreviados de teclado para búsqueda incremental. Estas técnicas permiten controlar el ámbito de búsqueda y reemplazo y elegir el método de revisión de las coincidencias de búsqueda y los reemplazos.  
  
 A la hora de buscar y reemplazar texto, es necesario tener en cuenta lo siguiente:  
  
-   Las opciones establecidas en el cuadro de diálogo **Buscar y reemplazar** afectan a todas las búsquedas. Estas opciones incluyen **Coincidir mayúsculas y minúsculas**, **Solo palabras completas**, **Buscar hacia atrás**, **Buscar en texto oculto**, **Usar Caracteres comodín**, **Usar Expresiones regulares**, **Buscar en Todos los documentos abiertos** y **Buscar en Proyecto actual**. Todas las opciones no están disponibles en todas las versiones del cuadro de diálogo **Buscar y reemplazar** .  
  
-   **Deshacer** solo está disponible para aquellos documentos que se dejan abiertos tras una operación de reemplazo.  
  
-   **Deshacer** para una operación **Reemplazar todo** que abarca más de un archivo se considera una acción única masiva en todos los archivos afectados. Es decir, no es posible deshacer el cambio en algunos archivos y no en otros.  
  
 Normalmente no es posible buscar elementos mediante vistas gráficas.  
  
## <a name="see-also"></a>Consulte también  
 [Buscar en un documento activo de forma incremental](./search-an-active-document-incrementally.md)   
 [Buscar documentos de forma interactiva](./search-documents-interactively.md)   
 [Buscar en documentos mediante las listas de resultados](./search-documents-using-results-lists.md)   
 [Buscar texto con caracteres comodín](./search-text-with-wildcards.md)   
 [Buscar texto mediante expresiones regulares](./search-text-with-regular-expressions.md)  
  
