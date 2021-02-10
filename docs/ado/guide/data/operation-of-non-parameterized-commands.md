---
description: Funcionamiento de los comandos sin parámetros
title: Operación de comandos sin parámetros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- non-parameterized commands [ADO]
- data shaping [ADO], non-parameterized commands
ms.assetid: 9700e50a-9f17-4ba3-8afb-f750741dc6ca
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee255f895e934c4405d888d72f15b8cee9159d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037205"
---
# <a name="operation-of-non-parameterized-commands"></a>Funcionamiento de los comandos sin parámetros
En el caso de comandos sin parámetros, se ejecutan todos los comandos de proveedor y los **conjuntos de registros** se crean durante la ejecución del comando. Si el comando se ejecuta sincrónicamente, todos los **conjuntos de registros** se rellenarán por completo. Si se ha seleccionado un modo de llenado asincrónico, el estado rellenado de los **conjuntos de registros** dependerá del modo de llenado y del tamaño de los **conjuntos de registros**.  
  
 Por ejemplo, el *comando Parent* podría devolver un **conjunto de registros** de clientes para una empresa de una tabla Customers y el *comando secundario* podría devolver un **conjunto de registros** de pedidos para todos los clientes de una tabla Orders.  
  
```  
SHAPE {SELECT * FROM Customers}   
   APPEND ({SELECT * FROM Orders} AS chapOrders   
   RELATE customerID TO customerID)  
```  
  
 En el caso de las relaciones de elementos primarios y secundarios sin parámetros, cada objeto de **conjunto de registros** primario y secundario debe tener una columna en común para asociarlos. Las columnas se denominan en la cláusula Relate, primero la *columna primaria* y, a continuación, la *columna secundaria*. Las columnas pueden tener nombres diferentes en sus respectivos objetos de **conjunto de registros** , pero deben hacer referencia a la misma información para especificar una relación significativa. Por ejemplo, los objetos de conjunto de registros **Customers** y **Orders** podrían tener un campo CustomerID. Dado que la pertenencia del **conjunto de registros** secundario viene determinada por el comando del proveedor, el **conjunto de registros** secundario puede contener filas huérfanas. No se puede acceder a estas filas huérfanas sin tener que remodelar adicional.  
  
 La forma de datos anexa una columna de capítulo al **conjunto de registros** primario. Los valores de la columna Chapter son referencias a las filas del **conjunto de registros** secundario, que cumplen la cláusula Relate. Es decir, el mismo valor está en la *columna primaria* de una fila primaria determinada, tal y como se encuentra en la *columna secundaria* de todas las filas del elemento secundario Chapter. Cuando se utilizan varias cláusulas TO en la misma cláusula Relate, se combinan implícitamente mediante un operador AND. Si las columnas primarias de la cláusula Relate no constituyen una clave para el **conjunto de registros** primario, una sola fila secundaria puede tener varias filas primarias.  
  
 Cuando se tiene acceso a la referencia en la columna Chapter, ADO recupera automáticamente el **conjunto de registros** representado por la referencia. Tenga en cuenta que en un comando sin parámetros, aunque se ha recuperado todo el **conjunto de registros** secundario, el capítulo solo presenta un subconjunto de filas.  
  
 Si la columna anexada no tiene ningún *alias de capítulo*, se generará un nombre automáticamente. Un objeto de [campo](../../reference/ado-api/field-object.md) de la columna se anexará a la colección de [campos](../../reference/ado-api/fields-collection-ado.md) del objeto de **conjunto de registros** y su tipo de datos será **adChapter**.  
  
 Para obtener información sobre cómo navegar por un **conjunto de registros** jerárquico, vea [obtener acceso a las filas de un conjunto de registros jerárquico](./accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](./data-shaping-example.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)