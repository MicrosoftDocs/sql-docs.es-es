---
title: Controlar espacios de nombres en XQuery | Microsoft Docs
description: Vea ejemplos de control de espacios de nombres en una expresión XQuery que incluye cómo declarar espacios de nombres nuevos y predeterminados.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- declaring namespaces
- namespaces [XQuery]
- XQuery, namespaces
ms.assetid: 542b63da-4d3d-4ad5-acea-f577730688f1
author: rothja
ms.author: jroth
ms.openlocfilehash: acb1595ca2e7032d0363df3a1dd81740b7bcbf2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341882"
---
# <a name="handling-namespaces-in-xquery"></a>Controlar espacios de nombres en XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En este tema se proporcionan ejemplos para controlar los espacios de nombres en las consultas.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-declaring-a-namespace"></a>A. Declarar un espacio de nombres  
 En la siguiente consulta se recuperan los pasos de fabricación de un modelo de producto específico.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /AWMI:root/AWMI:Location[1]/AWMI:step  
    ') as x  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Éste es el resultado parcial:  
  
```  
<AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>. </AWMI:step>  
...  
```  
  
 Tenga en cuenta que la palabra clave **namespace** se usa para definir un nuevo prefijo de espacio de nombres, "AWMI:". Este prefijo debe utilizarse después en la consulta para todos los elementos que se encuentren dentro del ámbito de dicho espacio de nombres.  
  
### <a name="b-declaring-a-default-namespace"></a>B. Declarar un espacio de nombres predeterminado  
 En la consulta anterior, se definía un nuevo prefijo de espacio de nombres. Ese prefijo tenía que utilizarse después en la consulta para seleccionar las estructuras XML previstas. También puede declarar un espacio de nombres como espacio de nombres predeterminado, tal y como se muestra en la siguiente consulta modificada:  
  
```  
SELECT Instructions.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /root/Location[1]/step  
    ') as x  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente  
  
```  
<step xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <material>aluminum sheet MS-2341</material> into the <tool>T-85A framing tool</tool>. </step>  
...  
```  
  
 Tenga en cuenta que, en este ejemplo, el espacio de nombres definido, `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"`, sirve para reemplazar al espacio de nombres predeterminado o vacío. Por este motivo, ya no hay un prefijo de espacio de nombres en la expresión de ruta de acceso que se utiliza para la consulta. Ya no hay tampoco un prefijo de espacio de nombres en los nombres de elemento que aparecen en los resultados. Además, el espacio de nombres predeterminado se aplica a todos los elementos, pero no a sus atributos.  
  
### <a name="c-using-namespaces-in-xml-construction"></a>C. Usar espacios de nombres en la construcción de XML  
 Cuando se definen nuevos espacios de nombres, se incorporan al ámbito no solo de la consulta, sino también de la construcción. Por ejemplo, al generar XML, se puede definir un nuevo espacio de nombres utilizando la declaración "`declare namespace ...`" y, a continuación, utilizar dicho espacio de nombres con elementos y atributos de generación propia para que aparezcan en los resultados de la consulta.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace myNS="uri:SomeNamespace";<myNS:Result>  
          { /ProductDescription/Summary }  
       </myNS:Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 El resultado es el siguiente:  
  
```  
  
      <myNS:Result xmlns:myNS="uri:SomeNamespace">  
  <Summary xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
     Our top-of-the-line competition mountain bike. Performance-enhancing   
     options include the innovative HL Frame, super-smooth front   
     suspension, and traction for all terrain.</p1:p>  
  </Summary>  
</myNS:Result>  
```  
  
 También se puede definir el espacio de nombres de forma explícita, en todos los puntos donde se utilice como parte de la construcción XML, tal y como se muestra en la siguiente consulta:  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <myNS:Result xmlns:myNS="uri:SomeNamespace">  
          { /ProductDescription/Summary }  
       </myNS:Result>  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
### <a name="d-construction-using-default-namespaces"></a>D. Construcción mediante espacios de nombres predeterminados  
 También puede definir un espacio de nombres predeterminado para utilizarlo en XML generado. Por ejemplo, la consulta siguiente muestra cómo se puede especificar un espacio de nombres predeterminado, "URI: SomeNamespace" \\ , que se usará como valor predeterminado para los elementos con nombre local que se construyen, como el `<Result>` elemento.  
  
```  
SELECT CatalogDescription.query('  
      declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      declare default element namespace "uri:SomeNamespace";<Result>  
          { /PD:ProductDescription/PD:Summary }  
       </Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 El resultado es el siguiente:  
  
```  
  
      <Result xmlns="uri:SomeNamespace">  
  <PD:Summary xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         Our top-of-the-line competition mountain bike. Performance-  
         enhancing options include the innovative HL Frame, super-smooth   
         front suspension, and traction for all terrain.</p1:p>  
  </PD:Summary>  
</Result>  
```  
  
 Observe que al reemplazar el espacio de nombres predeterminado del elemento o vaciarlo, todos los elementos nombrados localmente en el XML generado se enlazan posteriormente con el espacio de nombres predeterminado de reemplazo. Por lo tanto, si requiere flexibilidad a la hora de generar XML para poder beneficiarse de las ventajas de los espacios de nombres vacíos, no debe reemplazar el espacio de nombres predeterminado del elemento.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
