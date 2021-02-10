---
description: Cláusula COMPUTE de forma
title: Shape Compute (cláusula) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- compute clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: 3fdfead2-b5ab-4163-9b1d-3d2143a5db8c
author: rothja
ms.author: jroth
ms.openlocfilehash: b246b1e70f18836455008f8d9e3c1b972e71f183
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037005"
---
# <a name="shape-compute-clause"></a>Cláusula COMPUTE de forma
Una cláusula COMPUTE Shape genera un conjunto de **registros** primario cuyas columnas constan de una referencia al **conjunto de registros** secundario; columnas opcionales cuyo contenido es el capítulo, el nuevo o las columnas calculadas, o el resultado de la ejecución de funciones de agregado en el **conjunto de registros** secundario o en un **conjunto de registros** con forma anterior; y cualquier columna del conjunto de **registros** secundario incluido en la cláusula opcional by.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
SHAPE child-command [AS] child-alias  
   COMPUTE child-alias [[AS] name], [appended-column-list]  
   [BY grp-field-list]  
```  
  
## <a name="description"></a>Descripción  
 Las partes de esta cláusula son las siguientes:  
  
 *comando secundario*  
 Consta de uno de los siguientes elementos:  
  
-   Un comando de consulta entre llaves (" {} ") que devuelve un objeto secundario de **conjunto de registros** . El comando se emite al proveedor de datos subyacente y su sintaxis depende de los requisitos de ese proveedor. Normalmente será el lenguaje SQL, aunque ADO no requiere ningún lenguaje de consulta determinado.  
  
-   Nombre de un **conjunto de registros** con forma existente.  
  
-   Otro comando de forma.  
  
-   La palabra clave TABLE, seguida del nombre de una tabla en el proveedor de datos.  
  
 *alias secundario*  
 Alias que se usa para hacer referencia al **conjunto de registros** devuelto por el *comando secundario.* El *alias secundario* se requiere en la lista de columnas de la cláusula COMPUTE y define la relación entre los objetos de **conjunto de registros** primario y secundario.  
  
 *appended-column-list*  
 Lista en la que cada elemento define una columna en el elemento primario generado. Cada elemento contiene una columna de capítulo, una nueva columna, una columna calculada o un valor resultante de una función de agregado en el **conjunto de registros** secundario.  
  
 *lista de campos de GRP*  
 Una lista de columnas de los objetos de **conjunto de registros** primario y secundario que especifica cómo se deben agrupar las filas en el elemento secundario.  
  
 Para cada columna de la *lista de campos de GRP,* hay una columna correspondiente en los objetos de conjunto de **registros** primario y secundario. Para cada fila del conjunto de **registros** primario, las columnas *GRP-Field-List* tienen valores únicos, y el **conjunto de registros** secundario al que hace referencia la fila primaria consta únicamente de filas secundarias cuyas columnas *GRP-Field-List* tienen los mismos valores que la fila primaria.  
  
 Si se incluye la cláusula BY, las filas del **conjunto de registros** secundario se agruparán en función de las columnas de la cláusula COMPUTE. El **conjunto de registros** primario contendrá una fila por cada grupo de filas del **conjunto de registros** secundario.  
  
 Si se omite la cláusula BY, todo el **conjunto de registros** secundario se trata como un único grupo y el **conjunto de registros** primario contendrá exactamente una fila. Esa fila hará referencia a todo el **conjunto de registros** secundario. La omisión de la cláusula BY permite calcular agregados "total general" sobre todo el conjunto de **registros** secundario.  
  
 Por ejemplo:  
  
```  
SHAPE {select * from Orders} AS orders             COMPUTE orders, SUM(orders.OrderAmount) as TotalSales         
```  
  
 Independientemente de la forma en que se crea el **conjunto de registros** primario (mediante compute o con append), contiene una columna Chapter que se utiliza para relacionarla con un **conjunto de registros** secundario. Si lo desea, el **conjunto de registros** primario también puede contener columnas que contengan agregados (SUM, min, Max, etc.) en las filas secundarias. Tanto el **conjunto de registros** primario como el secundario pueden contener columnas que contengan una expresión en la fila del **conjunto de registros**, así como columnas nuevas e inicialmente vacías.  
  
## <a name="operation"></a>Operación  
 El *comando secundario* se emite al proveedor, que devuelve un **conjunto de registros** secundario.  
  
 La cláusula COMPUTE especifica las columnas del **conjunto de registros** primario, que puede ser una referencia al conjunto de **registros** secundario, uno o varios agregados, una expresión calculada o nuevas columnas. Si hay una cláusula BY, las columnas que define también se anexan al **conjunto de registros** primario. La cláusula BY especifica cómo se agrupan las filas del **conjunto de registros** secundario.  
  
 Por ejemplo, suponga que tiene una tabla, denominada demográficas, que consta de los campos estado, ciudad y población. (Las cifras de población de la tabla se proporcionan únicamente como ejemplo).  
  
|State|City|Población|  
|-----------|----------|----------------|  
|WA|Seattle|700.000|  
|O BIEN|Medford|200 000|  
|O BIEN|Portland|400.000|  
|CA|Los Angeles|800.000|  
|CA|San Diego|600.000|  
|WA|Tacoma|500.000|  
|O BIEN|Corvallis|300 000|  
  
 Ahora, emita este comando de forma:  
  
```  
rst.Open  "SHAPE {select * from demographics} AS rs "  & _  
          "COMPUTE rs, SUM(rs.population) BY state", _  
           objConnection  
```  
  
 Este comando abre un **conjunto de registros** con forma con dos niveles. El nivel primario es un **conjunto de registros** generado con una columna de agregado ( `SUM(rs.population)` ), una columna que hace referencia al **conjunto de registros** secundario ( `rs` ) y una columna para agrupar el **conjunto de registros** secundario ( `state` ). El nivel secundario es el **conjunto de registros** devuelto por el comando de consulta ( `select * from demographics` ).  
  
 Las filas de detalle del **conjunto de registros** secundario se agruparán por estado, pero de lo contrario no en ningún orden determinado. Es decir, los grupos no estarán en orden alfabético o numérico. Si desea que se ordene el **conjunto de registros** primario, puede utilizar el método de **ordenación del conjunto** de registros para ordenar el conjunto de **registros** primario.  
  
 Ahora puede navegar por el conjunto de **registros** primario abierto y obtener acceso a los objetos de **conjunto de registros** de detalle secundarios. Para obtener más información, vea [obtener acceso a las filas de un conjunto de registros jerárquico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="resultant-parent-and-child-detail-recordsets"></a>Conjuntos de registros de detalle primarios y secundarios resultantes  
  
### <a name="parent"></a>Parent  
  
|SUM (RS. Llenado|rs|State|  
|---------------------------|--------|-----------|  
|1,3 millones|Referencia a Child1|CA|  
|1 200 000|Referencia a child2|WA|  
|1,1 millones|Referencia a child3|O BIEN|  
  
## <a name="child1"></a>Secundario1  
  
|State|City|Población|  
|-----------|----------|----------------|  
|CA|Los Angeles|800.000|  
|CA|San Diego|600.000|  
  
## <a name="child2"></a>Secundario2  
  
|State|City|Población|  
|-----------|----------|----------------|  
|WA|Seattle|700.000|  
|WA|Tacoma|500.000|  
  
## <a name="child3"></a>Secundario3  
  
|State|City|Población|  
|-----------|----------|----------------|  
|O BIEN|Medford|200 000|  
|O BIEN|Portland|400.000|  
|O BIEN|Corvallis|300 000|  
  
## <a name="see-also"></a>Consulte también  
 [Obtener acceso a las filas de un conjunto de registros jerárquico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Información general sobre la forma de datos](../../../ado/guide/data/data-shaping-overview.md)   
 [Field (objeto)](../../../ado/reference/ado-api/field-object.md)   
 [Gramática de forma formal](../../../ado/guide/data/formal-shape-grammar.md)   
 [Objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Proveedores necesarios para el modelado de datos](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Cláusula APPEND de forma](../../../ado/guide/data/shape-append-clause.md)   
 [Comandos Shape en general](../../../ado/guide/data/shape-commands-in-general.md)   
 [Value (propiedad, ADO)](../../../ado/reference/ado-api/value-property-ado.md)   
 [Visual Basic para las funciones de aplicaciones](../../../ado/guide/data/visual-basic-for-applications-functions.md)
