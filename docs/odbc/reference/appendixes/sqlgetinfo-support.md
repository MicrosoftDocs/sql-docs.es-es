---
description: Compatibilidad con SQLGetInfo
title: Compatibilidad con SQLGetInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLGetInfo
- backward compatibility [ODBC], SQLGetInfo
- SQLGetInfo function [ODBC], support
ms.assetid: 57326f57-daba-46b6-b0be-6c97213b9ef1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dc92c6bd409829da13395b1286a5762e4504ced
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202746"
---
# <a name="sqlgetinfo-support"></a>Compatibilidad con SQLGetInfo
Cuando se trata de un ODBC 2. la aplicación *x* llama a **SQLGetInfo** a un controlador ODBC 3 *. x* , se deben admitir los argumentos *InfoType* de la tabla siguiente.  
  
|*InfoType*|Devoluciones|  
|----------------|-------------|  
|SQL_ALTER_TABLE (ODBC 2,0) **Nota:**  este tipo de información no está en desuso; las máscaras de la columna a la derecha están desusadas.|Máscara de SQLINTEGER donde que enumera las cláusulas de la instrucción **ALTER TABLE** que admite el origen de datos.<br /><br /> Las siguientes máscaras de código se usan para determinar qué cláusulas se admiten:<br /><br /> SQL_AT_DROP_COLUMN = se admite la posibilidad de quitar columnas. El hecho de que esto resulte en cascada o restringir el comportamiento está definido por el controlador. (ODBC 2,0)<br /><br /> SQL_AT_ADD_COLUMN = se admite la posibilidad de agregar varias columnas en una sola instrucción ALTER TABLE. Este bit no se combina con otros bits de SQL_AT_ADD_COLUMN_XXX o bits de SQL_AT_CONSTRAINT_XXX. (ODBC 2,0)|  
|SQL_FETCH_DIRECTION (ODBC 1,0)<br /><br /> El tipo de información se incorporó en ODBC 1,0; cada máscara de máscara tiene la etiqueta de la versión en la que se presentó.|Una máscara de SQLINTEGER donde que enumera las opciones de dirección de captura admitidas.<br /><br /> Las siguientes máscaras de código se usan junto con la marca para determinar qué opciones se admiten:<br /><br /> SQL_FD_FETCH_NEXT (ODBC 1,0) SQL_FD_FETCH_FIRST (ODBC 1,0) SQL_FD_FETCH_LAST (ODBC 1,0) SQL_FD_FETCH_PRIOR (ODBC 1,0) SQL_FD_FETCH_ABSOLUTE (ODBC 1,0) SQL_FD_FETCH_RELATIVE (ODBC 1,0) SQL_FD_FETCH_BOOKMARK (ODBC 2,0)|  
|SQL_LOCK_TYPES (ODBC 2,0)|Una máscara de SQLINTEGER donde que enumera los tipos de bloqueo admitidos para el argumento de *rebaño* en **SQLSetPos**.<br /><br /> Las siguientes máscaras de código se usan junto con la marca para determinar qué tipos de bloqueo se admiten:<br /><br /> SQL_LCK_NO_CHANGE SQL_LCK_EXCLUSIVE SQL_LCK_UNLOCK|  
|SQL_ODBC_API_CONFORMANCE (ODBC 1,0)|Un valor SQLSMALLINT que indica el nivel de conformidad con ODBC.<br /><br /> SQL_OAC_NONE = ninguno<br /><br /> SQL_OAC_LEVEL1 = nivel 1 admitido<br /><br /> SQL_OAC_LEVEL2 = nivel 2 compatible|  
|SQL_ODBC_SQL_CONFORMANCE (ODBC 1,0)|Un valor SQLSMALLINT que indica la gramática de SQL admitida por el controlador. Vea el [Apéndice C: gramática de SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md) para obtener una definición de los niveles de cumplimiento de SQL.<br /><br /> SQL_OSC_MINIMUM = gramática mínima admitida<br /><br /> SQL_OSC_CORE = gramática básica admitida<br /><br /> SQL_OSC_EXTENDED = gramática extendida compatible|  
|SQL_POS_OPERATIONS (ODBC 2,0)|Una máscara de SQLINTEGER donde que enumera las operaciones admitidas en **SQLSetPos**.<br /><br /> Las siguientes máscaras de código se utilizan junto con la marca para determinar qué opciones se admiten:<br /><br /> SQL_POS_POSITION (ODBC 2,0) SQL_POS_REFRESH (ODBC 2,0) SQL_POS_UPDATE (ODBC 2,0) SQL_POS_DELETE (ODBC 2,0) SQL_POS_ADD (ODBC 2,0)|  
|SQL_POSITIONED_STATEMENTS (ODBC 2,0)|Una máscara de SQLINTEGER donde que enumera las instrucciones SQL posicionadas admitidas.<br /><br /> Las siguientes máscaras de código se usan para determinar qué instrucciones se admiten:<br /><br /> SQL_PS_POSITIONED_DELETE SQL_PS_POSITIONED_UPDATE SQL_PS_SELECT_FOR_UPDATE|  
|SQL_SCROLL_CONCURRENCY (ODBC 1,0)|Máscara de SQLINTEGER donde que enumera las opciones de control de simultaneidad compatibles con el cursor.<br /><br /> Las siguientes máscaras de código se usan para determinar qué opciones se admiten:<br /><br /> SQL_SCCO_READ_ONLY = el cursor es de solo lectura. No se permiten actualizaciones.<br /><br /> SQL_SCCO_LOCK = cursor utiliza el nivel más bajo de bloqueo suficiente para asegurarse de que se puede actualizar la fila.<br /><br /> SQL_SCCO_OPT_ROWVER = cursor utiliza el control de simultaneidad optimista, comparando las versiones de fila, como SQLBase ROWID o Sybase TIMESTAMP.<br /><br /> SQL_SCCO_OPT_VALUES = cursor utiliza el control de simultaneidad optimista, comparando valores.|  
|SQL_STATIC_SENSITIVITY (ODBC 2,0)|Una máscara de bits SQLINTEGER donde que enumera si la aplicación puede detectar los cambios realizados por una aplicación en un cursor estático o controlado por conjunto de claves a través de instrucciones de actualización o de actualización por **posición o de** eliminación.<br /><br /> SQL_SS_ADDITIONS = las filas agregadas son visibles para el cursor; el cursor puede desplazarse hasta estas filas. El lugar donde se agregan estas filas al cursor depende del controlador.<br /><br /> SQL_SS_DELETIONS = las filas eliminadas ya no están disponibles para el cursor y no dejan un "agujero" en el conjunto de resultados; una vez que el cursor se desplaza de una fila eliminada, no puede volver a esa fila.<br /><br /> SQL_SS_UPDATES = las actualizaciones de las filas son visibles para el cursor; Si el cursor se desplaza desde y vuelve a una fila actualizada, los datos devueltos por el cursor son los datos actualizados, no los datos originales. Esta opción solo se aplica a los cursores estáticos o a las actualizaciones de cursores controlados por conjunto de claves que no actualizan la clave. Esta opción no se aplica a un cursor dinámico o en el caso en el que se cambia una clave en un cursor mixto.<br /><br /> Si una aplicación puede detectar los cambios realizados en el conjunto de resultados por otros usuarios, incluidos otros cursores de la misma aplicación, depende del tipo de cursor.|  
  
 Una aplicación ODBC 3 *. x* que funcione con un controlador ODBC 3 *. x* no debe llamar a **SQLGetInfo** con los argumentos *InfoType* descritos en la tabla anterior, pero debe usar los argumentos *InfoType* de ODBC 3 *. x* que se muestran en el párrafo siguiente. No hay una correspondencia uno a uno entre los argumentos de *InfoType* usados en ODBC 2. *x* y los que se usan en ODBC 3 *. x*. Una aplicación ODBC 3 *. x* que funciona con ODBC 2. por otro lado, el controlador *x* debe usar los argumentos *InfoType* descritos anteriormente.  
  
 Algunos de los tipos de información de la tabla anterior están en desuso en favor de los tipos de información de atributos de cursor. Estos tipos de información en desuso son SQL_FETCH_DIRECTION, SQL_LOCK_TYPES, SQL_POS_OPERATIONS, SQL_POSITIONED_STATEMENTS, SQL_SCROLL_CONCURRENCY y SQL_STATIC_SENSITIVITY. Los nuevos tipos de atributos de cursor se SQL_XXX_CURSOR_ATTRIBUTES1and SQL_XXX_CURSOR_ATTRIBUTES2, donde XXX es igual a DYNAMIC, FORWARD_ONLY, KEYSET_DRIVEN o STATIC. Cada uno de los nuevos tipos indica las capacidades del controlador para un solo tipo de cursor. Para obtener más información sobre estas opciones, vea la descripción de la función [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) .
