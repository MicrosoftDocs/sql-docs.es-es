---
title: Función Ceiling (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función Ceiling () de XQuery para devolver el número más pequeño sin una parte fraccionaria que no es menor que el valor del argumento de la función.
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
- fn:ceiling function
- ceiling function [XQuery]
ms.assetid: 594f1dd0-3c27-41b3-b809-9ce6714c5a97
author: rothja
ms.author: jroth
ms.openlocfilehash: 1ea2dbaab164c57acf9e9bc166ee431e80783648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341580"
---
# <a name="numeric-values-functions---ceiling"></a>Funciones de valores numéricos: ceiling 
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve el número más pequeño, sin la parte fraccionaria, que no sea menor que el valor de su argumento. Si el argumento es una secuencia vacía, devuelve la secuencia vacía.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:ceiling ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Número al que se aplica la función.  
  
## <a name="remarks"></a>Observaciones  
 Si el tipo de *$arg* es uno de los tres tipos base numéricos, **xs: Float**, **xs: Double** o **xs: decimal**, el tipo de valor devuelto es el mismo que el tipo de *$arg* .  
  
 Si el tipo de *$arg* es un tipo derivado de uno de los tipos numéricos, el tipo de valor devuelto es el tipo numérico base.  
  
 Si la entrada de las funciones FN: Floor, FN: Ceiling o FN: Round es **XDT: untypedAtomic**, se convierte implícitamente a **xs: Double**.  
  
 Cualquier otro tipo genera un error estático.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-ceiling-xquery-function"></a>A. Usar la función ceiling() de XQuery  
 Para el modelo de producto 7, esta consulta devuelve una lista de las ubicaciones de los centros de trabajo del proceso de fabricación del modelo de producto. Para cada ubicación de centro de trabajo, la consulta devuelve el Id. de ubicación, las horas de trabajo y el tamaño del lote, en caso de que estén documentados. La consulta utiliza la función **Ceiling** para devolver las horas de trabajo como valores de tipo **decimal**.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
     for $i in /AWMI:root/AWMI:Location  
     return   
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ ceiling($i/@LaborHours) }" >  
                    {   
                      $i/@LotSize  
                    }    
       </Location>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   El prefijo de espacio de nombres AWMI viene del inglés Adventure Works Manufacturing Instructions (instrucciones de fabricación de Adventure Works). Este prefijo hace referencia al mismo espacio de nombres que se utiliza en el documento que se consulta.  
  
-   **Instructions** es una columna de tipo **XML** . Por lo tanto, el [método Query () (tipo de datos XML)](../t-sql/xml/query-method-xml-data-type.md) se utiliza para especificar XQuery. La instrucción de XQuery se especifica como el argumento para el método de consulta.  
  
-   **para... Return** es una construcción de bucle. En la consulta, el bucle **for** identifica una lista de \<Location> elementos. Para cada ubicación del centro de trabajo, la instrucción **Return** del bucle **for** describe el XML que se va a generar:  
  
    -   \<Location>Elemento que tiene los atributos LocationID y LaborHrs. La expresión correspondiente situada dentro de las llaves ({ }) recupera los valores requeridos del documento.  
  
    -   La expresión {$ i/@LotSize } recupera el atributo de exceso del documento, si está presente.  
  
    -   El resultado es el siguiente:  
  
```  
ProductModelID Result    
-------------- ------------------------------------------------------  
7      <Location LocationID="10" LaborHrs="3" LotSize="100"/>  
       <Location LocationID="20" LaborHrs="2" LotSize="1"/>     
       <Location LocationID="30" LaborHrs="1" LotSize="1"/>     
       <Location LocationID="45" LaborHrs="1" LotSize="20"/>  
       <Location LocationID="60" LaborHrs="3" LotSize="1"/>     
       <Location LocationID="60" LaborHrs="4" LotSize="1"/>  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Ceiling ()** asigna todos los valores enteros a XS: decimal.  
  
## <a name="see-also"></a>Consulte también  
 [Función Floor &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Función Round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)  
  
  
