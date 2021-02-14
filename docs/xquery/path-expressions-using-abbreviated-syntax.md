---
title: Usar la sintaxis abreviada en una expresión de ruta de acceso | Microsoft Docs
description: Aprenda a usar la sintaxis abreviada en expresiones de ruta de acceso XQuery.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- abbreviated syntax [XQuery]
ms.assetid: f83c2e41-5722-47c3-b5b8-bf0f8cbe05d3
author: rothja
ms.author: jroth
ms.openlocfilehash: 959d5498011984fc1f400567517d3b5b07727fcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339932"
---
# <a name="path-expressions---using-abbreviated-syntax"></a>Expresiones de ruta de acceso: Usar una sintaxis abreviada
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Todos los ejemplos de [comprensión de las expresiones de ruta de acceso en XQuery](../xquery/path-expressions-xquery.md) usan sintaxis no abreviada para las expresiones de ruta de acceso. La sintaxis no abreviada para un paso de eje en una expresión de ruta de acceso incluye el nombre del eje y la prueba de nodo, separados por dos puntos dobles, y seguidos por cero o más calificadores de paso.  
  
 Por ejemplo:  
  
```  
child::ProductDescription[attribute::ProductModelID=19]  
```  
  
 XQuery admite las siguientes abreviaturas para usarlas con expresiones de ruta de acceso:  
  
-   El eje **secundario** es el eje predeterminado. Por lo tanto, se puede omitir el eje **Child::** en un paso de una expresión. Por ejemplo, `/child::ProductDescription/child::Summary` puede escribirse como `/ProductDescription/Summary`.  
  
-   Un eje de **atributos** se puede abreviar como @ . Por ejemplo, `/child::ProductDescription[attribute::ProductModelID=10]` puede escribirse como `/ProudctDescription[@ProductModelID=10]`.  
  
-   Un **/descendant-or-self:: node ()/** se puede abreviar como//. Por ejemplo, `/descendant-or-self::node()/child::act:telephoneNumber` puede escribirse como `//act:telephoneNumber`.  
  
     La consulta anterior recupera todos los números de teléfono almacenados en la columna AdditionalContactInfo de la tabla Contact. El esquema de AdditionalContactInfo se define de forma que un \<telephoneNumber> elemento puede aparecer en cualquier parte del documento. Por tanto, para recuperar todos los números de teléfono, debe buscar cada nodo del documento. Esta búsqueda se inicia en la raíz del documento y continúa en todos los nodos descendientes.  
  
     La consulta siguiente recupera todos los números de teléfono para un contacto de cliente específico:  
  
    ```  
    SELECT AdditionalContactInfo.query('             
                declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";             
                declare namespace crm="https://schemas.adventure-works.com/Contact/Record";             
                declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";             
                /descendant-or-self::node()/child::act:telephoneNumber             
                ') as result             
    FROM Person.Contact             
    WHERE ContactID=1             
    ```  
  
     Si reemplaza la expresión de ruta de acceso por la sintaxis abreviada, `//act:telephoneNumber`, obtendrá los mismos resultados.  
  
-   **Self:: node ()** en un paso se puede abreviar como un punto único (.). Sin embargo, el punto no es equivalente o intercambiable con **Self:: node ()**.  
  
     Por ejemplo, en la consulta siguiente, el uso de un punto representa un valor y no un nodo:  
  
    ```  
    ("abc", "cde")[. > "b"]  
    ```  
  
-   El **elemento primario:: node ()** en un paso puede abreviarse con un punto doble (..).  
  
  
