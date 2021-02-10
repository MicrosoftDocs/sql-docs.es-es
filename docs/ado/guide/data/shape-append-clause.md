---
description: Cláusula APPEND Shape
title: Cláusula APPEND de Shape | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], APPEND clause
- append clause [ADO]
ms.assetid: f90fcf55-6b24-401d-94e1-d65bd24bd342
author: rothja
ms.author: jroth
ms.openlocfilehash: 7c6d62d410e6709c93ad6e3d04db627f14f091a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036995"
---
# <a name="shape-append-clause"></a>Cláusula APPEND Shape
La cláusula APPEND del comando Shape anexa una o varias columnas a un **conjunto de registros**. Con frecuencia, estas columnas son columnas de capítulo, que hacen referencia a un **conjunto de registros** secundario.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
SHAPE [parent-command [[AS] parent-alias]] APPEND column-list  
```  
  
## <a name="description"></a>Descripción  
 Las partes de esta cláusula son las siguientes:  
  
 *comando primario*  
 Cero o uno de los siguientes (puede omitir completamente el *comando primario* ):  
  
-   Un comando de proveedor entre llaves (" {} ") que devuelve un objeto de **conjunto de registros** . El comando se emite al proveedor de datos subyacente y su sintaxis depende de los requisitos de ese proveedor. Normalmente será el lenguaje SQL, aunque ADO no requiere ningún lenguaje de consulta determinado.  
  
-   Otro comando de forma se inserta entre paréntesis.  
  
-   La palabra clave TABLE, seguida del nombre de una tabla en el proveedor de datos.  
  
 *alias primario*  
 Alias opcional que hace referencia al conjunto de **registros** primario.  
  
 *lista de columnas*  
 Uno o varios de los siguientes:  
  
-   Columna de agregado.  
  
-   Columna calculada.  
  
-   Nueva columna creada con la nueva cláusula.  
  
-   Columna de capítulo. Una definición de columna de capítulo se incluye entre paréntesis ("()"). Vea la siguiente sintaxis.  
  
```  
SHAPE [parent-command [[AS] parent-alias]]  
   APPEND (child-recordset [ [[AS] child-alias]   
      RELATE parent-column TO child-column | PARAMETER param-number, ... ])  
   [[AS] chapter-alias]   
   [, ... ]  
```  
  
## <a name="remarks"></a>Observaciones  
 *child-recordset*  
 -   Un comando de proveedor entre llaves (" {} ") que devuelve un objeto de **conjunto de registros** . El comando se emite al proveedor de datos subyacente y su sintaxis depende de los requisitos de ese proveedor. Normalmente será el lenguaje SQL, aunque ADO no requiere ningún lenguaje de consulta determinado.  
  
-   Otro comando de forma se inserta entre paréntesis.  
  
-   Nombre de un **conjunto de registros** con forma existente.  
  
-   La palabra clave TABLE, seguida del nombre de una tabla en el proveedor de datos.  
  
 *alias secundario*  
 Un alias que hace referencia al **conjunto de registros** secundario.  
  
 *columna primaria*  
 Columna del conjunto de **registros** devuelta por el *comando primario.*  
  
 *columna secundaria*  
 Columna del conjunto de **registros** devuelta por el *comando secundario*.  
  
 *número de parámetro*  
 Vea [operación de comandos con parámetros](../../../ado/guide/data/operation-of-parameterized-commands.md).  
  
 *Chapter: alias*  
 Un alias que hace referencia a la columna Chapter anexada al elemento primario.  
  
> [!NOTE]
>  La cláusula *"Parent-Column* to *Child-Column"* es realmente una lista, donde cada relación definida está separada por una coma.  
  
> [!NOTE]
>  La cláusula después de la palabra clave APPEND es realmente una lista, donde cada cláusula está separada por una coma y define otra columna que se va a anexar al elemento primario.  
  
Al construir comandos de proveedor a partir de datos proporcionados por el usuario como parte de un comando de forma, forma tratará el comando de proveedor proporcionado por el usuario como una cadena opaca y los pasará fielmente al proveedor. Por ejemplo, en el siguiente comando SHAPE,  
  
```  
SHAPE {select * from t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 La forma ejecutará dos comandos: `select * from t1` y ( `select * from t2 RELATE k1 TO k2)` . Si el usuario proporciona un comando compuesto que consta de varios comandos de proveedor separados por punto y coma, la forma no puede distinguir la diferencia. Por lo tanto, en el siguiente comando SHAPE,  
  
```  
SHAPE {select * from t1; drop table t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 SHAPE ejecuta `select * from t1; drop table t1` y ( `select * from t2 RELATE k1 TO k2),` sin darse cuenta de que `drop table t1` es un comando de proveedor independiente y en este caso, peligroso. Las aplicaciones siempre deben validar los datos proporcionados por el usuario para evitar que puedan producirse ataques de piratas informáticos potenciales.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Funcionamiento de los comandos sin parámetros](../../../ado/guide/data/operation-of-non-parameterized-commands.md)  
  
-   [Funcionamiento de los comandos con parámetros](../../../ado/guide/data/operation-of-parameterized-commands.md)  
  
-   [Comandos híbridos](../../../ado/guide/data/hybrid-commands.md)  
  
-   [Cláusulas COMPUTE de forma que intervengan](../../../ado/guide/data/intervening-shape-compute-clauses.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](../../../ado/guide/data/data-shaping-example.md)   
 [Gramática de forma formal](../../../ado/guide/data/formal-shape-grammar.md)   
 [Comandos Shape en General](../../../ado/guide/data/shape-commands-in-general.md)
