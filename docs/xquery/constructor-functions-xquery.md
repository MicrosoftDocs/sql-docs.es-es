---
title: Funciones de constructor (XQuery) | Microsoft Docs
description: Obtenga información sobre las funciones de constructor de XQuery que le permiten crear instancias de los tipos atómicos integrados de XSD o definidos por el usuario.
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
- constructor functions [XQuery]
ms.assetid: 98562d0e-d0e0-4f62-b001-90acbac67277
author: rothja
ms.author: jroth
ms.openlocfilehash: 0cc1fe3f4f36191a04973c759474f55a8356abda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349331"
---
# <a name="constructor-functions-xquery"></a>Funciones de constructor (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Desde una entrada especificada, las funciones de constructor crean instancias de cualquiera de los tipos atómicos integrados o definidos por el usuario.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
TYP($atomicvalue as xdt:anyAtomicType?  
  
) as TYP?  
  
```  
  
## <a name="arguments"></a>Argumentos  
 *$strval*  
 Cadena que se va a convertir.  
  
 *TYP*  
 Cualquier tipo XSD integrado.  
  
## <a name="remarks"></a>Comentarios  
 Se admiten constructores para tipos XSD atómicos base y derivados. Sin embargo, no se admiten los subtipos de **xs: Duration**, que incluye **XDT: yearMonthDuration y XDT: dayTimeDuration**, y **xs: QName**, **xs: NMTOKEN** y **xs: Notation** . Los tipos atómicos definidos por el usuario que están disponibles en las colecciones de esquemas asociadas también están disponibles, siempre que se deriven directa o indirectamente de los tipos siguientes.  
  
#### <a name="supported-base-types"></a>Tipos base compatibles  
 Éstos son los tipos base admitidos:  
  
-   xs:string  
  
-   xs:boolean  
  
-   xs:decimal  
  
-   xs:float  
  
-   xs:double  
  
-   xs:duration  
  
-   xs:dateTime  
  
-   xs:time  
  
-   xs:date  
  
-   xs:gYearMonth  
  
-   xs:gYear  
  
-   xs:gMonthDay  
  
-   xs:gDay  
  
-   xs:gMonth  
  
-   xs:hexBinary  
  
-   xs:base64Binary  
  
-   xs:anyURI  
  
#### <a name="supported-derived-types"></a>Tipos derivados compatibles  
 Éstos son los tipos derivados admitidos:  
  
-   xs:normalizedString  
  
-   xs:token  
  
-   xs:language  
  
-   xs:Name  
  
-   xs:NCName  
  
-   xs:ID  
  
-   xs:IDREF  
  
-   xs:ENTITY  
  
-   xs:integer  
  
-   xs:nonPositiveInteger  
  
-   xs:negativeInteger  
  
-   xs:long  
  
-   xs:int  
  
-   xs:short  
  
-   xs:byte  
  
-   xs:nonNegativeInteger  
  
-   xs:unsignedLong  
  
-   xs:unsignedInt  
  
-   xs:unsignedShort  
  
-   xs:unsignedByte  
  
-   xs:positiveInteger  
  
 SQL Server también admite el doblado constante para invocaciones de funciones de construcción de las formas siguientes:  
  
-   Si el argumento es un literal de cadena, la expresión se evaluará durante la compilación. Cuando el valor no satisfaga las restricciones de tipo, se producirá un error estático.  
  
-   Si el argumento es un literal de otro tipo, la expresión se evaluará durante la compilación. Cuando el valor no satisfaga las restricciones de tipo, se devolverá la secuencia vacía.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-datetime-xquery-function-to-retrieve-older-product-descriptions"></a>A. Usar la función dateTime() de XQuery para recuperar descripciones antiguas de productos  
 En este ejemplo, primero se asigna un documento XML de ejemplo a una variable de tipo **XML** . Este documento contiene tres <de `ProductDescription`> de ejemplo, cada uno de los cuales contiene un <`DateCreated`> elemento secundario.  
  
 A continuación, se realiza una consulta en la variable para recuperar solo las descripciones de producto que se crearon antes de una fecha específica. En lo que respecta a la comparación, la consulta utiliza la función constructora **xs: DateTime ()** para escribir las fechas.  
  
```  
declare @x xml  
set @x = '<root>  
<ProductDescription ProductID="1" >  
  <DateCreated DateValue="2000-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription  ProductID="2" >  
  <DateCreated DateValue="2001-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
<ProductDescription ProductID="3" >  
  <DateCreated DateValue="2002-01-01T00:00:00Z" />  
  <Summary>Some Summary description</Summary>  
</ProductDescription>  
</root>'  
  
select @x.query('  
     for $PD in  /root/ProductDescription  
     where xs:dateTime(data( ($PD/DateCreated/@DateValue)[1] )) < xs:dateTime("2001-01-01T00:00:00Z")  
     return  
        element Product  
       {   
        ( attribute ProductID { data($PD/@ProductID ) },  
        attribute DateCreated { data( ($PD/DateCreated/@DateValue)[1] ) } )  
        }  
 ')  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La... La estructura de bucle WHERE se utiliza para recuperar el \<ProductDescription> elemento que satisface la condición especificada en la cláusula WHERE.  
  
-   La función constructora **DateTime ()** se usa para crear valores de tipo **DateTime** para que se puedan comparar adecuadamente.  
  
-   Finalmente, la consulta construye el XML resultante. Dado que se pretende construir una secuencia de atributos, en la construcción de XML se utilizan comas y paréntesis.  
  
 El resultado es el siguiente:  
  
```  
<Product   
   ProductID="1"   
   DateCreated="2000-01-01T00:00:00Z"/>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Construcción XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
