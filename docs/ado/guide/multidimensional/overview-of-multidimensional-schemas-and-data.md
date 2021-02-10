---
description: Información general de esquemas y datos multidimensionales
title: Información general de los esquemas y datos multidimensionales | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional schemas and data
ms.assetid: ce37fa06-c581-4d80-9a9b-c3aa66408909
author: rothja
ms.author: jroth
ms.openlocfilehash: 2dcd54dfb8ba5797588c615f987830dcb8aa5736
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032187"
---
# <a name="overview-of-multidimensional-schemas-and-data"></a>Información general de esquemas y datos multidimensionales
## <a name="understanding-multidimensional-schemas"></a>Descripción de los esquemas multidimensionales  
 El objeto de metadatos central de ADO MD es el *cubo*, que consta de un conjunto estructurado de dimensiones, jerarquías, niveles y miembros relacionados.  
  
 Una *dimensión* es una categoría independiente de datos de la base de datos multidimensional, derivada de las entidades empresariales. Una dimensión normalmente contiene elementos que se usarán como criterios de consulta para las medidas de la base de datos.  
  
 Una *jerarquía* es una ruta de agregación de una dimensión. Una dimensión puede tener varios niveles de granularidad, que tienen relaciones de elementos primarios y secundarios. Una jerarquía define cómo se relacionan estos niveles.  
  
 Un *nivel* es un paso de agregación en una jerarquía. En el caso de las dimensiones con varios niveles de información, cada capa es un nivel.  
  
 Un *miembro* es un elemento de datos de una dimensión. Normalmente, se crea un título o se describe una medida de la base de datos mediante miembros.  
  
 Los cubos se representan mediante objetos [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) en ADO MD. Las dimensiones, las jerarquías, los niveles y los miembros también se representan mediante sus objetos de ADO MD correspondientes: [dimensión](../../reference/ado-md-api/dimension-object-ado-md.md), [jerarquía](../../reference/ado-md-api/hierarchy-object-ado-md.md), [nivel](../../reference/ado-md-api/level-object-ado-md.md)y [miembro](../../reference/ado-md-api/member-object-ado-md.md).  
  
### <a name="dimensions"></a>Dimensions  
 Las dimensiones de un cubo dependen de las entidades empresariales y los tipos de datos que se van a modelar en la base de datos. Normalmente, cada dimensión es un punto de entrada independiente o un mecanismo para seleccionar los datos.  
  
 Por ejemplo, un cubo que contiene datos de ventas tiene las cinco dimensiones siguientes: vendedor, geografía, hora, productos y medidas. La dimensión Measures contiene valores de datos de ventas reales, mientras que las demás dimensiones representan maneras de categorizar y agrupar los valores de los datos de ventas.  
  
 La dimensión Geography tiene el siguiente conjunto de miembros:  
  
```console
{All, North America, Europe, Canada, USA, UK, Germany, Canada-West,  
Canada-East, USA-NW, USA-SW, USA-NE, USA-SE, England, Scotland,   
Wales,Ireland, Germany-North, Germany-South, Ottawa, Toronto,   
Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston,   
Shreveport, Miami, Boston, New York, London, Dover, Glasgow,   
Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin,   
Hamburg, Munich, Stuttgart}  
```  
  
### <a name="hierarchies"></a>Jerarquías  
 Las jerarquías definen las formas en las que los niveles de una dimensión se pueden "acumular" o agrupar. Una dimensión puede tener más de una jerarquía. Existe una jerarquía natural en la dimensión Geography:  
  
### <a name="levels"></a>Niveles  
 En la dimensión de geografía de ejemplo que se muestra en la ilustración anterior, cada cuadro representa un nivel en la jerarquía.  
  
 Cada nivel tiene un conjunto de miembros, como se indica a continuación:  
  
-   El mundo `= {All}`  
  
-   Continentes `= {North America, Europe}`  
  
-   Countrie `= {Canada, USA, UK, Germany}`  
  
-   Zonas `= {Canada-East, Canada-West, USA-NE, USA-NW, USA-SE, USA-SW, England, Ireland, Scotland, Wales, Germany-North, Germany-South}`  
  
-   Cities `= {Ottawa, Toronto, Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston, Shreveport, Miami, Boston, New York, London, Dover, Glasgow, Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin, Hamburg, Munich, Stuttgart}`  
  
### <a name="members"></a>Miembros  
 Los miembros del nivel hoja de una jerarquía no tienen elementos secundarios y los miembros del nivel raíz no tienen ningún elemento primario. Todos los demás miembros tienen al menos un elemento primario y al menos un elemento secundario. Por ejemplo, un recorrido parcial del árbol de jerarquía de la dimensión Geography produce las siguientes relaciones de elementos primarios y secundarios:  
  
-   `{All} (parent of) {Europe, North America}`  
  
-   `{North America} (parent of) {Canada, USA}`  
  
-   `{USA} (parent of) {USA-NE, USA-NW, USA-SE, USA-SW}`  
  
-   `{USA-NW} (parent of) {Boise, Seattle}`  
  
 Los miembros se pueden consolidar en una o más jerarquías por dimensión. Considere una dimensión de tiempo en la que hay dos maneras de acumular el nivel de año en el nivel de días:  
  
 En este ejemplo también se muestra otra característica: algunos miembros del nivel semana de la jerarquía Year-Week no aparecen en ningún nivel de la jerarquía Year-Quarter. Por lo tanto, una jerarquía no necesita incluir todos los miembros de una dimensión.  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de objetos de ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensional) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Programar con ADO MD](./programming-with-ado-md.md)   
 [Usar ADO con ADO MD](./using-ado-with-ado-md.md)   
 [Trabajo con datos multidimensionales](./working-with-multidimensional-data.md)