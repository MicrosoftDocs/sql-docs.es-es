---
description: WITH XMLNAMESPACES
title: WITH XMLNAMESPACES (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH_XMLNAMESPACES_TSQL
- WITH XMLNAMESPACES
dev_langs:
- TSQL
helpviewer_keywords:
- adding XML namespaces
- XML namespace declarations [SQL Server]
- clauses [SQL Server], WITH XMLNAMESPACES
- WITH XMLNAMESPACES clause
- declaring XML namespaces
ms.assetid: 3b32662b-566f-454d-b7ca-e247002a9a0b
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6ec08509389c8b5aadd080245253e289cc23100c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181741"
---
# <a name="with-xmlnamespaces"></a>WITH XMLNAMESPACES
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Declara uno o varios espacios de nombres XML.  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
WITH XMLNAMESPACES ( <XML namespace declaration item>  
[ { , <XML namespace declaration item> }...] )   
  
<XML namespace declaration item> ::=  
<xml_namespace_uri> AS <xml_namespace_prefix>  
| <XML default namespace declaration item>  
<xml_namespace_uri> ::= <character string literal>  
```  
  
```syntaxsql
  
<xml_namespace_prefix> ::= <identifier>  
```  
  
```syntaxsql
  
<XML default namespace declaration item> ::=  
DEFAULT <xml_namespace_uri>  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *xml_namespace_uri*  
 Identificador uniforme de recursos (URI) que identifica el espacio de nombres XML que se va a declarar. *xml_namespace_uri* es una cadena de SQL.  
  
 *xml_namespace_prefix*  
 Especifica un prefijo que se va a asignar y asociar al valor URI del espacio de nombres especificado en *xml_namespace_uri*. *xml_namespace_prefix* debe ser un identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Observaciones  
 Al utilizar la cláusula WITH XMLNAMESPACES en una instrucción que también incluye una expresión de tabla común, la cláusula WITH XMLNAMESPACES debe preceder a la expresión de tabla común en la instrucción.  
  
 Estas son las reglas generales de sintaxis que se aplican cuando se usa la cláusula WITH XMLNAMESPACES:  
  
-   Cada declaración de espacio de nombres XML debe contener al menos un elemento de declaración de espacio de nombres XML predeterminado.  
  
-   Cada prefijo de espacio de nombres XML utilizado debe ser un nombre no colonizado (NCName) y que el carácter de dos puntos (:) no forme parte del nombre.  
  
-   No se puede definir un prefijo de espacio de nombres dos veces.  
  
-   Los prefijos del espacio de nombres XML y los URI distinguen entre mayúsculas y minúsculas.  
  
-   No se puede declarar el prefijo del espacio de nombres XML `xmlns` .  
  
-   No se puede reemplazar el prefijo del espacio de nombres `xml` con un espacio de nombres distinto del URI de los espacios de nombres `'http://www.w3.org/XML/1998/namespace'` y este URI no se puede asignar a diferentes prefijos.  
  
-   No se puede volver a declarar el prefijo del espacio de nombres XML `xsi` cuando se va a utilizar la directiva ELEMENTS XSINIL en la consulta.  

-   No es necesario declarar "http://www.w3.org/2001/XMLSchema-instance" para usar el espacio de nombres estándar de xsi. Se agrega de forma implícita mediante el procesador de XML/XPATH si no se especifica y las expresiones de XPath pueden usar el prefijo xsi siempre que se declare correctamente el esquema "http://www.w3.org/2001/XMLSchema-instance" en el documento XML.

-   Los valores de la cadena del URI se codifican según la página de códigos de la intercalación de la base de datos actual y se traducen internamente a Unicode.  
  
-   Se eliminarán los espacios en blanco del URI del espacio de nombres XML siguiendo las reglas de eliminación de espacios en blanco de XSD que se usan para **xs:anyURI**. Además, tenga en cuenta que no se definirán entidades ni se eliminarán entidades en los valores de URI del espacio de nombres XML.  

-   Se comprobará el URI del espacio de nombres XML para buscar caracteres XML 1.0 no válidos y se generará un error si se encuentran (por ejemplo, U+0007).  
  
-   El URI del espacio de nombres XML (después de eliminar los espacios en blanco) no puede ser una cadena de longitud cero o se producirá un error de URI de espacio de nombres vacío no válido.  
  
-   La palabra clave XMLNAMESPACES está reservada en el contexto de la cláusula WITH.  
  
## <a name="examples"></a>Ejemplos  
 Para obtener ejemplos, vea [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
  
