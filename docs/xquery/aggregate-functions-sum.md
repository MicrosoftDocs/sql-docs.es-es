---
title: Función SUM (XQuery) | Microsoft Docs
description: Obtenga información sobre la función XQuery SUM () que devuelve la suma de una secuencia de números.
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
- sum function [XQuery]
- fn:sum function
ms.assetid: 12288f37-b54c-4237-b75e-eedc5fe8f96d
author: rothja
ms.author: jroth
ms.openlocfilehash: e9d68fba9560ef4b0fe99f1263e8d65c57a7c6a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341937"
---
# <a name="aggregate-functions---sum"></a>Funciones de agregado: sum
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve la suma de un flujo de números.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:sum($arg as xdt:anyAtomicType*) as xdt:anyAtomicType  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de valores atómicos cuya suma se va a calcular.  
  
## <a name="remarks"></a>Observaciones  
 Todos los tipos de valores atomizados que se pasan a **SUM ()** deben ser subtipos del mismo tipo base. Los tipos base aceptados son los tres tipos numéricos base integrados o xdt:untypedAtomic. Los valores del tipo xdt:untypedAtomic se convierten a xs:double. Si hay una combinación de estos tipos, o si se pasan otros valores de otros tipos, se genera un error estático.  
  
 El resultado de **SUM ()** recibe el tipo base de los tipos pasados, como XS: Double en el caso de XDT: untypedAtomic, incluso si la entrada es opcionalmente la secuencia vacía. Si se trata de una entrada vacía estática, el resultado es 0 con el tipo estático y dinámico xs:integer.  
  
 La función **SUM ()** devuelve la suma de los valores numéricos. Si un valor XDT: untypedAtomic no se puede convertir a XS: Double, el valor se omite en la secuencia de entrada, *$arg*. Si la entrada es una secuencia vacía calculada dinámicamente, se devuelve el valor 0 del tipo base utilizado.  
  
 La función devuelve un error en tiempo de ejecución cuando se produce una excepción por desbordamiento o por valores fuera del intervalo.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos.  
  
### <a name="a-using-the-sum-xquery-function-to-find-the-total-combined-number-of-labor-hours-for-all-work-center-locations-in-the-manufacturing-process"></a>A. Utilizar la función sum() de XQuery para calcular el número total de horas de trabajo de todos los centros de trabajo del proceso de fabricación  
 La consulta siguiente averigua el número total de horas de trabajo para todos los centros de trabajo del proceso de fabricación de todos los modelos de producto para los que se almacenan instrucciones de fabricación.  
  
```  
SELECT Instructions.query('         
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
  <ProductModel PMID= "{ sql:column("Production.ProductModel.ProductModelID") }"         
  ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >         
   <TotalLaborHrs>         
     { sum(//AWMI:Location/@LaborHours) }         
   </TotalLaborHrs>         
 </ProductModel>         
    ') as Result         
FROM Production.ProductModel         
WHERE Instructions is not NULL         
```  
  
 El resultado parcial es el siguiente.  
  
```  
<ProductModel PMID="7" ProductModelName="HL Touring Frame">  
   <TotalLaborHrs>12.75</TotalLaborHrs>  
</ProductModel>  
<ProductModel PMID="10" ProductModelName="LL Touring Frame">  
  <TotalLaborHrs>13</TotalLaborHrs>  
</ProductModel>  
...  
```  
  
 En lugar de obtener el resultado como XML, puede escribir la consulta de manera que genere resultados relacionales, como la consulta siguiente:  
  
```  
SELECT ProductModelID,         
        Name,         
        Instructions.value('declare namespace   
      AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
    sum(//AWMI:Location/@LaborHours)', 'float') as TotalLaborHours         
FROM Production.ProductModel         
WHERE Instructions is not NULL          
```  
  
 Éste es un resultado parcial:  
  
```  
ProductModelID Name                 TotalLaborHours         
-------------- -------------------------------------------------  
7              HL Touring Frame           12.75                   
10             LL Touring Frame           13                      
43             Touring Rear Wheel         3                       
...  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   Solo se admite la versión del único argumento de **SUM ()** .  
  
-   Si la entrada es una secuencia vacía calculada dinámicamente, se devuelve el valor 0 del tipo base utilizado, en lugar del tipo xs:integer.  
  
-   La función **SUM ()** asigna todos los enteros a XS: decimal.  
  
-   No se admite la función **SUM ()** en valores de tipo XS: Duration.  
  
-   No se admiten las secuencias que mezclan tipos en límites de tipo base.  
  
-   La suma ((XS: Double ("INF"), XS: Double ("-INF"))) produce un error de dominio.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
