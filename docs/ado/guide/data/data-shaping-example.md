---
description: Ejemplo de la forma de datos
title: Ejemplo de forma de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
author: rothja
ms.author: jroth
ms.openlocfilehash: 0199dfc2165d5ae54b4b59a45ec740a9c399f114
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033285"
---
# <a name="data-shaping-example"></a>Ejemplo de la forma de datos
El siguiente comando de forma de datos muestra cómo generar un conjunto de **registros** jerárquico a partir de las tablas **Customers** y **Orders** de la base de datos Northwind.  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 Cuando este comando se usa para abrir un objeto de **conjunto de registros** (como se muestra en [Visual Basic ejemplo de forma de datos](./visual-basic-example-of-data-shaping.md)), crea un capítulo (**chapOrders**) para cada registro devuelto de la tabla **Customers** . Este capítulo consta de un subconjunto del **conjunto de registros** devuelto de la tabla **Orders** . El capítulo **chapOrders** contiene toda la información solicitada sobre los pedidos realizados por el cliente determinado. En este ejemplo, el capítulo consta de tres columnas: **OrderID**, **OrderDate** y **CustomerID**.  
  
 Las dos primeras entradas del **conjunto de registros** con forma resultante son las siguientes:  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria Anders|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 En un comando SHAPE, APPEND se usa para crear un **conjunto de registros** secundario relacionado con el **conjunto de registros** primario (tal y como se devuelve desde el comando específico del proveedor inmediatamente después de la palabra clave Shape que se explicó anteriormente) por la cláusula Relate. Los elementos primarios y secundarios suelen tener al menos una columna en común: el valor de la columna en una fila del elemento primario es el mismo que el valor de la columna en todas las filas del elemento secundario.  
  
 Hay una segunda manera de usar comandos de forma: es decir, para generar un **conjunto de registros** primario a partir de un **conjunto de registros** secundario. Los registros del conjunto de **registros** secundario se agrupan, normalmente mediante la cláusula by, y se agrega una fila al conjunto de **registros** primario para cada grupo resultante en el elemento secundario. Si se omite la cláusula BY, el **conjunto de registros** secundario formará un solo grupo y el **conjunto de registros** primario contendrá exactamente una fila. Esto resulta útil para calcular agregados "totales generales" en todo el **conjunto de registros** secundario.  
  
 La construcción de comandos SHAPE también permite crear un **conjunto de registros** con forma mediante programación. A continuación, puede tener acceso a los componentes del **conjunto de registros** mediante programación o a través de un control visual adecuado. Un comando de forma se emite como cualquier otro texto de comando de ADO. Para obtener más información, vea [comandos Shape en general](./shape-commands-in-general.md).  
  
 Independientemente de la forma en que se forme el **conjunto de registros** principal, contendrá una columna de capítulo que se utiliza para relacionarla con un **conjunto de registros** secundario. Si lo desea, el **conjunto de registros** primario también puede tener columnas que contengan agregados (SUM, min, Max, etc.) en las filas secundarias. Tanto el **conjunto de registros** primario como el secundario pueden tener columnas que contengan una expresión en la fila del **conjunto de registros**, así como columnas nuevas e inicialmente vacías.  
  
 Esta sección continúa con el siguiente tema.  
  
-   [Ejemplo de Visual Basic de forma de datos](./visual-basic-example-of-data-shaping.md)