---
title: Función min (XQuery) | Microsoft Docs
description: Obtenga información sobre la función min () de XQuery que devuelve el elemento de una secuencia cuyo valor es menor que el de todos los demás.
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
- fn:min function
- min function [XQuery]
ms.assetid: db0b7d94-3fa6-488f-96d6-6a9a7d6eda23
author: rothja
ms.author: jroth
ms.openlocfilehash: 19c4204e84759bab022d0f0765b9d6154d23d003
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341943"
---
# <a name="aggregate-functions---min"></a>Funciones de agregado: min
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve de una secuencia de valores atómicos, *$arg*, un elemento cuyo valor es menor que el de todos los demás.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:min($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de elementos de los cuales se devolverá el valor mínimo.  
  
## <a name="remarks"></a>Observaciones  
 Todos los tipos de valores atomizados que se pasan a **min ()** deben ser subtipos del mismo tipo base. Los tipos base aceptados son los tipos que admiten la operación **gt** . Entre estos tipos se incluyen los tres tipos base numéricos integrados, los tipos base de fecha y hora, xs:string, xs:boolean y xdt:untypedAtomic. Los valores del tipo xdt:untypedAtomic se convierten a xs:double. Si hay una combinación de estos tipos, o si se pasan otros valores de otros tipos, se genera un error estático.  
  
 El resultado de **min ()** recibe el tipo base de los tipos pasados, como XS: Double en el caso de XDT: untypedAtomic. Si la entrada se encuentra estáticamente vacía, se considera implícitamente vacía y se devuelve un error estático.  
  
 La función **min ()** devuelve el valor de la secuencia que es menor que otro en la secuencia de entrada. En el caso de los valores xs:string, se utiliza la intercalación de puntos de código Unicode predeterminada. Si un valor XDT: untypedAtomic no se puede convertir a XS: Double, el valor se omite en la secuencia de entrada, *$arg*. Si la entrada es una secuencia vacía calculada dinámicamente, se devolverá la secuencia vacía.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-min-xquery-function-to-find-the-work-center-location-that-has-the-fewest-labor-hours"></a>A. Usar la función min() de XQuery para encontrar la ubicación de centro de trabajo con el menor número de horas de trabajo  
 La consulta siguiente recupera todas las ubicaciones de centro de trabajo del proceso de fabricación de un modelo de producto (ProductModelID=7) con el menor número de horas de trabajo (LaborHours). Por lo general, se devuelve una sola ubicación, tal como se muestra a continuación. Si existen varias ubicaciones con el mismo número mínimo de horas de trabajo, se devolverán todas.  
  
```  
select ProductModelID, Name, Instructions.query('  
  declare namespace AWMI=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  for   $Location in /AWMI:root/AWMI:Location  
  where $Location/@LaborHours =  
          min( /AWMI:root/AWMI:Location/@LaborHours )  
return  
  <Location WCID=     "{ $Location/@LocationID }"   
              LaborHrs= "{ $Location/@LaborHours }" />  
  ') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La palabra clave **namespace** del prólogo de XQuery define un prefijo de espacio de nombres. A continuación, el prefijo se utiliza en el cuerpo de XQuery.  
  
 El cuerpo de XQuery crea el XML que tiene un \<Location> elemento con los atributos WCID y **LaborHrs** .  
  
-   Esta consulta también recupera los valores de ProductModelID y de nombres.  
  
 El resultado es el siguiente:  
  
```  
ProductModelID   Name              Result  
---------------  ----------------  ---------------------------------  
7                HL Touring Frame  <Location WCID="45" LaborHrs="0.5"/>   
```  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **min ()** asigna todos los enteros a XS: decimal.  
  
-   No se admite la función **min ()** en valores de tipo XS: Duration.  
  
-   No se admiten las secuencias que mezclan tipos en límites de tipo base.  
  
-   No se admite la opción sintáctica que proporciona una intercalación.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
