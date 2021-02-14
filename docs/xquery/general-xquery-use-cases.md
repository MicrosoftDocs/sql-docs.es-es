---
title: Casos de uso generales de XQuery | Microsoft Docs
description: Ver varios ejemplos de uso general de XQuery, como buscar, recuperar y enumerar datos específicos de un catálogo de productos.
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
- XQuery, general usage cases
ms.assetid: 5187c97b-6866-474d-8bdb-a082634039cc
author: rothja
ms.author: jroth
ms.openlocfilehash: 62fa5c7bba9133c9a8b0098142d97fdd422c47ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341916"
---
# <a name="general-xquery-use-cases"></a>Casos de uso generales de XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En este tema se proporcionan ejemplos generales del uso de XQuery.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-query-catalog-descriptions-to-find-products-and-weights"></a>A. Consultar descripciones del catálogo para encontrar productos y pesos  
 La siguiente consulta devuelve los Id. de modelo de producto y los pesos, si existen, de la descripción del catálogo de productos. La consulta crea XML con el formato siguiente:  
  
```  
<Product ProductModelID="...">  
  <Weight>...</Weight>  
</Product>  
```  
  
 Esta es la consulta:  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  <Product  ProductModelID="{ (/p1:ProductDescription/@ProductModelID)[1] }">  
     {   
       /p1:ProductDescription/p1:Specifications/Weight   
     }   
  </Product>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La palabra clave **namespace** del prólogo de XQuery define un prefijo de espacio de nombres que se utiliza en el cuerpo de la consulta.  
  
-   El cuerpo de la consulta genera el XML requerido.  
  
-   En la cláusula WHERE, el método **exist ()** se usa para buscar solo las filas que contienen descripciones del catálogo de productos. Es decir, el XML que contiene el `ProductDescription` elemento <>.  
  
 El resultado es el siguiente:  
  
```  
<Product ProductModelID="19"/>  
<Product ProductModelID="23"/>   
<Product ProductModelID="25"/>   
<Product ProductModelID="28"><Weight>Varies with size.</Weight></Product>  
<Product ProductModelID="34"/>  
<Product ProductModelID="35"/>  
```  
  
 La consulta siguiente recupera la misma información, pero solo para los modelos de producto cuya descripción de catálogo incluye el peso, el elemento <`Weight`>, en las especificaciones, el `Specifications` elemento de> <. En este ejemplo se utiliza WITH XMLNAMESPACES para declarar el prefijo pd y su enlace de espacio de nombres. De esta manera, el enlace no se describe en el método **query ()** y en el método **exist ()** .  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
          <Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
                 {   
                      /pd:ProductDescription/pd:Specifications/Weight   
                 }   
          </Product>  
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Specifications//Weight ') = 1  
```  
  
 En la consulta anterior, el método **exist ()** del tipo de datos **XML** de la cláusula WHERE comprueba si hay un `Weight` elemento <> en el elemento de> <`Specifications` .  
  
### <a name="b-find-product-model-ids-for-product-models-whose-catalog-descriptions-include-front-angle-and-small-size-pictures"></a>B. Encontrar identificadores de modelos de productos para modelos de productos en cuyas descripciones de catálogo se incluyan imágenes de pequeño tamaño y ángulo frontal  
 La descripción del catálogo de productos XML incluye las imágenes del producto, el `Picture` elemento <>. Cada imagen tiene varias propiedades. Entre ellos se incluyen el ángulo de la imagen, el `Angle` elemento de> <y el tamaño, el `Size` elemento de> <.  
  
 Para los modelos de productos en cuyas descripciones de catálogo se incluyen imágenes de pequeño tamaño y ángulo frontal, la consulta genera XML con el siguiente formato:  
  
```  
< Product ProductModelID="...">  
  <Picture>  
    <Angle>front</Angle>  
    <Size>small</Size>  
  </Picture>  
</Product>  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT CatalogDescription.query('  
   <pd:Product  ProductModelID="{ (/pd:ProductDescription/@ProductModelID)[1] }">  
      <Picture>  
         {  /pd:ProductDescription/pd:Picture/pd:Angle }   
         {  /pd:ProductDescription/pd:Picture/pd:Size }   
      </Picture>  
   </pd:Product>  
') as Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('/pd:ProductDescription/pd:Picture') = 1  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Angle)[1]', 'varchar(20)')  = 'front'  
AND   CatalogDescription.value('(/pd:ProductDescription/pd:Picture/pd:Size)[1]', 'varchar(20)')  = 'small'  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   En la cláusula WHERE, el método **exist ()** se utiliza para recuperar únicamente las filas que tienen descripciones del catálogo de productos con el `Picture` elemento <>.  
  
-   La cláusula WHERE utiliza el método **Value ()** dos veces para comparar los valores de los `Size` elementos <> y <`Angle`>.  
  
 Éste es un resultado parcial:  
  
```  
<p1:Product   
  xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
  ProductModelID="19">  
  <Picture>  
    <p1:Angle>front</p1:Angle>  
    <p1:Size>small</p1:Size>  
  </Picture>  
</p1:Product>  
...  
```  
  
### <a name="c-create-a-flat-list-of-the-product-model-name-and-feature-pairs-with-each-pair-enclosed-in-the-features-element"></a>C. Crear una lista plana del nombre del modelo de producto y los pares de características, con cada par incluido en el \<Features> elemento  
 En la descripción de catálogo del modelo de producto, el XML incluye varias características del producto. Todas estas características se incluyen en el `Features` elemento <>. La consulta utiliza la [construcción XML (XQuery)](../xquery/xml-construction-xquery.md) para construir el XML necesario. La expresión incluida entre llaves se reemplaza por el resultado.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  for $pd in /p1:ProductDescription,  
   $f in $pd/p1:Features/*  
  return  
   <Feature>  
     <ProductModelName> { data($pd/@ProductModelName) } </ProductModelName>  
     { $f }  
  </Feature>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   $pd/P1: Features/* devuelve solo los nodos de elemento secundarios de <`Features`>, pero $PD/P1: Features/node () devuelve todos los nodos. donde se incluyen los nodos de elemento, nodos de texto, instrucciones de procesamiento y comentarios.  
  
-   Los dos bucles FOR generan un producto cartesiano a partir del que se devuelven el nombre de producto y la característica individual.  
  
-   **ProductName** es un atributo. La construcción XML de esta consulta lo devuelve como un elemento.  
  
 Éste es un resultado parcial:  
  
```  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p1:Warranty   
   xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
 </p1:Warranty>  
</Feature>  
<Feature>  
 <ProductModelName>Mountain 100</ProductModelName>  
 <ProductModelID>19</ProductModelID>  
 <p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer   
           or any AdventureWorks retail store.</p2:Description>  
    </p2:Maintenance>  
</Feature>  
...  
...      
```  
  
### <a name="d-from-the-catalog-description-of-a-product-model-list-the-product-model-name-model-id-and-features-grouped-inside-a-product-element"></a>D. En la descripción del catálogo de un modelo de producto, enumere el nombre del modelo de producto, el ID. de modelo y las características agrupadas dentro de un \<Product> elemento.  
 Mediante la información almacenada en la descripción del catálogo del modelo de producto, la consulta siguiente muestra el nombre del modelo de producto, el identificador del modelo y las características agrupadas dentro de un \<Product> elemento.  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>  
         <ProductModelName>   
           { data(/pd:ProductDescription/@ProductModelName) }   
         </ProductModelName>  
         <ProductModelID>   
           { data(/pd:ProductDescription/@ProductModelID) }   
         </ProductModelID>  
         { /pd:ProductDescription/pd:Features/* }  
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Éste es un resultado parcial:  
  
```  
<Product>  
  <ProductModelName>Mountain 100</ProductModelName>  
  <ProductModelID>19</ProductModelID>  
  <p1:Warranty>... </p1:Warranty>  
  <p2:Maintenance>...  </p2:Maintenance>  
  <p3:wheel xmlns:p3="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p3:wheel>  
  <p4:saddle xmlns:p4="https://www.adventure-works.com/schemas/OtherFeatures">  
    <p5:i xmlns:p5="http://www.w3.org/1999/xhtml">Anatomic design</p5:i> and made from durable leather for a full-day of riding in comfort.</p4:saddle>  
  <p6:pedal xmlns:p6="https://www.adventure-works.com/schemas/OtherFeatures">  
    <p7:b xmlns:p7="http://www.w3.org/1999/xhtml">Top-of-the-line</p7:b> clipless pedals with adjustable tension.</p6:pedal>  
   ...  
```  
  
### <a name="e-retrieve-product-model-feature-descriptions"></a>E. Recuperar descripciones de características de modelos de productos  
 La siguiente consulta crea XML que incluye un <`Product`> elemento que tiene los atributos **ProducModelID**, **ProductModelName** y las dos primeras características del producto. En concreto, las dos primeras características del producto son los dos primeros elementos secundarios del `Features` elemento <>. Si hay más características, devuelve un elemento <> vacío `There-is-more/` .  
  
```  
SELECT CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /pd:ProductDescription/@ProductModelID }  
          { /pd:ProductDescription/@ProductModelName }   
          {  
            for $f in /pd:ProductDescription/pd:Features/*[position()<=2]  
            return  
            $f   
          }  
          {  
            if (count(/pd:ProductDescription/pd:Features/*) > 2)  
            then <there-is-more/>  
            else ()  
          }   
     </Product>          
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La... La estructura de bucles de retorno recupera las dos primeras características del producto. La función **Position ()** se utiliza para buscar la posición de los elementos de la secuencia.  
  
### <a name="f-find-element-names-from-the-product-catalog-description-that-end-with-ons"></a>F. Buscar nombres de elementos que terminen en "ons" en la descripción del catálogo de productos  
 La siguiente consulta busca en las descripciones del catálogo y devuelve todos los elementos del `ProductDescription` elemento <> cuyo nombre termina en "ons".  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in /p1:ProductDescription/*[substring(local-name(.),string-length(local-name(.))-2,3)="ons"]  
      return   
          <Root>  
             { $pd }  
          </Root>  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription is not NULL  
```  
  
 Éste es un resultado parcial:  
  
```  
ProductModelID   Result  
-----------------------------------------  
         19        <Root>         
                     <p1:Specifications xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">          
                          ...         
                     </p1:Specifications>         
                   </Root>          
```  
  
### <a name="g-find-summary-descriptions-that-contain-the-word-aerodynamic"></a>G. Buscar descripciones resumidas que contengan la palabra "Aerodynamic"  
 La siguiente consulta recupera los modelos de productos cuyas descripciones de catálogo incluyen la palabra "Aerodynamic" en la descripción resumida:  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
          <Prod >  
             { /pd:ProductDescription/@ProductModelID }  
             { /pd:ProductDescription/pd:Summary }  
          </Prod>  
 ') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.value('  
     contains( string( (/pd:ProductDescription/pd:Summary)[1] ),"Aerodynamic")','bit') = 1  
```  
  
 Tenga en cuenta que la consulta SELECT especifica métodos **query ()** y **Value ()** del tipo de datos **XML** . Por lo tanto, en lugar de repetir la declaración de espacios de nombres dos veces en dos prólogos de consulta distintos, se utiliza el prefijo pd en la consulta y se define una sola vez mediante WITH XMLNAMESPACES.  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La cláusula WHERE se utiliza para recuperar solo las filas en las que la descripción del catálogo contiene la palabra "aerodinámica" en el `Summary` elemento <>.  
  
-   La función **Contains ()** se usa para ver si la palabra está incluida en el texto.  
  
-   El método **Value ()** del tipo de datos **XML** compara el valor devuelto por **Contains ()** con 1.  
  
 El resultado es el siguiente:  
  
```  
ProductModelID Result        
-------------- ------------------------------------------  
28     <Prod ProductModelID="28">  
        <pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
       <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         A TRUE multi-sport bike that offers streamlined riding and a  
         revolutionary design. Aerodynamic design lets you ride with the   
         pros, and the gearing will conquer hilly roads.</p1:p>  
       </pd:Summary>  
      </Prod>    
```  
  
### <a name="h-find-product-models-whose-catalog-descriptions-do-not-include-product-model-pictures"></a>H. Buscar modelos de productos en cuyas descripciones de catálogo no se incluyan imágenes del modelo de producto  
 La consulta siguiente recupera ProductModelIDs para los modelos de producto cuyas descripciones de catálogo no incluyen un `Picture` elemento <>.  
  
```  
SELECT  ProductModelID  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not NULL  
AND     CatalogDescription.exist('declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /p1:ProductDescription/p1:Picture  
') = 0  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Si el método **exist ()** de la cláusula WHERE devuelve false (0), se devuelve el ID. del modelo de producto. De lo contrario, no se devuelve.  
  
-   Dado que todas las descripciones del producto incluyen un `Picture` elemento <>, el conjunto de resultados está vacío en este caso.  
  
## <a name="see-also"></a>Consulte también  
 [Consultas XQuery que implica la jerarquía](../xquery/xqueries-involving-hierarchy.md)   
 [Consultas XQuery con orden](../xquery/xqueries-involving-order.md)   
 [Consultas XQuery controlar datos relacionales](../xquery/xqueries-handling-relational-data.md)   
 [Búsqueda de cadenas en XQuery](../xquery/string-search-in-xquery.md)   
 [Controlar espacios de nombres en XQuery](../xquery/handling-namespaces-in-xquery.md)   
 [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
