---
title: Funciones de descriptor de acceso de datos | Microsoft Docs
description: 'Obtenga información sobre cómo usar las funciones de descriptor de acceso de datos de XQuery FN: Data (), FN: String () y Text ().'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- data-accessor functions [XQuery]
ms.assetid: 31bad04f-7c74-4773-9f83-612704fdd21c
author: rothja
ms.author: jroth
ms.openlocfilehash: 53ce941c88e2ab551ac0787126e2cf981d353819
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351069"
---
# <a name="data-accessor-functions"></a>Funciones del descriptor de acceso a datos
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En los temas expuestos en esta sección se ofrecen descripciones y ejemplos de código para las funciones del descriptor de acceso a datos.  
  
## <a name="understanding-fndata-fnstring-and-text"></a>Información acerca de fn:data (), fn:string () y text()  
 XQuery tiene una función **FN: Data ()** para extraer los valores con tipo escalar de los nodos, un texto de prueba de nodo **()** para devolver los nodos de texto y la función **FN: String ()** que devuelve el valor de cadena de un nodo. Su uso puede resultar confuso. A continuación, se presentan las directrices para usarlas correctamente en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La instancia XML \<age> 12 \</age> se usa con fines ilustrativos.  
  
-   XML sin tipo: la expresión de ruta de acceso /age/text() devuelve el nodo de texto "12". La función fn:data(/age) devuelve el valor de cadena "12", igual que fn:string(/age).  
  
-   XML con tipo: la expresión/Age/Text () devuelve un error estático para cualquier elemento de tipo simple \<age> . Por otro lado, fn:data (/edad) devuelve el entero 12. La función fn:string(/age) produce la cadena "12".  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Función de cadena &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md)  
  
-   [Función de datos &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md)  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones de ruta de acceso &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)  
  
  
