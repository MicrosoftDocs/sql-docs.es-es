---
title: Expresiones de ruta de acceso (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo las expresiones de ruta de acceso de XQuery ubican nodos, como nodos de elementos, atributos y texto, en un documento.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- path expressions [XQuery]
- expressions [XQuery], path
ms.assetid: b93fa36c-bf69-46b9-b137-f597d66fd0c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d69a916d19c519e1b313a7244a6a07feba36a67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339991"
---
# <a name="path-expressions-xquery"></a>Expresiones de ruta de acceso (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Estas expresiones buscan nodos, tales como element, attribute y text, en un documento. El resultado de una expresión de ruta de acceso siempre se produce en el orden de los documentos sin nodos duplicados en la secuencia resultante. Al especificar una ruta de acceso, se puede utilizar sintaxis abreviada o sin abreviar. La información siguiente se centra en la sintaxis sin abreviar. La sintaxis abreviada se describe más adelante en este tema.  
  
> [!NOTE]  
>  Dado que las consultas de ejemplo de este tema se especifican en las columnas de tipo **XML** , **CatalogDescription** e **instrucciones**, en la tabla **ProductModel** , debe familiarizarse con el contenido y la estructura de los documentos XML almacenados en estas columnas.  
  
 Una expresión de ruta de acceso puede ser relativa o absoluta. A continuación se ofrece una descripción de ambas:  
  
-   Una expresión de ruta de acceso consta de uno o varios pasos separados por una o dos marcas de barra diagonal (/ o //). Por ejemplo, `child::Features` es una expresión de ruta de acceso relativa, en la que `Child` solo hace referencia a los nodos secundarios del nodo de contexto. Se trata del nodo que se está procesando. La expresión recupera los nodos \<Features> de elemento secundarios del nodo de contexto.  
  
-   Una expresión de ruta de acceso absoluta empieza con una o dos marcas de barra diagonal (/ o //), seguidas de una ruta de acceso relativa opcional. Por ejemplo, la marca de barra diagonal inicial de la expresión, `/child::ProductDescription`, indica que se trata de una expresión de ruta de acceso absoluta. Dado que una marca de barra diagonal al principio de una expresión devuelve el nodo raíz del documento del nodo de contexto, la expresión devuelve todos los \<ProductDescription> nodos de elemento secundarios de la raíz del documento.  
  
     Si una ruta de acceso absoluta empieza con una sola marca de barra diagonal, puede ir seguida o no por una ruta de acceso relativa. Si solo se especifica una sola marca de barra diagonal, la expresión devolverá el nodo raíz del nodo de contexto. En el caso de un tipo de datos XML, se trata del nodo de su documento.  
  
 Una expresión de ruta de acceso se compone de pasos. Por ejemplo, la expresión de ruta de acceso absoluta, `/child::ProductDescription/child::Summary` , contiene dos pasos separados por una barra diagonal.  
  
-   El primer paso recupera los \<ProductDescription> nodos de elemento secundarios de la raíz del documento.  
  
-   En el segundo paso se recuperan los nodos \<Summary> secundarios del nodo de elemento para cada nodo de elemento recuperado \<ProductDescription> , que a su vez se convierte en el nodo de contexto.  
  
 Los pasos de una expresión de ruta de acceso pueden ser de eje o generales.  
  
## <a name="axis-step"></a>Paso de eje  
 Un paso de eje en una expresión de ruta de acceso está formado por las partes que se indican a continuación.  
  
 [eje](../xquery/path-expressions-specifying-axis.md)  
 Define la dirección del movimiento. Se trata de un paso de eje de una expresión de ruta de acceso que empieza en el nodo de contexto y se navega hasta los nodos disponibles en la dirección especificada por el eje.  
  
 [prueba de nodo](../xquery/path-expressions-specifying-node-test.md)  
 Especifica el tipo de nodo o los nombres de nodo que se seleccionarán.  
  
 Cero o más predicados opcionales  
 Filtra los nodos seleccionando algunos y descartando otros.  
  
 En los ejemplos siguientes se usa un **axisstep** en las expresiones de ruta de acceso:  
  
-   La expresión de ruta de acceso absoluta, `/child::ProductDescription`, incluye un solo paso. Especifica un eje (`child`) y una prueba de nodo (`ProductDescription`).  
  
-   La expresión de ruta de acceso relativa, `child::ProductDescription/child::Features`, incluye dos pasos separados por una marca de barra diagonal. Ambos pasos especifican un eje secundario. ProductDescription y Features son pruebas de nodos.  
  
-   La expresión de ruta de acceso relativa, `child::root/child::Location[attribute::LocationID=10]` , contiene dos pasos separados por una barra diagonal. El primero especifica un eje (`child`) y una prueba de nodo (`root`). El segundo especifica los tres componentes de un paso de eje: un eje (secundario), una prueba de nodo (`Location`) y un predicado (`[attribute::LocationID=10]`).  
  
 Para obtener más información sobre los componentes de un paso de eje, vea [especificar ejes en un paso de expresión de ruta](../xquery/path-expressions-specifying-axis.md)de acceso, [especificar la prueba de nodo en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-node-test.md)y [Especificar predicados en un paso de expresión de ruta](../xquery/path-expressions-specifying-predicates.md)de acceso.  
  
## <a name="general-step"></a>Paso general  
 Un paso general es simplemente una expresión que debe dar como resultado una secuencia de nodos.  
  
 La implementación de XQuery en SQL Server admite un paso general como primer paso de una expresión de ruta de acceso. A continuación se muestran ejemplos de expresiones de ruta de acceso que utilizan pasos generales.  
  
```  
(/a, /b)/c  
id(/a/b)  
```  
  
 Para obtener más información sobre la función ID, vea la [función id &#40;XQuery&#41;](../xquery/functions-on-sequences-id.md).  
  
## <a name="in-this-section"></a>En esta sección  
 [Especificar ejes en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-axis.md)  
 Se describe el funcionamiento con el paso de eje en una expresión de ruta de acceso.  
  
 [Especificar una prueba de nodo en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-node-test.md)  
 Se describe el funcionamiento con pruebas de nodo en una expresión de ruta de acceso.  
  
 [Especificar predicados en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-predicates.md)  
 Se describe el funcionamiento con predicados en una expresión de ruta de acceso.  
  
 [Usar sintaxis abreviada en una expresión de ruta de acceso](../xquery/path-expressions-using-abbreviated-syntax.md)  
 Se describe el funcionamiento con sintaxis abreviada en una expresión de ruta de acceso.  
  
  
