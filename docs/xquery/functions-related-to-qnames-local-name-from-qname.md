---
title: local-name-from-QName (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función local-name-from-QName () para devolver la parte del nombre local de un QName.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name-from-QName function
- local-name-from-QName function
ms.assetid: fafed718-8c3c-403f-93ee-ec51fc157a6e
author: rothja
ms.author: jroth
ms.openlocfilehash: 97852b8479df926a933e34f5a8dddff7eb240e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345007"
---
# <a name="functions-related-to-qnames---local-name-from-qname"></a>Funciones relacionadas con QNames: local-name-from-QName
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Devuelve un XS: NCNAME que representa la parte local del QName especificado por *$arg*. El resultado es una secuencia vacía si *$arg* es la secuencia vacía.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
fn:local-name-from-QName($arg as xs:QName?) as xs:NCName?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Es el QName del que se debería extraer el nombre local.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos.  
  
 En el ejemplo siguiente se usa la función **local-name-from-QName ()** para recuperar las partes de nombre local y URI de espacio de nombres de un valor de tipo QName. En el ejemplo, se realizan las tareas siguientes:  
  
-   Crear una colección de esquemas XML.  
  
-   Crear una tabla con una columna de tipo xml. El tipo xml se escribe utilizando la colección de esquemas XML.  
  
-   Almacenar una instancia XML de ejemplo en la tabla. Mediante el método **query ()** del tipo de datos XML, la expresión de consulta se ejecuta para recuperar la parte del nombre local del valor de tipo QName de la instancia.  
  
```sql
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<root xmlns="QNameXSD" xmlns:a="https://someURI">a:someLocalName</root>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare default element namespace "QNameXSD"; local-name-from-QName(/root[1])')  
FROM T  
-- Result = someLocalName  
-- You can retrieve namespace URI part from the QName using the namespace-uri-from-QName() function  
SELECT xmlCol.query('declare default element namespace "QNameXSD"; namespace-uri-from-QName(/root[1])')  
FROM T  
-- Result = https://someURI  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones relacionadas con QNames &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)  
  
