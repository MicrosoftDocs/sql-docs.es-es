---
description: sp_cursoropen (Transact-SQL)
title: sp_cursoropen (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoropen
- sp_cursoropen_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoropen
ms.assetid: 16462ede-4393-4293-a598-ca88c48ca70b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1e8e4025a31c92ec001bcb45bee4f8d26f3dc24e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206275"
---
# <a name="sp_cursoropen-transact-sql"></a>sp_cursoropen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Abre un cursor. sp_cursoropen define la instrucción SQL asociada a las opciones cursor y cursor y, a continuación, rellena el cursor. sp_cursoropen es equivalente a la combinación de las [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones DECLARE_CURSOR y Open. Este procedimiento se invoca especificando el identificador 2 en un paquete de flujo TDS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursoropen cursor OUTPUT, stmt  
    [, scrollopt[ OUTPUT ] [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,boundparam][,...n]]] ]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *cursor*  
 Un identificador del cursor generado por SQL Server. *cursor* es un valor de *identificador* que se debe proporcionar en todos los procedimientos subsiguientes que impliquen el cursor, como sp_cursorfetch. *cursor* es un parámetro necesario con un valor devuelto **int** .  
  
 *cursor* permite que varios cursores estén activos en una sola conexión de base de datos.  
  
 *instrucción*  
 Es un parámetro necesario que define el conjunto de resultados del cursor. Cualquier cadena de consulta válida (sintaxis y enlace) de cualquier tipo de cadena (independientemente de Unicode, tamaño, etc.) puede actuar como un tipo de valor *stmt* válido.  
  
 *scrollopt*  
 Opción de desplazamiento. *scrollopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 Debido a la posibilidad de que el valor solicitado no sea adecuado para el cursor definido por *stmt*, este parámetro actúa como entrada y salida. En casos como este, SQL Server asigna un valor adecuado.  
  
 *ccopt*  
 Opción de control de simultaneidad. *ccopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (conocido anteriormente como LOCKCC)|  
|0x0004|**Optimista** (conocido anteriormente como OPTCC)|  
|0x0008|OPTIMISTIC (conocido anteriormente como OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Al igual que con *scrollopt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede invalidar los valores de *ccopt* solicitados.  
  
 *empleado*  
 Número de filas del búfer de captura que se usan con AUTO_FETCH. El valor predeterminado es 20 filas. *RowCount* se comporta de forma diferente cuando se asigna como valor de entrada frente a un valor devuelto.  
  
|Como valor de entrada|Como valor devuelto|  
|--------------------|---------------------|  
|Cuando se especifica el valor de *scrollopt* AUTO_FETCH, *RowCount* representa el número de filas que se van a colocar en el búfer de captura.<br /><br /> Nota: >0 es un valor válido cuando se especifica AUTO_FETCH, pero se omite de otro modo.|Representa el número de filas del conjunto de resultados, excepto cuando se especifica el valor AUTO_FETCH de *scrollopt* .|  
  
-  
  
 *boundparam*  
 Indica el uso de parámetros adicionales. *boundparam* es un parámetro opcional que se debe especificar si el valor de la PARAMETERIZED_STMT *scrollopt* está establecido en on.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Si no se produce ningún error, sp_cursoropen devuelve uno de los siguientes valores.  
  
 0  
 El procedimiento se ejecutó correctamente.  
  
 0x0001  
 Se produjo un error durante la ejecución (un error secundario, no lo suficientemente grave como para generar un error en la operación).  
  
 0x0002  
 Está en curso una operación asincrónica.  
  
 0x0002  
 Está en curso una operación FETCH.  
  
 A  
 Este cursor se ha desasignado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no está disponible.  
  
 Cuando se produce un error, los valores devueltos pueden ser incoherentes y no se puede garantizar la exactitud.  
  
 Cuando el parámetro *RowCount* se especifica como un valor devuelto, se produce el siguiente conjunto de resultados.  
  
 -1  
 Se devuelve si el número de filas es desconocido o no es aplicable.  
  
 -n  
 Se devuelve cuando está en vigor un rellenado asincrónico. Representa el número de filas que se colocaron en el búfer de captura cuando se especifica el valor AUTO_FETCH de *scrollopt* .  
  
 Si se usa una RPC, los valores devueltos son los siguientes.  
  
 0  
 El procedimiento es correcto.  
  
 1  
 Se produjo un error en el procedimiento.  
  
 2  
 Se genera un cursor de conjunto de claves de forma asincrónica.  
  
 16  
 Se ha cerrado automáticamente un cursor de avance rápido.  
  
> [!NOTE]  
>  Si el procedimiento de sp_cursoropen se ejecuta correctamente, se envían los parámetros devueltos por RPC y un conjunto de resultados con información de formato de columna TDS (0XA0 & mensajes 0xa1). Si no puede ejecutarse se envían uno o varios mensajes de error TDS. En cualquier caso, no se devolverá ningún dato de fila y el número de mensajes *terminados* será cero. Si usa una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior a la 7.0, se devuelven 0xa0, 0xa1 (estándar de las instrucciones SELECT) junto con las flujos de token 0xa5 y 0xa4. Si usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, se devuelve 0x81 (estándar para las instrucciones SELECT) junto con las flujos de token 0xa5 y 0xa4.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="stmt-parameter"></a>Parámetro stmt  
 Si *stmt* especifica la ejecución de un procedimiento almacenado, los parámetros de entrada pueden definirse como constantes como parte de la cadena de *stmt* o especificarse como argumentos de *boundparam* . Las variables declaradas se pueden pasar como parámetros enlazados de esta manera.  
  
 El contenido permitido del parámetro *stmt* depende de si el valor devuelto de *ccopt* ALLOW_DIRECT se ha vinculado o no al resto de los valores de *ccopt* , es decir:  
  
-   Si no se especifica ALLOW_DIRECT, [!INCLUDE[tsql](../../includes/tsql-md.md)] se debe usar una instrucción SELECT o EXECUTE que llame a para un procedimiento almacenado que contenga una sola instrucción SELECT. Además, la instrucción SELECT debe calificarse como un cursor; es decir, no puede contener las palabras clave SELECT INTO o FOR BROWSE.  
  
-   Si se especifica ALLOW_DIRECT, esto puede dar como resultado una o más instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], incluso aquellas que, a su vez, ejecuten otros procedimientos almacenados con varias instrucciones. Las instrucciones que no sean SELECT o cualquier instrucción SELECT que contenga las palabras clave SELECT INTO o FOR BROWSE simplemente se ejecutarán y no darán como resultado la creación de un cursor. Lo mismo puede decirse de cualquier instrucción SELECT incluida en un lote de varias instrucciones. En los casos en los que una instrucción SELECT contenga cláusulas que solo sean relativas a los cursores, dichas cláusulas se omiten. Por ejemplo, cuando el valor de *ccopt* es 0x2002, se trata de una solicitud de:  
  
    -   Un cursor con bloqueos de desplazamiento, si solo hay una instrucción SELECT que se califica como un cursor, o  
  
    -   La ejecución de una instrucción directa si hay varias instrucciones, una sola instrucción distinta de SELECT o una instrucción SELECT que no se califica como un cursor.  
  
## <a name="scrollopt-parameter"></a>Parámetro scrollopt  
 Los cinco primeros valores de *scrollopt* (KEYSEY, DYNAMIC, FORWARD_ONLY, STATIC y FAST_FORWARD) son mutuamente excluyentes.  
  
 PARAMETERIZED_STMT y CHECK_ACCEPTED_TYPES se pueden vincular mediante OR a cualquiera de los primeros cinco valores.  
  
 AUTO_FETCH y AUTO_CLOSE se pueden vincular mediante OR a FAST_FORWARD.  
  
 Si CHECK_ACCEPTED_TYPES está activada, al menos uno de los cinco últimos valores de *scrollopt* (KEYSET_ACCEPTABLE `,` DYNAMIC_ACCEPTABLE, FORWARD_ONLY_ACCEPTABLE, STATIC_ACCEPTABLE o FAST_FORWARD_ACCEPTABLE) también debe estar activado.  
  
 Los cursores STATIC se abren siempre como READ_ONLY. Esto significa que la tabla subyacente no se puede actualizar a través de este cursor.  
  
## <a name="ccopt-parameter"></a>Parámetro ccopt  
 Los primeros cuatro valores de *ccopt* (READ_ONLY, SCROLL_LOCKS y ambos valores optimistas) son mutuamente excluyentes.  
  
> [!NOTE]  
>  Al elegir uno de los cuatro primeros valores de *ccopt* se determina si el cursor es de solo lectura, o si se usan métodos optimistas o de bloqueo para evitar las actualizaciones perdidas. Si no se especifica un valor de *ccopt* , el valor predeterminado es optimistic.  
  
 ALLOW_DIRECT y CHECK_ACCEPTED_TYPES se pueden vincular mediante OR a cualquiera de los primeros cuatro valores.  
  
 UPDT_IN_PLACE se puede vincular mediante OR a READ_ONLY, SCROLL_LOCKS o a cualquiera de los valores OPTIMISTIC.  
  
 Si CHECK_ACCEPTED_TYPES está activada, al menos uno de los cuatro últimos valores de *ccopt* (READ_ONLY_ACCEPTABLE, SCROLL_LOCKS_ACCEPTABLE y cualquiera de los valores de OPTIMISTIC_ACCEPTABLE) también debe estar activado.  
  
 Las funciones de actualización y eliminación posicionadas solo se pueden realizar dentro del búfer de captura y solo si el valor de *ccopt* es igual a SCROLL_LOCKS o optimista. Si SCROLL_LOCKS es el valor especificado, se garantiza que la operación se realiza correctamente. Si el valor especificado es OPTIMISTIC, se producirá un error en la operación si la fila ha cambiado desde que se capturó en último lugar.  
  
 La razón de este error es que cuando el valor especificado es OPTIMISTIC, una función de control de simultaneidad optimista se realiza comparando las marcas de tiempo o los valores de las sumas de comprobación, según determina [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si alguna de estas filas no coincide, se producirá un error en la operación.  
  
 Especificar UPDT_IN_PLACE como valor devuelto produce los siguientes resultados:  
  
-   Si no está establecido al realizar una actualización colocada en una tabla con un único índice, el cursor elimina la fila de su tabla de trabajo y la inserta al final de alguna de las columnas de clave que usa el cursor, con lo que las cambia.  
  
-   Si está establecida en ON, el cursor simplemente actualizará las columnas de clave en la fila original de la tabla de trabajo.  
  
## <a name="bound_param-parameter"></a>Parámetro bound_param  
 El nombre del parámetro debe ser *ParamDef* cuando se especifica PARAMETERIZED_STMT, de acuerdo con el mensaje de error del código. Cuando no se especifica PARAMETERIZED_STMT, no se especifica ningún nombre en el mensaje de error.  
  
## <a name="rpc-considerations"></a>Consideraciones sobre RPC  
 La marca de entrada RPC RETURN_METADATA puede establecerse en 0x0001 para solicitar que se devuelvan los metadatos de la lista de selección del cursor en el flujo de TDS.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="bound_param-parameter"></a>Parámetro bound_param  
 Cualquier parámetro después del quinto se pasa como parámetro de entrada al plan de instrucción. El primer parámetro debe ser una cadena con el formato:  
  
 *{tipo de datos de nombre de variable local} [,... n*  
  
 Los parámetros subsiguientes se utilizan para pasar los valores que se van a sustituir por el nombre de la *variable local* en la instrucción.  
  
## <a name="see-also"></a>Consulte también  
 [sp_cursorfetch &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
