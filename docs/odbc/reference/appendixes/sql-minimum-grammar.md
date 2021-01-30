---
description: Gramática mínima de SQL
title: Gramática mínima de SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 4f36d785-104f-4fec-93be-f201203bc7c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d611fb753034a7cde370f87700aa8b8c3db2e11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159081"
---
# <a name="sql-minimum-grammar"></a>Gramática mínima de SQL
En esta sección se describe la sintaxis SQL mínima que debe admitir un controlador ODBC. La sintaxis descrita en esta sección es un subconjunto de la sintaxis de nivel de entrada de SQL-92.  
  
 Una aplicación puede utilizar cualquiera de las sintaxis de esta sección y estar seguro de que cualquier controlador compatible con ODBC admitirá esa sintaxis. Para determinar si se admiten características adicionales de SQL-92 que no se incluyen en esta sección, la aplicación debe llamar a **SQLGetInfo** con el tipo de información SQL_SQL_CONFORMANCE. Incluso si el controlador no se ajusta a ningún nivel de conformidad con SQL-92, una aplicación puede seguir usando la sintaxis descrita en esta sección. Si un controlador se ajusta a un nivel de SQL-92, por otro lado, admite toda la sintaxis incluida en ese nivel. Esto incluye la sintaxis de esta sección porque la gramática mínima que se describe aquí es un subconjunto puro del nivel de conformidad más bajo de SQL-92. Una vez que la aplicación conoce el nivel SQL-92 compatible, puede determinar si se admite una característica de nivel superior (si existe) llamando a **SQLGetInfo** con el tipo de información individual correspondiente a esa característica.  
  
 Es posible que los controladores que solo funcionan con orígenes de datos de solo lectura no admitan las partes de la gramática que se incluyen en esta sección y que tratan sobre cómo cambiar los datos. Una aplicación puede determinar si un origen de datos es de solo lectura llamando a **SQLGetInfo** con el tipo de información SQL_DATA_SOURCE_READ_ONLY.  
  
## <a name="statement"></a>.  
 *CREATE-TABLE-Statement* :: =  
  
 CREATE TABLE *nombre de tabla base*  
  
 (*tipo de datos de identificador de columna* [*, tipo de datos de identificador de columna*]...)  
  
> [!IMPORTANT]  
>  Como *tipo de datos* en una *instrucción CREATE-TABLE-Statement*, las aplicaciones deben usar un tipo de datos de la columna TYPE_NAME del conjunto de resultados devuelto por **SQLGetTypeInfo**.  
  
 *Delete-Statement-buscada* :: =  
  
 ELIMINAR de *TABLE-Name* [where *Search-Condition*]  
  
 *DROP-TABLE-Statement* :: =  
  
 DROP TABLE *base-TABLE-Name*  
  
 *Insert-Statement* :: =  
  
 INSERT INTO *TABLE-Name* [( *columna-identificador* [, *columna-identificador*]...)]      VALORES (*Insert-Value*[, *Insert-Value*]...)  
  
 *select-statement* ::=  
  
 SELECT [ALL &#124; DISTINCt] *Select-List*  
  
 DESDE *la lista de referencias de tabla*  
  
 [WHERE *-condición de búsqueda*]  
  
 [*order-by-clause*]  
  
 *instrucción* :: = *CREATE-TABLE-Statement*  
  
 &#124; *Delete-Statement-searched*  
  
 &#124; *DROP-TABLE-Statement*  
  
 &#124; *Insert-Statement*  
  
 &#124; *Select-Statement*  
  
 &#124; *Update-Statement-buscado*  
  
 *Update-Statement-buscada*  
  
 ACTUALIZAR *tabla-nombre*  
  
 ESTABLECER *columna-identificador* = {*expresión* &#124; null}  
  
 [, *identificador de columna* = {*expresión* &#124; null}]...  
  
 [WHERE *-condición de búsqueda*]  
  
 Esta sección contiene los temas siguientes.  
  
-   [Elementos que se usan en instrucciones SQL](../../../odbc/reference/appendixes/elements-used-in-sql-statements.md)  
  
-   [Compatibilidad con tipos de datos](../../../odbc/reference/appendixes/data-type-support.md)  
  
-   [Tipos de datos de parámetro](../../../odbc/reference/appendixes/parameter-data-types.md)  
  
-   [Marcadores de parámetros](../../../odbc/reference/appendixes/parameter-markers.md)
