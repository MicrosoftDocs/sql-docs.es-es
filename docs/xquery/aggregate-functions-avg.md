---
title: AVG (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la función de XQuery AVG () que devuelve el promedio de una secuencia especificada de números.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:avg function
- avg function [XQuery]
ms.assetid: 0cc60267-3c56-4a88-8ad7-bb07f0255d56
author: rothja
ms.author: jroth
ms.openlocfilehash: ee09ab89e1acb0fc8ddad23d002e2cd0e136bc59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340491"
---
# <a name="aggregate-functions---avg"></a>Funciones de agregado: avg
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Devuelve el promedio de una secuencia de números.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:avg($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de valores atómicos cuyo promedio se va a calcular.  
  
## <a name="remarks"></a>Comentarios  
 Todos los tipos de los valores atomizados que se pasan a **AVG ()** deben ser un subtipo de exactamente uno de los tres tipos base numéricos integrados o XDT: untypedAtomic. No se pueden mezclar. Los valores de tipo xdt:untypedAtomic se tratan como xs:double. El resultado de **AVG ()** recibe el tipo base de los tipos pasados, como XS: Double en el caso de XDT: untypedAtomic.  
  
 Si la entrada está vacía estáticamente, el vacío es implícito y se genera un error estático.  
  
 La función **AVG ()** devuelve el promedio de los números calculados. Por ejemplo:  
  
 **SUM (** *$arg* **) número de div (** *$arg* **)**  
  
 Si *$arg* es una secuencia vacía, se devuelve la secuencia vacía.  
  
 Si un valor XDT: untypedAtomic no se puede convertir a XS: Double, el valor se descarta en la secuencia de entrada, *$arg*.  
  
 En los demás casos, la función devuelve un error estático.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-avg-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-in-which-labor-hours-are-greater-than-the-average-for-all-work-center-locations"></a>A. Utilizar la función avg() de XQuery para buscar los centros de trabajo del proceso de fabricación cuyas horas de trabajo son superiores al promedio de todos los centros de trabajo  
 Puede volver a escribir la consulta proporcionada en la [función min (XQuery)](../xquery/aggregate-functions-min.md) para usar la función **AVG ()** .  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **AVG ()** asigna todos los enteros a XS: decimal.  
  
-   No se admite la función **AVG ()** en valores de tipo XS: Duration.  
  
-   No se admiten las secuencias que mezclan tipos en límites de tipo base.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
