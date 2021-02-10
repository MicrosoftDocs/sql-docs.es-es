---
description: Comandos Shape en General
title: Comandos Shape en general | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], shape commands
ms.assetid: 1fac7831-a187-4b15-9b43-aad380c5556c
author: rothja
ms.author: jroth
ms.openlocfilehash: 021d47eb011a4c0628b6f356772e82c591747103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032467"
---
# <a name="shape-commands-in-general"></a>Comandos Shape en General
La forma de datos define las columnas de un **conjunto de registros** con forma, las relaciones entre las entidades representadas por las columnas y la manera en que el **conjunto de registros** se rellena con datos.  
  
 Un **conjunto de registros** con forma puede constar de los siguientes tipos de columnas.  
  
|Tipo de columna|Descripción|  
|-----------------|-----------------|  
|datos|Campos de un **conjunto de registros** devueltos por un comando de consulta a un proveedor de datos, una tabla o un **conjunto de registros** con forma anterior.|  
|Capítulo|Referencia a otro **conjunto de registros**, denominado *capítulo*. Las columnas Chapter permiten definir una relación *de elementos primarios y secundarios* en la que el *elemento primario* es el **conjunto de registros** que contiene la columna Chapter y el *elemento secundario* es el **conjunto de registros** representado por el capítulo.|  
|aggregate|El valor de la columna se deriva mediante la ejecución de una *función de agregado* en todas las filas o en una columna de todas las filas de un conjunto de **registros** secundario. (Vea funciones de agregado en el tema siguiente, [funciones de agregado, la función Calc y la palabra clave New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)).|  
|expresión calculada|El valor de la columna se deriva calculando una expresión Visual Basic para Aplicaciones en las columnas de la misma fila del **conjunto de registros**. La expresión es el argumento de la función CALC. (Vea expresión calculada en el tema siguiente, [funciones de agregado, la función Calc y la palabra clave New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md) y en [funciones Visual Basic para aplicaciones](../../../ado/guide/data/visual-basic-for-applications-functions.md)).|  
|nuevo|Campos de fabricación vacíos, que se pueden rellenar con datos en otro momento. La columna se define con la palabra clave NEW. (Vea la nueva palabra clave en el tema siguiente, [funciones de agregado, la función Calc y la palabra clave New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)).|  
  
 Un comando de forma puede contener una cláusula que especifica un comando de consulta para un proveedor de datos subyacente que devolverá un objeto de **conjunto de registros** . La sintaxis de la consulta depende de los requisitos del proveedor de datos subyacente. Normalmente será SQL, aunque ADO no requiere el uso de un lenguaje de consulta determinado.  
  
 Los comandos de forma pueden ser emitidos por objetos de **conjunto de registros** o estableciendo la propiedad [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) del objeto de [comando](../../../ado/reference/ado-api/command-object-ado.md) y, a continuación, llamando al método [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) .  
  
 Puede usar una cláusula SQL JOIN para relacionar dos tablas; sin embargo, un **conjunto de registros** jerárquico puede representar la información de forma más eficaz. Cada fila de un **conjunto de registros** creado por una combinación repite información de forma redundante de una de las tablas. Un conjunto de **registros** jerárquico solo tiene un **conjunto de registros** primario para cada uno de los distintos objetos de **conjunto de registros** secundarios.  
  
 Los comandos de forma se pueden anidar. Es decir, el comando *primario* o *secundario* puede ser otro comando de forma.  
  
 El proveedor de la forma siempre devuelve un cursor de cliente, incluso cuando el usuario especifica una ubicación de cursor de **adUseServer**.  
  
 Puede tener acceso a los componentes de **conjunto** de registros del **conjunto de registros** con forma mediante programación o a través de un control visual adecuado.  
  
 Microsoft proporciona una herramienta visual que genera comandos de forma (vea el [Diseñador de entorno de datos](/previous-versions/visualstudio/aa445793(v=vs.60)) en la documentación de Visual Basic 6) y otro que muestra los cursores jerárquicos (vea "usar el control Hierarchical FlexGrid de Microsoft" en la documentación de Visual Basic 6).  
  
 Para obtener información sobre cómo navegar por un **conjunto de registros** jerárquico, vea [obtener acceso a las filas de un conjunto de registros jerárquico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 Para obtener información precisa sobre los comandos Shape sintácticamente correctos, vea [gramática formal](../../../ado/guide/data/formal-shape-grammar.md)de las formas.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Las funciones de agregado, la función CALC y la palabra clave NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)  
  
-   [Emitir comandos al proveedor de datos subyacente](../../../ado/guide/data/issuing-commands-to-the-underlying-data-provider.md)