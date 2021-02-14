---
title: Sequence y QNames (XQuery) | Microsoft Docs
description: Obtenga información sobre los conceptos fundamentales de las secuencias y QNames en XQuery.
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
- sequence [XQuery]
- XQuery, sequence
- QName [XQuery]
- predefined namespaces [XML in SQL Server]
ms.assetid: 3593ac26-dd78-4bf0-bb87-64fbcac5f026
author: rothja
ms.author: jroth
ms.openlocfilehash: 10caff2486f8dae470a4778c6da649b7bba5ddba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339927"
---
# <a name="sequence-and-qnames-xquery"></a>Secuencia y QName (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En este tema se describen los siguientes conceptos fundamentales de XQuery:  
  
-   Secuencia  
  
-   QName y espacio de nombres predefinido  
  
## <a name="sequence"></a>Secuencia  
 En XQuery, el resultado de una expresión es una secuencia formada por una lista de nodos e instancias XML de tipos atómicos XSD. Una entrada concreta de una secuencia es un elemento. Un elemento de una secuencia puede ser:  
  
-   Un nodo, como un elemento, atributo, texto, instrucción de procesamiento, comentario o documento  
  
-   Un valor atómico, como una instancia de un tipo XSD simple  
  
 Por ejemplo, la consulta siguiente genera una secuencia de dos nodos de elemento:  
  
```  
SELECT Instructions.query('  
     <step1> Step 1 description goes here</step1>,  
     <step2> Step 2 description goes here </step2>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
  
```  
  
 El resultado es el siguiente:  
  
```  
<step1> Step 1 description goes here </step1>  
<step2> Step 2 description goes here </step2>   
```  
  
 En la consulta anterior, la coma (`,`) al final de la construcción `<step1>` es el constructor de secuencia, y es obligatoria. Los espacios en blanco se agregan al resultado solo a modo de ilustración y se incluyen en los resultados de todos los ejemplos de esta documentación.  
  
 A continuación se ofrece información adicional que debe conocer sobre las secuencias:  
  
-   Si una consulta genera una secuencia que contiene otra secuencia, la secuencia contenida se incluye como una secuencia plana en la secuencia contenedora. Por ejemplo, la secuencia ((1,2, (3,4,5)),6) pasa a ser la secuencia plana (1, 2, 3, 4, 5, 6) en el modelo de datos  
  
    ```  
    DECLARE @x xml;  
    SET @x = '';  
    SELECT @x.query('(1,2, (3,4,5)),6');  
    ```  
  
-   Una secuencia vacía es una secuencia que no contiene ningún elemento. Se representa como "()".  
  
-   Una secuencia con un solo elemento se puede tratar como un valor atómico, y lo mismo a la inversa. Es decir, (1) = 1.  
  
 En esta implementación, la secuencia debe ser homogénea. Dicho de otro modo, será una secuencia de valores atómicos o una secuencia de nodos. Por ejemplo, las siguientes secuencias son válidas:  
  
```  
DECLARE @x xml;  
SET @x = '';  
-- Expression returns a sequence of 1 text node (singleton).  
SELECT @x.query('1');  
-- Expression returns a sequence of 2 text nodes  
SELECT @x.query('"abc", "xyz"');  
-- Expression returns a sequence of one atomic value. data() returns  
-- typed value of the node.  
SELECT @x.query('data(1)');  
-- Expression returns a sequence of one element node.   
-- In the expression XML construction is used to construct an element.  
SELECT @x.query('<x> {1+2} </x>');  
```  
  
 La consulta siguiente devuelve un error, porque no se admiten las secuencias heterogéneas.  
  
```  
SELECT @x.query('<x>11</x>, 22');  
```  
  
## <a name="qname"></a>QName  
 Cada identificador de una expresión XQuery es un elemento QName. Un elemento QName se compone de un prefijo de espacio de nombres y un nombre local. En esta implementación, los nombres de variables de las expresiones XQuery son elementos QName y no pueden tener prefijos.  
  
 Considere el siguiente ejemplo en el que se especifica una consulta en una variable **XML** sin tipo:  
  
```  
DECLARE @x xml;  
SET @x = '<Root><a>111</a></Root>';  
SELECT @x.query('/Root/a');  
```  
  
 En la expresión (`/Root/a`), `Root` y `a` son elementos QName.  
  
 En el ejemplo siguiente, se especifica una consulta en una columna **XML** con tipo. La consulta recorre en iteración todos los \<step> elementos de la primera ubicación centro.  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in /AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 En la expresión de la consulta, tenga en cuenta lo siguiente:  
  
-   `AWMI root`, `AWMI:Location`, `AWMI:step` y `$Step` son elementos QName. `AWMI` es un prefijo, y `root`, `Location` y `Step` son nombres locales.  
  
-   La variable `$step` es un elemento QName y no tiene prefijo.  
  
 En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] existen los siguientes espacios de nombres predefinidos compatibles con XQuery.  
  
|Prefijo|URI|  
|------------|---------|  
|xs|http://www.w3.org/2001/XMLSchema|  
|xsi|http://www.w3.org/2001/XMLSchema-instance|  
|xdt|http://www.w3.org/2004/07/xpath-datatypes|  
|fn|http://www.w3.org/2004/07/xpath-functions|  
|(sin prefijo)|`urn:schemas-microsoft-com:xml-sql`|  
|sqltypes|`https://schemas.microsoft.com/sqlserver/2004/sqltypes`|  
|Xml|`http://www.w3.org/XML/1998/namespace`|  
|(sin prefijo)|`https://schemas.microsoft.com/sqlserver/2004/SOAP`|  
  
 Cada base de datos que se crea tiene la colección de esquemas XML **Sys** . Estos esquemas se reservan para que estén accesibles desde cualquier otra colección de esquemas XML creada por el usuario.  
  
> [!NOTE]  
>  Esta implementación no admite el `local` prefijo tal y como se describe en la especificación de XQuery en http://www.w3.org/2004/07/xquery-local-functions .  
  
## <a name="see-also"></a>Consulte también  
 [Conceptos básicos de XQuery](../xquery/xquery-basics.md)  
  
  
