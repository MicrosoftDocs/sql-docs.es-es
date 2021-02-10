---
description: 'Apéndice B: Tablas de transición de estado de ODBC'
title: 'Apéndice B: tablas de transición de estado de ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC]
- transitioning states [ODBC], about state transitions
- state transitions [ODBC], about state transitions
ms.assetid: 15088dbe-896f-4296-b397-02bb3d0ac0fb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9bb65299be2778897b72303cccb99043a3b01406
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061110"
---
# <a name="appendix-b-odbc-state-transition-tables"></a>Apéndice B: Tablas de transición de estado de ODBC
En las tablas de este apéndice se muestra cómo las funciones ODBC causan transiciones del entorno, la conexión, la instrucción y los Estados del descriptor. Normalmente, el estado del entorno, la conexión, la instrucción o el descriptor determina cuándo se puede llamar a las funciones que utilizan el tipo de identificador (entorno, conexión, instrucción o descriptor) correspondiente. Los Estados del entorno, la conexión, la instrucción y el descriptor se superponen aproximadamente, tal y como se muestra en las ilustraciones siguientes. Por ejemplo, la superposición exacta de los Estados de conexión C5 y C6 y los Estados de instrucción S1 a través de S12 es dependiente del origen de datos, ya que las transacciones se inician en distintos momentos en orígenes de datos diferentes, y el estado de descriptor D1i (descriptor asignado implícitamente) depende del estado de la instrucción a la que está asociado el descriptor, Para obtener una descripción de cada Estado, vea [transiciones de entorno](../../../odbc/reference/appendixes/environment-transitions.md), [transiciones de conexión](../../../odbc/reference/appendixes/connection-transitions.md), [transiciones de instrucciones](../../../odbc/reference/appendixes/statement-transitions.md)y [transiciones de descriptor](../../../odbc/reference/appendixes/descriptor-transitions.md), más adelante en este apéndice.  
  
 El entorno y los Estados de conexión se superponen de la siguiente manera:  
  
 ![Los estados del entorno y de la conexión se superponen](../../../odbc/reference/appendixes/media/app01.gif "app01")  
  
 Los Estados de la conexión y la instrucción se superponen de la siguiente manera:  
  
 ![Los estados de la conexión y la instrucción se superponen](../../../odbc/reference/appendixes/media/app02.gif "app02")  
  
 Los Estados de la instrucción y del descriptor se superponen del siguiente modo:  
  
 ![Los estados de la instrucción y del descriptor se superponen](../../../odbc/reference/appendixes/media/app03.gif "app03")  
  
 Los Estados de conexión y descriptor se superponen de la siguiente manera:  
  
 ![Los estados de la conexión y del descriptor se superponen](../../../odbc/reference/appendixes/media/app04.gif "app04")  
  
 Cada entrada de una tabla de transición puede tener uno de los valores siguientes:  
  
-   **--** -El estado no cambia después de ejecutar la función.  
  
-   **E**  

     **_n_** , **C_n_**, **S_n_** o **D_n_** : el entorno, la conexión, la instrucción o el estado del descriptor se mueven al estado especificado.  
 
-   **(Admitir)** : se pasó un identificador no válido a la función. Si el identificador era un identificador nulo o era un identificador válido del tipo incorrecto; por ejemplo, se pasó un identificador de conexión cuando se requería un identificador de instrucción: la función devuelve SQL_INVALID_HANDLE; de lo contrario, el comportamiento es indefinido y probablemente grave. Este error solo se muestra cuando es el único resultado posible de llamar a la función en el estado especificado. Este error no cambia el estado y siempre lo detecta el administrador de controladores, como se indica en los paréntesis.  
  
-   **NS** -siguiente estado. La transición de la instrucción es la misma que si la instrucción no hubiera pasado por los Estados asincrónicos. Por ejemplo, supongamos que una instrucción que crea un conjunto de resultados entra en el estado S11 desde el estado S1 porque **SQLExecDirect** devolvió SQL_STILL_EXECUTING. La notación NS en State S11 significa que las transiciones de la instrucción son las mismas que las de una instrucción en el estado S1 que crea un conjunto de resultados. Si **SQLExecDirect** devuelve un error, la instrucción permanece en el estado S1; Si se realiza correctamente, la instrucción pasa al estado S5; Si necesita datos, la instrucción pasa al estado S8; y si todavía se está ejecutando, permanece en el estado S11.  

-   **_Xxxxx_**  o **(*xxxxx*)** : un SQLSTATE que está relacionado con la tabla de transición; Los SQLSTATEs detectados por el administrador de controladores se incluyen entre paréntesis. La función devolvió SQL_ERROR y el SQLSTATE especificado, pero el estado no cambia. Por ejemplo, si se llama a **SQLExecute** antes que a **SQLPrepare**, devuelve SQLSTATE HY010 (error de secuencia de función).  

> [!NOTE]  
>  Las tablas no muestran errores no relacionados con las tablas de transición que no cambian el estado. Por ejemplo, cuando se llama a **SQLAllocHandle** en el estado del entorno E1 y se devuelve SQLSTATE HY001 (error de asignación de memoria), el entorno permanece en el estado E1; Esto no se muestra en la tabla de transición de entorno para **SQLAllocHandle**.  
  
 Si el entorno, la conexión, la instrucción o el descriptor pueden moverse a más de un estado, se muestra cada estado posible y una o varias notas al pie explican las condiciones en las que tiene lugar cada transición. Las siguientes notas al pie pueden aparecer en cualquier tabla.  
  
|Footnote|Significado|  
|--------------|-------------|  
|b|Antes o después de. El cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.|  
|c|Función actual. La función actual se estaba ejecutando de forma asincrónica.|  
|d|Necesita datos. La función devolvió SQL_NEED_DATA.|  
|h|Error. La función devolvió SQL_ERROR.|  
|i|Fila no válida. El cursor se colocó en una fila del conjunto de resultados y se eliminó la fila o se produjo un error en una operación de la fila. Si existe la matriz de estado de fila, el valor de la matriz de estado de fila de la fila se SQL_ROW_DELETED o SQL_ROW_ERROR. (El atributo de instrucción SQL_ATTR_ROW_STATUS_PTR apunta a la matriz de estado de fila).|  
|nf|Not found. La función devolvió SQL_NO_DATA. Esto no se aplica cuando **SQLExecDirect**, **SQLExecute** o **SQLParamData** devuelven SQL_NO_DATA después de ejecutar una instrucción UPDATE o DELETE buscada.|  
|np|No preparado. No se ha preparado la instrucción.|  
|nr|Especifica que no hay resultados. La instrucción no creará o no un conjunto de resultados.|  
|o|Otra función. Otra función se estaba ejecutando de forma asincrónica.|  
|p|Prepare. La instrucción se preparó.|  
|r|Resultados. La instrucción creará un conjunto de resultados (posiblemente vacío).|  
|s|Correcto. La función devolvió SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.|  
|v|Fila válida. El cursor está colocado en una fila del conjunto de resultados y la fila se ha insertado correctamente, se ha actualizado correctamente u otra operación de la fila se ha completado correctamente. Si existía la matriz de estado de fila, el valor de la matriz de estado de fila de la fila era SQL_ROW_ADDED, SQL_ROW_SUCCESS o SQL_ROW_UPDATED. (El atributo de instrucción SQL_ATTR_ROW_STATUS_PTR apunta a la matriz de estado de fila).|  
|x|En ejecución. La función devolvió SQL_STILL_EXECUTING.|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 En este ejemplo, la fila de la tabla de transición de estado del entorno para **SQLFreeHandle** cuando *HandleType* es SQL_HANDLE_ENV es como se indica a continuación.  
  
|E0<br /><br /> Sin asignar|E1<br /><br /> Allocated|E2<br /><br /> Conexión|  
|------------------------|----------------------|-----------------------|  
|ADMITIR|E0|HY010|  
  
 Si se llama a **SQLFreeHandle** en el estado del entorno E0 con *HandleType* establecido en SQL_HANDLE_ENV, el administrador de controladores devuelve SQL_INVALID_HANDLE. Si se llama en State E1 con *HandleType* establecido en SQL_HANDLE_ENV, el entorno pasa al estado E0 si la función se ejecuta correctamente y permanece en el estado E1 si se produce un error en la función. Si se llama en el estado E2 con *HandleType* establecido en SQL_HANDLE_ENV, el administrador de controladores siempre devuelve SQL_ERROR y SQLSTATE HY010 (error de secuencia de función) y el entorno permanece en el estado E2.  
  
 Para comprender las tablas de transición de estado, es necesario comprender a qué elemento (entorno, conexión, instrucción o descriptor) hacen referencia. Supongamos que una función acepta el identificador de un elemento de tipo X. La tabla de transición de estado X para esa función describe cómo la llamada a la función, con el identificador de un elemento de tipo X, afecta a ese elemento. Por ejemplo, **SQLDisconnect** acepta un identificador de conexión. La tabla de transición de estado de conexión para **SQLDisconnect** describe cómo **SQLDisconnect** afecta al estado de la conexión para la que se llama.  
  
 Suponga que una función acepta el identificador de un elemento de tipo Y, donde Y no es igual a X. La tabla de transición de estado X de esa función describe cómo llamar a la función, con un identificador de tipo X que está asociado al elemento de tipo Y, afecta al elemento de tipo Y. Por ejemplo, la tabla de transición de estado de instrucciones para **SQLDisconnect** describe cómo **SQLDisconnect** afecta al estado de una instrucción cuando se llama con el identificador de la conexión a la que está asociada la instrucción.  
  
 Este apéndice contiene los siguientes temas.  
  
-   [Transiciones de entorno](../../../odbc/reference/appendixes/environment-transitions.md)  
  
-   [Transiciones de conexión](../../../odbc/reference/appendixes/connection-transitions.md)  
  
-   [Transiciones de instrucción](../../../odbc/reference/appendixes/statement-transitions.md)  
  
-   [Transiciones de descriptor](../../../odbc/reference/appendixes/descriptor-transitions.md)
