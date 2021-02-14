---
title: Función string-length (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la longitud de cadena de la función XQuery ().
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
- string-length function
- fn:string-length function
ms.assetid: 7cd69c8b-cf2c-478c-b9a3-e0e14e1aa8aa
author: rothja
ms.author: jroth
ms.openlocfilehash: 588e3a4dc19d502da07d23971aea7b987e49a8ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344988"
---
# <a name="functions-on-string-values---string-length"></a>Funciones usadas en valores de cadena: string-length
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve la longitud de la cadena en caracteres.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:string-length() as xs:integer  
fn:string-length($arg as xs:string?) as xs:integer  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Cadena de origen cuya longitud se va a calcular.  
  
## <a name="remarks"></a>Comentarios  
 Si el valor de *$arg* es una secuencia vacía, se devuelve un valor **xs: Integer** de 0.  
  
 El comportamiento de los pares suplentes en funciones XQuery depende del nivel de compatibilidad de la base de datos. Si el nivel de compatibilidad es 110 o superior, cada par suplente se cuenta como un carácter individual. Para los niveles de compatibilidad inferiores, se cuentan como dos caracteres. Para obtener más información, vea [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) e [Intercalación y compatibilidad con Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
 Si el valor contiene un carácter Unicode de 4 bytes representado por dos caracteres suplentes, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contará los caracteres suplentes individualmente.  
  
 La **longitud de cadena ()** sin un parámetro solo se puede usar dentro de un predicado. Por ejemplo, la consulta siguiente devuelve el `ROOT` elemento <>:  
  
```  
DECLARE @x xml;  
SET @x='<ROOT>Hello</ROOT>';  
SELECT @x.query('/ROOT[string-length()=5]');  
```  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 El comportamiento de pares suplentes en las funciones XQuery depende del nivel de compatibilidad de la base de datos y, en algunos casos, del URI del espacio de nombres predeterminado de las funciones. Para obtener más información, vea la sección "las funciones XQuery son compatibles con suplentes" en el tema [cambios importantes en las características de motor de base de datos en SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vea también el [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) y la [Intercalación y compatibilidad con Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-string-length-xquery-function-to-retrieve-products-with-long-summary-descriptions"></a>A. Usar la función string-length() de XQuery para recuperar productos con descripciones resumidas largas  
 En el caso de los productos cuya descripción de Resumen sea superior a 50 caracteres, la consulta siguiente recupera el identificador del producto, la longitud de la descripción del Resumen y el propio resumen, el elemento de> de <`Summary` .  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT CatalogDescription.query('  
      <Prod ProductID= "{ /pd:ProductDescription[1]/@ProductModelID }" >  
       <LongSummary SummaryLength =   
           "{string-length(string( (/pd:ProductDescription/pd:Summary)[1] )) }" >  
           { string( (/pd:ProductDescription/pd:Summary)[1] ) }  
       </LongSummary>  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('string-length( string( (/pd:ProductDescription/pd:Summary)[1]))', 'decimal') > 200;  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La condición de la cláusula WHERE recupera solo las filas donde la descripción resumida almacenada en el documento XML tiene más de 200 caracteres. Utiliza el [método Value () (tipo de datos XML)](../t-sql/xml/value-method-xml-data-type.md).  
  
-   La cláusula SELECT genera solo el XML que desea. Utiliza el [método Query () (tipo de datos XML)](../t-sql/xml/query-method-xml-data-type.md) para construir el XML y especificar la expresión XQuery necesaria para recuperar los datos del documento XML.  
  
 Éste es un resultado parcial:  
  
```  
Result  
-------------------  
<Prod ProductID="19">  
      <LongSummary SummaryLength="214">Our top-of-the-line competition   
             mountain bike. Performance-enhancing options include the  
             innovative HL Frame, super-smooth front suspension, and   
             traction for all terrain.  
      </LongSummary>  
</Prod>  
...  
```  
  
### <a name="b-using-the-string-length-xquery-function-to-retrieve-products-whose-warranty-descriptions-are-short"></a>B. Usar la función de XQuery string-length() para recuperar productos cuya descripción de garantía es corta  
 En el caso de los productos cuyas descripciones de garantía tengan menos de 20 caracteres, la consulta siguiente recupera XML que incluye el identificador del producto, la longitud, la descripción de la garantía y el <`Warranty`> propio elemento.  
  
 Warranty es una de las características del producto. `Warranty`Después del elemento de> <se muestra un elemento secundario de> <opcional `Features` .  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for   $ProdDesc in /pd:ProductDescription,  
            $pf in $ProdDesc/pd:Features/wm:Warranty  
      where string-length( string(($pf/wm:Description)[1]) ) < 20  
      return   
          <Prod >  
             { $ProdDesc/@ProductModelID }  
             <ShortFeature FeatureDescLength =   
                             "{string-length( string(($pf/wm:Description)[1]) ) }" >  
                 { $pf }  
             </ShortFeature>  
          </Prod>  
     ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription')=1;  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   **PD** y **WM** son los prefijos de espacio de nombres utilizados en esta consulta. Identifican los mismos espacios de nombres utilizados en el documento que se va a consultar.  
  
-   La consulta XQuery especifica un bucle FOR anidado. El bucle FOR externo es necesario porque se desean recuperar los atributos **ProductModelID** del `ProductDescription` elemento <>. El bucle FOR interno es necesario, porque solo desea obtener aquellos productos que tengan descripciones de garantía con menos de 20 caracteres.  
  
 Éste es el resultado parcial:  
  
```  
Result  
-------------------------  
<Prod ProductModelID="19">  
  <ShortFeature FeatureDescLength="15">  
    <wm:Warranty   
       xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
      <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
      <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
   </ShortFeature>  
</Prod>  
...  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
