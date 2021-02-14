---
title: Atomización (XQuery) | Microsoft Docs
description: Obtenga información sobre el proceso de atomización en XQuery en el que se extraen los valores con tipo de un elemento.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, atomization
- atomization [XQuery]
ms.assetid: e3d7cf2f-c6fb-43c2-8538-4470a6375af5
author: rothja
ms.author: jroth
ms.openlocfilehash: e12ac51b38073363edf7774f4ac8046c1985b38c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340536"
---
# <a name="atomization-xquery"></a>Atomización (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  La atomización es un proceso que consiste en extraer el valor con tipo de un elemento. Este proceso se produce en determinadas circunstancias. Algunos de los operadores XQuery, como los aritméticos y los de comparación, dependen de este proceso. Por ejemplo, al aplicar operadores aritméticos directamente a los nodos, el valor con tipo de un nodo se recupera primero al invocar implícitamente la [función de datos](../xquery/data-accessor-functions-data-xquery.md). De esta manera, el valor atómico se pasa al operador aritmético como un operando.  
  
 Por ejemplo, la consulta siguiente devuelve el total de los atributos LaborHours. En este caso, los **datos ()** se aplican implícitamente a los nodos de atributo.  
  
```  
declare @x xml  
set @x='<ROOT><Location LID="1" SetupTime="1.1" LaborHours="3.3" />  
<Location LID="2" SetupTime="1.0" LaborHours="5" />  
<Location LID="3" SetupTime="2.1" LaborHours="4" />  
</ROOT>'  
-- data() implicitly applied to the attribute node sequence.  
SELECT @x.query('sum(/ROOT/Location/@LaborHours)')  
```  
  
 Aunque no es necesario, también puede especificar explícitamente la función **Data ()** :  
  
```  
SELECT @x.query('sum(data(ROOT/Location/@LaborHours))')  
```  
  
 Otro ejemplo de atomización implícita se produce al utilizar operadores aritméticos. El **+** operador requiere valores atómicos y los **datos ()** se aplican implícitamente para recuperar el valor atómico del atributo LaborHours. La consulta se especifica en la columna instructions del tipo **XML** de la tabla ProductModel. La consulta siguiente devuelve el atributo LaborHours tres veces. Respecto a la consulta, observe lo siguiente:  
  
-   Al construir el atributo OrigninalLaborHours, la atomización se aplica implícitamente a la secuencia singleton devuelta por (`$WC/@LaborHours`). El valor con tipo del atributo LaborHours se asigna a OrigninalLaborHours.  
  
-   Al construir el atributo UpdatedLaborHoursV1, el operador aritmético requiere valores atómicos. Por lo tanto, los **datos ()** se aplican implícitamente al atributo LaborHours devuelto por ( `$WC/@LaborHours` ). A continuación, se le agrega el valor atómico 1. La construcción del atributo UpdatedLaborHoursV2 muestra la aplicación explícita de **datos ()**, pero no es obligatorio.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location[1]  
        return  
            <WC OriginalLaborHours = "{ $WC/@LaborHours }"  
                UpdatedLaborHoursV1 = "{ $WC/@LaborHours + 1 }"   
                UpdatedLaborHoursV2 = "{ data($WC/@LaborHours) + 1 }" >  
            </WC>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```  
<WC OriginalLaborHours="2.5"   
    UpdatedLaborHoursV1="3.5"   
    UpdatedLaborHoursV2="3.5" />  
```  
  
 La atomización da lugar a una instancia de tipo simple, un conjunto vacío o un error de tipo estático.  
  
 La atomización también se produce en parámetros de expresión de comparación pasados a funciones, valores devueltos por funciones, expresiones de **conversión ()** y expresiones de ordenación pasadas en la cláusula ORDER BY.  
  
## <a name="see-also"></a>Consulte también  
 [Conceptos básicos de XQuery](../xquery/xquery-basics.md)   
 [Expresiones de comparación &#40;XQuery&#41;](../xquery/comparison-expressions-xquery.md)   
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
