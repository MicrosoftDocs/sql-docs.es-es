---
description: Funcionamiento de los comandos con parámetros
title: Operación de comandos parametrizados | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
ms.assetid: 4fae0d54-83b6-4ead-99cc-bcf532daa121
author: rothja
ms.author: jroth
ms.openlocfilehash: 55eaf3798ee8d14a776da14010b4b6617d0e2723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032657"
---
# <a name="operation-of-parameterized-commands"></a>Funcionamiento de los comandos con parámetros
Si está trabajando con un **conjunto de registros** secundario grande, especialmente en comparación con el tamaño del **conjunto de registros** primario, pero solo necesita tener acceso a algunos capítulos secundarios, es posible que le resulte más eficaz usar un comando con parámetros.  
  
 Un *comando sin parámetros* recupera los **conjuntos de registros** primarios y secundarios completos, anexa una columna Chapter al elemento primario y, a continuación, asigna una referencia al capítulo secundario relacionado para cada fila primaria.  
  
 Un *comando con parámetros* recupera el **conjunto de registros** primario completo, pero solo recupera el capítulo **conjunto de registros** cuando se tiene acceso a la columna de capítulo. Esta diferencia en la estrategia de recuperación puede producir ventajas de rendimiento significativas.  
  
 Por ejemplo, puede especificar lo siguiente:  
  
```  
SHAPE {SELECT * FROM customer}   
   APPEND ({SELECT * FROM orders WHERE cust_id = ?}   
   RELATE cust_id TO PARAMETER 0)  
```  
  
 Las tablas primaria y secundaria tienen un nombre de columna en común *cust_id*. El *comando secundario* tiene un marcador de posición "?", al que hace referencia la cláusula Relate (es decir, "... PARÁMETRO 0 ").  
  
> [!NOTE]
>  La cláusula PARAMETER solo pertenece a la sintaxis del comando Shape. No está asociado al objeto de [parámetro](../../reference/ado-api/parameter-object.md) de ADO o a la colección de [parámetros](../../reference/ado-api/parameters-collection-ado.md) .  
  
 Cuando se ejecuta el comando de forma con parámetros, ocurre lo siguiente:  
  
1.  El *comando primario* se ejecuta y devuelve un conjunto de **registros** primario de la tabla customers.  
  
2.  Una columna de capítulo se anexa al conjunto de **registros** primario.  
  
3.  Cuando se tiene acceso a la columna Chapter de una fila primaria, el *comando secundario* se ejecuta utilizando el valor de la Customer.cust_id como valor del parámetro.  
  
4.  Todas las filas del conjunto de filas del proveedor de datos creadas en el paso 3 se utilizan para rellenar el **conjunto de registros** secundario. En este ejemplo, todas las filas de la tabla Orders en las que el cust_id es igual al valor de customer.cust_id. De forma predeterminada, los **conjuntos de registros** secundarios se almacenarán en la memoria caché del cliente hasta que se liberen todas las referencias al **conjunto de registros** primario. Para cambiar este comportamiento, establezca las **filas secundarias** de la caché de [propiedades dinámicas](../../reference/ado-api/ado-dynamic-property-index.md) de **conjunto de registros** en **false**.  
  
5.  Una referencia a las filas secundarias recuperadas (es decir, el capítulo del **conjunto de registros** secundario) se coloca en la columna Chapter de la fila actual del **conjunto de registros** primario.  
  
6.  Los pasos 3-5 se repiten cuando se tiene acceso a la columna Chapter de otra fila.  
  
 De forma predeterminada, la propiedad dinámica **Cache Child Rows** está establecida en **true** . El comportamiento del almacenamiento en caché varía en función de los valores de los parámetros de la consulta. En una consulta con un único parámetro, el **conjunto de registros** secundario para un valor de parámetro determinado se almacenará en la memoria caché entre las solicitudes de un elemento secundario con ese valor. El código siguiente muestra este proceso:  
  
```  
SCmd = "SHAPE {select * from customer} " & _  
         "APPEND({select * from orders where cust_id = ?} " & _  
         "RELATE cust_id TO PARAMETER 0) AS chpCustOrder"  
Rst1.Open sCmd, Cnn1  
Set RstChild = Rst1("chpCustOrder").Value  
Rst1.MoveNext      ' Next cust_id passed to Param 0, & new rs fetched   
                   ' into RstChild.  
Rst1.MovePrevious  ' RstChild now holds cached rs, saving round trip.  
```  
  
 En una consulta con dos o más parámetros, se usa un elemento secundario almacenado en caché solo si todos los valores de parámetro coinciden con los valores almacenados en caché.  
  
## <a name="parameterized-commands-and-complex-parent-child-relations"></a>Comandos con parámetros y relaciones de elementos primarios y secundarios complejos  
 Además de usar comandos con parámetros para mejorar el rendimiento de una jerarquía de tipos de combinación de igualdad, los comandos con parámetros se pueden usar para admitir relaciones de elementos primarios y secundarios más complejos. Por ejemplo, considere una pequeña base de datos de liga con dos tablas: una que consta de los equipos (team_id, team_name) y la otra de juegos (Date, home_team, visiting_team).  
  
 Mediante una jerarquía sin parámetros, no hay forma de relacionar las tablas Teams y Games de tal forma que el conjunto de **registros** secundario de cada equipo contenga su programación completa. Puede crear capítulos que contengan la programación de inicio o la programación de carretera, pero no ambos. Esto se debe a que la cláusula Relate le limita a las relaciones de elementos primarios y secundarios del formulario (PC1 = CC1) y (PC2 = PC2). Por lo tanto, si el comando incluía "RELACIONAr team_id con home_team, team_id a visiting_team", obtendría solo los juegos en los que un equipo se reproducira por sí mismo. Lo que desea es "(team_id = home_team) o (team_id = visiting_team)", pero el proveedor de la forma no admite la cláusula o.  
  
 Para obtener el resultado deseado, puede usar un comando con parámetros. Por ejemplo:  
  
```  
SHAPE {SELECT * FROM teams}   
APPEND ({SELECT * FROM games WHERE home_team = ? OR visiting_team = ?}   
        RELATE team_id TO PARAMETER 0,   
               team_id TO PARAMETER 1)   
```  
  
 En este ejemplo se aprovecha la mayor flexibilidad de la cláusula WHERE de SQL para obtener el resultado que necesita.  
  
> [!NOTE]
>  Al utilizar las cláusulas WHERE, los parámetros no pueden usar los tipos de datos de SQL para Text, ntext e Image, o bien se producirá un error que contiene la siguiente descripción: `Invalid operator for data type` .  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](./data-shaping-example.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)