---
description: Información general sobre la forma de datos
title: Información general sobre la forma de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], overview
ms.assetid: 4cb5fd29-4e56-46ac-ae48-a6771c321c0c
author: rothja
ms.author: jroth
ms.openlocfilehash: dbe025e38960b458c2ad5a224bad44c37868d51f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037655"
---
# <a name="data-shaping-overview"></a>Información general sobre la forma de datos
La forma de *datos* significa generar relaciones jerárquicas entre dos o más entidades lógicas en una consulta. La jerarquía se puede ver en las relaciones de elementos primarios y secundarios entre un registro de un [conjunto de registros](../../reference/ado-api/recordset-object-ado.md)y uno o más registros (también denominados capítulo) de otro **conjunto** de registros. En una relación de elementos primarios y secundarios, el **conjunto de registros** primario contiene el **conjunto de registros** secundario. Un ejemplo de una relación jerárquica de este tipo son los clientes y los pedidos. Para cada cliente de una base de datos, puede haber cero o más pedidos. La relación jerárquica puede ser recursiva, lo que significa que los registros terciarios se pueden anidar en un registro secundario. En principio, un registro jerárquico puede estar anidado a cualquier profundidad. En la práctica, ADO limita la recursividad a un máximo de 512 **conjuntos de registros**.  
  
 En general, las columnas de un **conjunto de registros** con forma pueden contener datos de un proveedor de datos como Microsoft® SQL Server, referencias a otro **conjunto de registros**, valores derivados de un cálculo en una sola fila de un conjunto de **registros** o valores derivados de una operación en una columna de un **conjunto de registros** completo. También se puede crear una columna y estar vacía.  
  
 Al recuperar el valor de una columna que contiene una referencia a otro **conjunto de registros**, ADO devuelve automáticamente el **conjunto de registros** real representado por la referencia. La referencia a un **conjunto de registros** es realmente una referencia a un subconjunto del elemento secundario, denominado *capítulo*. Un único elemento primario puede hacer referencia a más de un **conjunto de registros** secundario.  
  
 La compatibilidad de ADO con la forma de datos permite consultar un origen de datos y devolver un **conjunto de registros** en el que un registro (primario) representa un **conjunto de registros**(secundario). En el escenario de pedidos de clientes, puede usar la forma de datos para recuperar la información de los clientes, así como los pedidos realizados por cada cliente en una sola consulta. El conjunto de **registros** resultante también se conoce como **conjunto de registros** con forma.  
  
 Además, el modelado de datos en ADO permite crear nuevos objetos de **conjunto de registros** sin un origen de datos subyacente mediante el uso de la palabra clave **New** para describir los campos de los conjuntos de **registros** primarios y secundarios. El nuevo objeto de **conjunto de registros** se puede rellenar con datos y almacenar de manera persistente. Los desarrolladores también pueden realizar diversos cálculos o agregaciones (por ejemplo, **SUM**, **AVG** y **Max**) en los campos secundarios. La forma de datos también puede crear un **conjunto** de registros primario a partir de un **conjunto de registros** secundario agrupando los registros del elemento secundario y colocando una fila en el elemento primario para cada grupo del elemento secundario.  
  
 SQL regular le permite recuperar datos mediante la sintaxis de **join** , pero esto puede ser ineficaz y difícil, ya que los datos primarios redundantes se repiten en cada registro devuelto para una relación de elementos primarios y secundarios. La forma de los datos puede relacionar un único registro primario en el **conjunto de registros** primario con varios registros secundarios en el **conjunto de registros** secundario, evitando la redundancia de una **combinación**. La mayoría de los usuarios buscan el modelo de programación de varios **conjuntos de registros** primario-secundario más natural y más sencillo de trabajar con el modelo de combinación de **conjunto de registros** único.