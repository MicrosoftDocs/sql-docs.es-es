---
title: not (función de XQuery) | Microsoft Docs
description: Obtenga información sobre cómo se usa la función XQuery not () con valores booleanos.
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
- effective Boolean value [XQuery]
- fn:not function
- not function [XQuery]
- EBV
ms.assetid: 93dfc377-45f1-4384-9392-560d9331a915
author: rothja
ms.author: jroth
ms.openlocfilehash: f857a8f107b26500bb324584d7d7853e09bbb735
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353304"
---
# <a name="functions-on-boolean-values---not-function"></a>Funciones usadas en valores booleanos: función not 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve TRUE si el valor booleano efectivo de *$arg* es false y devuelve false si el valor booleano efectivo de *$arg* es true.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:not($arg as item()*) as xs:boolean  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de elementos para los que existe un valor booleano efectivo.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-not-xquery-function-to-find-product-models-whose-catalog-descriptions-do-not-include-the-specifications-element"></a>A. Usar la función not () de XQuery para buscar modelos de producto cuyas descripciones de catálogo no incluyen el \<Specifications> elemento.  
 La siguiente consulta crea XML que contiene los identificadores de modelo de producto de los modelos de producto cuyas descripciones de catálogo no incluyen el `Specifications` elemento <>.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
       <Product   
           ProductModelID="{ sql:column("ProductModelID") }"  
        />  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications/*)]  '  
     ) = 0  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Dado que el documento utiliza espacios de nombres, en el ejemplo se utiliza la instrucción WITH NAMESPACES. Otra opción es usar la palabra clave **declare namespace** en el [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md) para definir el prefijo.  
  
-   A continuación, la consulta construye el XML que incluye el `Product` elemento <> y su atributo **ProductModelID** .  
  
-   La cláusula WHERE utiliza el [método exist () (tipo de datos XML)](../t-sql/xml/exist-method-xml-data-type.md) para filtrar las filas. El método **exist ()** devuelve true si hay \<ProductDescription> elementos que no tienen \<Specification> elementos secundarios. Tenga en cuenta el uso de la función **Not ()** .  
  
 Este conjunto de resultados está vacío, porque cada descripción del catálogo del modelo de producto incluye el \<Specifications> elemento.  
  
### <a name="b-using-the-not-xquery-function-to-retrieve-work-center-locations-that-do-not-have-a-machinehours-attribute"></a>B. Usar la función not() de XQuery para recuperar ubicaciones de centros de trabajo que no contienen el atributo MachineHours.  
 La siguiente consulta se especifica para la columna Instructions. En esta columna, se almacenan las instrucciones de fabricación de los modelos de producto.  
  
 Para un modelo de producto determinado, la consulta recupera las ubicaciones de los centros de trabajo para las que no se ha especificado MachineHours. Es decir, el atributo **MachineHours** no se especifica para el \<Location> elemento.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in /AWMI:root/AWMI:Location[not(@MachineHours)]  
     return  
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ $i/@LaborHours }" >  
        </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7   
```  
  
 En la consulta anterior, observe lo siguiente:  
  
-   **Declarenamespace** en el [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md) define el prefijo de espacio de nombres de las instrucciones de fabricación de Adventure Works. Representa el mismo espacio de nombres utilizado en el documento de instrucciones de fabricación.  
  
-   En la consulta, el predicado **Not ( @MachineHours )** devuelve true si no hay ningún atributo **MachineHours** .  
  
 El resultado es el siguiente:  
  
```  
ProductModelID Result   
-------------- --------------------------------------------  
7              <Location LocationID="30" LaborHrs="1"/>  
               <Location LocationID="50" LaborHrs="3"/>  
               <Location LocationID="60" LaborHrs="4"/>  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Not ()** solo admite argumentos de tipo XS: Boolean, o node () *, o la secuencia vacía.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
