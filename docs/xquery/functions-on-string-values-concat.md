---
title: concat (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la función XQuery concat () que devuelve una cadena creada mediante la concatenación de cero o más cadenas especificadas como argumentos.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 3ad92315e20ae314fb9a22ddea1241f379ecc90f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341927"
---
# <a name="functions-on-string-values---concat"></a>Funciones usadas en valores de cadena: concat
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Acepta cero o más cadenas como argumentos y devuelve una cadena creada mediante la concatenación de los valores de cada uno de estos argumentos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>Argumentos  
 *$string*  
 Cadena opcional que se concatenará.  
  
## <a name="remarks"></a>Observaciones  
 La función requiere al menos dos argumentos. Si un argumento es una secuencia vacía, se tratará como una cadena de longitud cero.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 El comportamiento de pares suplentes en las funciones XQuery depende del nivel de compatibilidad de la base de datos y, en algunos casos, del URI del espacio de nombres predeterminado de las funciones. Para obtener más información, vea la sección "las funciones XQuery son compatibles con suplentes" en el tema [cambios importantes en las características de motor de base de datos en SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vea también el [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) y la [Intercalación y compatibilidad con Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** en la base de datos de ejemplo AdventureWorks.  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>A. Usar la función de XQuery concat() para concatenar cadenas  
 Para un modelo de producto determinado, esta consulta devuelve una cadena creada mediante la concatenación del período de garantía y la descripción de la misma. En el documento de Descripción del catálogo, el `Warranty` elemento <> se compone de <`WarrantyPeriod`> y <`Description` los elementos secundarios.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   En la cláusula SELECT, CatalogDescription es una columna de tipo **XML** . Por lo tanto, se usa el [método Query () (tipo de datos XML)](../t-sql/xml/query-method-xml-data-type.md), instructions. Query (). La instrucción de XQuery se especifica como el argumento para el método de consulta.  
  
-   El documento en el que se ejecuta la consulta utiliza espacios de nombres. Por lo tanto, la palabra clave **namespace** se utiliza para definir el prefijo del espacio de nombres. Para obtener más información, vea el [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md).  
  
 El resultado es el siguiente:  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 La consulta anterior recupera información para un producto determinado. La consulta siguiente recupera la misma información para todos los productos para los que se almacenan descripciones del catálogo XML. El método **exist ()** del tipo de datos **XML** de la cláusula WHERE devuelve true si el documento XML de las filas tiene un `ProductDescription` elemento <>.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 Tenga en cuenta que el valor booleano devuelto por el método **exist ()** del tipo **XML** se compara con 1.  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **concat ()** de SQL Server solo acepta valores de tipo XS: String. Los demás valores se deben convertir explícitamente a xs:string o xdt:untypedAtomic.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
